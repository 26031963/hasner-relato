# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 14:30** · publicado em **11/11/2026 14:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 14:30 · sessao c243052975dc · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**NÃO PROCEDE declarar o caminho inteiro fechado.** A cura limita `/hora/` e corrige a prova do aviso/controle dos mutantes; permanecem esperas anteriores à fila e ausência de uma identidade estável para o reenvio. (`app/static/pwa.js:93`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/static/pwa.js:48`)

Chromium, mortes efetivamente executadas, smoke e efeitos em produção: **[nao verificado]**. Não executei o navegador: seu executor cria arquivos e perfil temporários. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:279`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:286`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:295`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:298`)

1. **Fechamento de cada achado da volta 2, por inspeção.**

Na tabela, o veredito qualifica **o fechamento** do achado.

| Achado da volta 2 | Veredito, cura e teste correspondente | [nao verificado]
|---|---|
| Triagem offline escondia confirmar quando a câmera falhava | **PROCEDE.** Botão mostrado antes da abertura e preservado sem stream; M1 reinjeta a ocultação. (`app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:262`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:550`) |
| Triagem online presa em câmera pendente | **PROCEDE.** Abertura limitada, seguida da liberação sem stream; matriz inclui câmera pendente, M3 retira a liberação e Md retira o teto. (`app/static/pwa.js:159`, `app/static/pwa.js:162`; `app/templates/ponto/triagem/apto.html:164`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:55`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:560`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:626`) |
| Triagem online dependia exclusivamente de `toBlob` | **PROCEDE.** Captura central limitada, aguardada antes do submit; matriz e teste direto cobrem callback mudo/conversão falha. (`app/templates/ponto/triagem/apto.html:177`, `app/templates/ponto/triagem/apto.html:231`; `app/static/pwa.js:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:59`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`) |
| Triagem offline enfileirava somente dentro do callback | **PROCEDE.** Captura termina antes da chamada independente ao escritor; M4 reinjeta callback sem teto. (`app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:565`) |
| V2 vetava foto nula | **PROCEDE.** Requisitos avisam e prosseguem; M2 reinjeta o veto. (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:298`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:555`) |
| V2 pendurava com `toBlob` mudo e `toDataURL` lançando | **PROCEDE.** Exceção do fallback é absorvida pelo teto; cenário específico e Md cobrem o contrato. (`app/static/pwa.js:138`, `app/static/pwa.js:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:60`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:626`) |
| Barreiras no template legado | **PROCEDE por eliminação.** Seleção atual usa V2/triagem; M6 permanece apenas como histórico, sem ensaio comportamental próprio. (`app/ponto/views.py:356`, `app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:524`) |
| Captura/espera duplicadas nas telas | **PROCEDE para a operação da câmera.** A operação fica no PWA; censo exige lista fechada de produtores. Os textos próprios continuam, abaixo. (`app/static/pwa.js:157`, `app/static/pwa.js:168`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:641`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:661`) |
| Mutante com `await` infinito em requisitos | **PROCEDE quanto à prova prevista.** Ma injeta exatamente a espera; agora passa por controle acoplado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:595`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:545`) |
| Mutante sem aviso | **PROCEDE quanto à prova prevista.** Mb troca o aviso por `false` e exige sua falta na tela. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:600`) |
| `throw` no ramo sem foto | **PROCEDE quanto à prova prevista.** Mc reinjeta a exceção e exige ausência de envio. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:605`) |
| PWA não carregou | **NÃO PROCEDE o fechamento offline.** Ambas as telas dependem do escritor ausente; mostram erro sem gravação alternativa. O teste sem PWA continua somente online. (`app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:332`, `app/templates/ponto/bater_ponto_v2.html:338`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:465`) |
| `estadoPermissao` ainda participava do fluxo | **PROCEDE a retirada do fluxo examinado.** Requisitos consultam captura e GPS diretamente. (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:279`) |
| Transporte incompleto de timestamp/GPS | **NÃO PROCEDE.** Sync continua omitindo timestamp corrigido, gap, precisão e instante GPS. (`app/static/pwa.js:188`, `app/static/pwa.js:191`, `app/static/pwa.js:217`, `app/static/pwa.js:226`) |
| Textos de obrigatoriedade contraditórios | **NÃO PROCEDE.** Permanecem na triagem e na V2; os testes procuram o aviso, sem proibir essas mensagens contraditórias. (`app/templates/ponto/triagem/apto.html:66`, `app/templates/ponto/triagem/apto.html:124`, `app/templates/ponto/triagem/apto.html:194`, `app/templates/ponto/triagem/apto.html:250`; `app/templates/ponto/bater_ponto_v2.html:100`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:338`) |
| B1: `/hora/` pendurada antes do `add` | **PROCEDE o fechamento específico da espera HTTP/corpo.** `_noTeto` envolve ambos; FilaRealTest e Me exercitam a fila pelo escritor real. **Não fecha a espera total**, pois IndexedDB permanece sem teto. (`app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:113`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:501`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:610`) |
| B2: aviso invisível em atributo | **PROCEDE.** V2 acrescenta texto; oráculo usa `innerText`; Mf recoloca somente `title`. (`app/templates/ponto/bater_ponto_v2.html:250`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:621`) |
| B2: mutante morto sem controle verde acoplado | **PROCEDE.** `par` junta controle/mutante, exige controle verde/não vazio e exclui sonda sem veredito das mortes. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:451`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:453`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:455`) |

2. **PROCEDE a existência de caminhos sem teto antes da gravação.**

| Caminho | O que acontece antes da fila |
|---|---|
| Triagem online | Clique desabilita botão → GPS com timer de 8 s → selfie limitada → espera de 400 ms → submit nativo. Esse ramo **não possui fallback para a fila** se o envio falhar ou ficar pendente. (`app/templates/ponto/triagem/apto.html:212`, `app/templates/ponto/triagem/apto.html:218`, `app/templates/ponto/triagem/apto.html:231`, `app/templates/ponto/triagem/apto.html:240`, `app/templates/ponto/triagem/apto.html:241`) |
| Triagem offline | Primeiro clique abre confirmação; segundo clique → captura limitada → escritor da fila. Falha de câmera não impede essa chamada; falha do escritor mostra erro, sem gravação alternativa. (`app/templates/ponto/triagem/apto.html:243`, `app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:308`) |
| V2 offline | Clique → selfie limitada → GPS com tentativas de 10 e 12 s → escritor. Esses timers de GPS são próprios, além do teto de selfie/hora. (`app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:280`, `app/templates/ponto/bater_ponto_v2.html:281`, `app/templates/ponto/bater_ponto_v2.html:332`) |
| V2 online | Após requisitos, espera `fetch` **sem teto**. Somente rejeição chega ao fallback da fila. HTTP de erro espera `r.json()` também sem teto e, depois, retorna sem enfileirar. (`app/templates/ponto/bater_ponto_v2.html:298`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:314`, `app/templates/ponto/bater_ponto_v2.html:316`, `app/templates/ponto/bater_ponto_v2.html:319`) |
| Escritor PWA | `obterGap` → leitura de config → possível medição limitada → leitura/gravação de config → nova abertura do banco → `add` → `tx.oncomplete`. O teto HTTP não engloba essas operações de armazenamento. (`app/static/pwa.js:104`, `app/static/pwa.js:108`, `app/static/pwa.js:113`, `app/static/pwa.js:182`, `app/static/pwa.js:184`, `app/static/pwa.js:194`) |

**`abrirDB` continua sendo uma promessa potencialmente pendente:** resolve em `onsuccess`, rejeita em `onerror`; não possui `onblocked`, teto ou fechamento das conexões abertas. Se a abertura não chegar a nenhum desses dois eventos, `obterGap` já fica esperando antes de alcançar o teto HTTP. A transação da fila também não possui teto nem tratamento explícito de `onabort`. Ocorrência real dessas condições: **[nao verificado]**. (`app/static/pwa.js:48`, `app/static/pwa.js:58`, `app/static/pwa.js:59`, `app/static/pwa.js:74`, `app/static/pwa.js:113`, `app/static/pwa.js:185`, `app/static/pwa.js:195`)

**POST recebido, resposta perdida:**

- Se o `fetch` permanecer pendente, **não entra na fila**; o botão permanece desabilitado. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:295`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:317`)
- Se acabar rejeitando, **tenta entrar na fila**, com timestamp produzido naquele momento. O POST inicial não leva timestamp nem ID estável do clique. (`app/templates/ponto/bater_ponto_v2.html:102`, `app/templates/ponto/bater_ponto_v2.html:304`, `app/templates/ponto/bater_ponto_v2.html:319`; `app/static/pwa.js:182`, `app/static/pwa.js:189`)
- **Não procede “sempre duplica” nem “nunca duplica”.** As APIs deduplicam por **colaborador + tipo + timestamp efetivo em ±5 minutos**. A porta acrescenta mesmo tipo em ±60 segundos e igualdade exata **colaborador + timestamp**; essa última também é a constraint do modelo. Nenhuma é chave do clique original. (`app/api/views.py:539`, `app/api/views.py:542`, `app/api/views_core.py:526`, `app/api/views_core.py:529`; `app/ponto/registro_batida.py:106`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`; `app/ponto/models.py:106`)

Logo, demora antes da rejeição ou relógio desencontrado pode colocar o novo instante fora dessas janelas; o mesmo evento deixa de ser identificado pelo dedup. Duplicação observada em produção: **[nao verificado]**. (`app/static/pwa.js:183`, `app/static/pwa.js:189`; `app/api/views.py:539`; `app/ponto/registro_batida.py:120`)

3. **NÃO PROCEDE confundir os tetos com o gap; PROCEDE a diferença no instante gravado.**

| Valor | Significado, escritor e leitores |
|---|---|
| `HORA_TETO_MS` | Duração de **2000 ms**, declarada uma vez; `calcularESalvarGap` a passa para `_noTeto`, abrangendo fetch e corpo. (`app/static/pwa.js:88`, `app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:98`) |
| `SELFIE_TETO_MS` | Duração **base de 1500 ms**, declarada uma vez; captura lê diretamente e abertura lê multiplicada por dois. Portanto, não é um único teto absoluto para as duas operações. (`app/static/pwa.js:129`, `app/static/pwa.js:162`, `app/static/pwa.js:177`) |
| `timestamp_gap_ms` / `ntp_gap` | Offset assinado em milissegundos: servidor menos estimativa do relógio local. Não é duração de espera nem horário com fuso. (`app/static/pwa.js:101`, `app/static/pwa.js:102`, `app/static/pwa.js:103`, `app/static/pwa.js:190`; `app/ponto/models.py:76`) |

**No teto não se guarda um novo gap:** o código lê o anterior ou usa zero; também não renova `ntp_calculado_em`. Esse é o mesmo fallback da rejeição imediata de rede. Entretanto, `Date.now()` da batida só é lido **depois** dessa espera: `/hora/` pendurada desloca o instante amostrado pelo tempo esperado, aproximadamente 2 s no modelo dos timers. Não há identidade com o instante do clique nem com a execução que rejeitou imediatamente. (`app/static/pwa.js:98`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:182`, `app/static/pwa.js:183`)

**NÃO PROCEDE atribuir outra unidade/fuso ao servidor nesse caminho:** o sincronizador sequer transmite o gap ou o timestamp corrigido. Usa `b.timestamp` para ordenar e envia `timestamp_dispositivo` bruto. A API usada é `/api/ponto/bater/`; ambas as cascas entregam esse bruto a `hora_do_aparelho`. O endpoint offline antigo retorna 410. (`app/static/pwa.js:211`, `app/static/pwa.js:221`, `app/static/pwa.js:227`; `app/api/views.py:518`, `app/api/views_core.py:505`; `app/ponto/views.py:419`, `app/ponto/views.py:426`)

`hora_do_aparelho` aceita ISO ou epoch segundos/ms e decide entre instante bruto e chegada. Os leitores encontrados de `timestamp_gap_ms` mantêm milissegundos: modelo, tela e alerta de skew do **ping geo**, sem aplicação de fuso ao offset. O problema aqui é **transporte omitido**, não conversão nova de unidade. (`app/api/hora_do_aparelho.py:79`, `app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`; `app/ponto/models.py:76`; `app/templates/ponto/batidas_offline_pendentes.html:79`, `app/templates/ponto/batidas_offline_pendentes.html:80`; `app/api/views.py:2340`, `app/api/views.py:2371`)

4. **PROCEDE a mordida específica dos novos testes; NÃO PROCEDE tratá-los como prova de persistência real.** [nao verificado]

| Prova | Avaliação |
|---|---|
| FilaRealTest | Usa o escritor real e testa hora normal, fetch pendente e corpo pendente. Corrige a substituição que escondia B1. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:214`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:437`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:477`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:501`) |
| Me | `1e9` impede o teto dentro dos 15 s do ensaio; exige ausência de fila diretamente e nas duas telas offline, com controle verde acoplado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:439`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:610`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:616`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:618`) |
| Stub que ignora abort | Não dá passagem indevida: obriga `_noTeto` a resolver independentemente do abort. A promise HTTP original pode continuar pendente sem segurar o chamador limitado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:116`; `app/static/pwa.js:138`, `app/static/pwa.js:140`) |
| Mf | `title` não satisfaz `innerText`. Para câmera rejeitada, o aviso precisa estar presente antes do clique; o toast posterior não salva o mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:190`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:342`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:621`) |
| `_Base.par` | Controle e mutante são processos separados submetidos ao mesmo lote; controle vermelho impede declarar morte, e sonda sem veredito é filtrada. Promessa pendente **com veredito da sonda** continua sendo falha válida. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:298`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:318`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:351`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:455`) |

**Há um caso que o selo aceita sem provar a conclusão:** o fake marca fila dentro do `add`, antes de `tx.oncomplete`; `falhas_da_fila` não exige “fila resolveu”. Um escritor pendurado depois desse `add` satisfaria o oráculo, embora a função real ainda estivesse aguardando conclusão. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:135`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:151`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:215`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:484`; `app/static/pwa.js:194`)

O fake sempre abre com sucesso e não reproduz bloqueio, aborto ou rollback; os timers rodam em tempo virtual. O caso online novo injeta rejeição imediata, sem representar POST pendurado ou servidor que gravou antes da rejeição. Persistência nativa e comportamento em tempo real: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:109`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:160`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:173`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:299`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:506`)

A ressalva do frame “ok” permanece: stream vazio, dimensões falsificadas e `drawImage` real. Se isso falhar, agora o controle vermelho impede uma falsa morte; sucesso efetivo desse controle: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:80`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:87`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:452`; `app/static/pwa.js:172`)

5. **Escritores, leitores e autoridades.**

Além dos dois tetos descritos no item 3, os contratos relevantes são:

| Campo/valor | Quem escreve → quem lê |
|---|---|
| `/hora/`: `hora`, `data`, `timestamp` | `hora_servidor` produz strings de apresentação e ISO com offset → PWA usa timestamp para epoch-ms; relógios das telas usam hora/data. (`app/core/views.py:14`, `app/core/views.py:20`; `app/static/pwa.js:102`; `app/templates/ponto/bater_ponto_v2.html:169`; `app/templates/ponto/triagem/base_triagem.html:46`) |
| `t0`, `t1`, `medida`, `latencia`, `gap` | Medição PWA captura relógios/corpo e deriva um offset → grava config e devolve ao escritor. (`app/static/pwa.js:94`, `app/static/pwa.js:96`, `app/static/pwa.js:97`, `app/static/pwa.js:101`, `app/static/pwa.js:104`, `app/static/pwa.js:182`) |
| `ntp_gap`, `ntp_calculado_em` | `calcularESalvarGap`, via `salvarConfig`, escreve → `obterGap` lê validade de 30 min; fallback lê gap anterior. (`app/static/pwa.js:67`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:113`) |
| `{stream, aviso}` / `{foto, aviso}` | Operação central e `_respostaSelfie` produzem → triagem/V2 leem controles, foto e aviso. (`app/static/pwa.js:145`, `app/static/pwa.js:163`, `app/static/pwa.js:178`; `app/templates/ponto/triagem/apto.html:155`, `app/templates/ponto/triagem/apto.html:233`; `app/templates/ponto/bater_ponto_v2.html:250`, `app/templates/ponto/bater_ponto_v2.html:275`) |
| `_fotoBlob` / foto anterior | Input V2 e captura escrevem → captura preserva anterior; POST/fila transportam; sync anexa somente se presente. (`app/templates/ponto/bater_ponto_v2.html:255`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:306`, `app/templates/ponto/bater_ponto_v2.html:332`; `app/static/pwa.js:178`, `app/static/pwa.js:226`) |
| Registro IndexedDB | `registrarBatidaOffline` escreve tipo, horários, gap, coordenadas, precisão, instante GPS e foto → sync lê; horário corrigido apenas ordena, e três metadados não são transportados. (`app/static/pwa.js:187`, `app/static/pwa.js:211`, `app/static/pwa.js:217`, `app/static/pwa.js:226`) |
| `id` da fila | IndexedDB gera por autoincremento → sync usa para apagar localmente; não vira ID enviado ao servidor. (`app/static/pwa.js:54`, `app/static/pwa.js:217`, `app/static/pwa.js:236`) |
| `C.hora`, `C.post`, `C.fim` | Construtor de cenário escreve → fetch fake seleciona resposta/rejeição e timer emite veredito. (`app/ponto/tests/test_se

[truncado em 20000 de 24229 caracteres]

## 11/11 14:29 · sessao 9ba71d54c610 · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE — alternativa (a), pela leitura literal de que o RED deve evidenciar o defeito pelo selo definido no aval.** O aval exige painel renderizado × API para o mesmo colaborador; a PROPORCAO mantém “RED e GREEN”; a LEI-AKITA 5 determina: “Cura sem teste vermelho evidenciado nao existe”. (`aval_ui_painel_raspado.txt:10–11`; `aval_proporcao_por_risco_1111.txt:1`, item 2; `CLAUDE.md:28–29`). [nao verificado]

- **Não há evidência fornecida do ORM vermelho pelo defeito da obra antes da cura.** A docstring registra cinco falhas **em memória**, mas declara que esse selo não certifica ORM e que o “RED visual/ORM antes da cura NAO [foi] executado nesta copia”. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:3–11`).
- O único log ORM mostra quatro falhas por **“fixture invisivel”**, inclusive com mutante. No código, essa asserção está no `setUp`, antes da instalação do mutante e da renderização; o comentário atribui a ela os quatro RED v1. Isso evidencia falha de preparação, não ausência do rótulo/lista. (`v1_orm_falhas.txt:13–31`; `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:58–61`, `:93–105`).
- A L-006 explicita o precedente: “RED que nao se le nao vale como evidencia (LEI-AKITA 5)”, com reconstrução contra o código anterior. (`app/docs/LEIS.md:64`).
- O corpo do commit `984ec6938` ficou **[não verificado]**: `git log` não pôde consultá-lo nesta cópia sem repositório Git. **[não verificado]**

**A alternativa (b) não cabe nessa leitura:** a fila cobre “como o selo poderia ser enganado”; aqui falta comprovar o RED obrigatório do selo contratado. O vermelho em memória não o substitui, conforme a própria docstring. (`aval_proporcao_por_risco_1111.txt:1`, item 2; `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:3–4`).

*Referências aos anexos contam linhas desde a primeira linha do conteúdo fornecido.*

## 11/11 14:25 · sessao 0be98c4a67e3 · - · - · origem=Ronald (tmux)

**Servidor, timestamps e reenvio**

- **PROCEDE: POST online pode ficar indefinidamente antes da fila.** V2 espera `fetch` sem teto; só entra na fila no `catch`. Resposta pendente não executa o `catch`, e `_enviando`/botão permanecem bloqueados. HTTP de erro vai para `r.json()`, também sem teto, e depois retorna sem enfileirar. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:314`, `app/templates/ponto/bater_ponto_v2.html:317`)
- **Se POST recebido terminar em rejeição no cliente**, V2 enfileira; se apenas permanecer pendente, não enfileira. A fila calcula novo instante depois de `obterGap`; o POST inicial não carrega instante nem identificador estável do evento: o formulário tem tipo/coordenadas/foto e o envio só acrescenta precisão/foto. Portanto, a mesma tentativa não preserva uma chave de idempotência de ponta a ponta. (`app/templates/ponto/bater_ponto_v2.html:102`, `app/templates/ponto/bater_ponto_v2.html:304`, `app/templates/ponto/bater_ponto_v2.html:319`, `app/static/pwa.js:182`)
- **Não é correto dizer “sempre duplica” ou “nunca duplica”.** O dedup API procura `(colaborador, tipo)` com timestamp em ±5 minutos do instante efetivo e retorna `ja_registrada`; as duas cascas fazem isso. É proximidade temporal, não igualdade por ID do clique. Se a nova hora cair fora da janela — por demora antes de rejeitar ou relógio bruto desencontrado — essa primeira barreira não identifica a mesma tentativa. Incidência real: **[nao verificado]**. (`app/api/views.py:539`, `app/api/views.py:541`, `app/api/views.py:549`, `app/api/views_core.py:526`, `app/api/views_core.py:528`; `app/static/pwa.js:183`, `app/static/pwa.js:189`)
- A porta oferece mais duas barreiras: mesmo colaborador/tipo não retratado em ±60 s; depois igualdade exata `(colaborador, timestamp)`, inclusive outro tipo ou retratada. Colisão de tipo oposto usa 60 ou 300 s, salvo marco próprio; a constraint do modelo é `(colaborador, timestamp)`. Nenhuma dessas chaves é o ID do clique original. (`app/ponto/registro_batida.py:21`, `app/ponto/registro_batida.py:106`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:138`, `app/ponto/models.py:106`)

**Gap, unidades e leitores**

- `HORA_TETO_MS` é duração de espera em milissegundos, escrita uma vez em 2000 e lida na chamada `_noTeto` que engloba fetch **e** `json()`. Não é o offset `timestamp_gap_ms`. (`app/static/pwa.js:88`, `app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:98`)
- `SELFIE_TETO_MS` é duração base de 1500 ms; abertura lê multiplicada por dois, captura lê diretamente. O mecanismo `_noTeto` resolve timeout/erro, sem depender de o abort encerrar a promise original. (`app/static/pwa.js:129`, `app/static/pwa.js:134`, `app/static/pwa.js:138`, `app/static/pwa.js:140`, `app/static/pwa.js:162`, `app/static/pwa.js:177`)
- No teto da hora **não se grava um novo gap**: cai no `catch` e lê `ntp_gap || 0`. A fórmula corrigida permanece `Date.now()+gap`; assim, não surge conversão nova de unidade/fuso, mas uma espera de até 2 s antes da leitura do instante desloca a hora gravada frente à rejeição imediata sem rede. Não se preserva a hora do clique antes da espera. (`app/static/pwa.js:99`, `app/static/pwa.js:104`, `app/static/pwa.js:108`, `app/static/pwa.js:182`, `app/static/pwa.js:183`)
- Escritor dos configs: `calcularESalvarGap`, via `salvarConfig`, grava `ntp_gap` como diferença de epoch-ms e `ntp_calculado_em` como epoch-ms local. Leitores: `obterGap` decide validade de 30 min e recupera offset; fallback da própria medição recupera o offset. (`app/static/pwa.js:63`, `app/static/pwa.js:101`, `app/static/pwa.js:103`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:112`)
- **PROCEDE, transporte continua incompleto.** Fila grava timestamp ISO corrigido, timestamp ISO bruto, gap-ms, precisão e instante GPS; sync usa `b.timestamp` só para ordenação e transmite apenas tipo/coordenadas/timestamp bruto/foto/liveness, omitindo corrigido/gap/precisão/instante GPS. (`app/static/pwa.js:188`, `app/static/pwa.js:189`, `app/static/pwa.js:190`, `app/static/pwa.js:191`, `app/static/pwa.js:211`, `app/static/pwa.js:217`, `app/static/pwa.js:226`)
- API de sincronização efetivamente usada é `/api/ponto/bater/`, roteada para `api_bater_ponto`; endpoint antigo retorna 410. As duas cascas passam somente `timestamp_dispositivo` para `hora_do_aparelho` e gravam `ts_efetivo`/`ts_aparelho`, sem aplicar gap da fila. (`app/static/pwa.js:227`, `app/api/urls.py:67`, `app/ponto/views.py:419`, `app/ponto/views.py:426`, `app/api/views.py:518`, `app/api/views.py:666`, `app/api/views.py:678`, `app/api/views_core.py:505`, `app/api/views_core.py:658`, `app/api/views_core.py:670`)
- `hora_do_aparelho` aceita ISO e epoch em segundos/ms, torna naive aware, aceita hora até 48 h no passado e usa chegada para futuro/antiga demais. Isso opera sobre **timestamp bruto**, não pressupõe unidade/fuso de `timestamp_gap_ms`. (`app/api/hora_do_aparelho.py:72`, `app/api/hora_do_aparelho.py:79`, `app/api/hora_do_aparelho.py:85`, `app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`)
- Modelo define gap como milissegundos; tela mostra `ms` e compara com ±300000; JSON de detalhes entrega `gap_ms` sem conversão. As leituras API encontradas de `timestamp_gap_ms` são de **ping geo**, comparando ±300000 para aviso de skew; não são o kernel de bater/sincronizar e não aplicam fuso ao gap. (`app/ponto/models.py:76`, `app/templates/ponto/batidas_offline_pendentes.html:77`, `app/templates/ponto/batidas_offline_pendentes.html:79`, `app/templates/ponto/batidas_offline_pendentes.html:80`, `app/core/views.py:1022`, `app/api/views.py:2327`, `app/api/views.py:2340`, `app/api/views.py:2371`, `app/api/views_core.py:874`, `app/api/views_core.py:905`)

**Autoridades e derivação**

- Juiz registrado de selfie decide **ausência cobrável**, recebendo origem/canal/foto/duração; PWA = 0 min, APK/desconhecido = 90 min. Isso não é o timeout de câmera nem veto à gravação. As duas APIs consultam esse juiz sob foto ausente e depois chamam a porta de registro; foto é anexada condicionalmente. (`app/core/juizes.py:1192`, `app/core/juizes.py:1195`, `app/ponto/registro_batida.py:72`, `app/ponto/registro_batida.py:75`, `app/ponto/registro_batida.py:80`, `app/api/views.py:449`, `app/api/views.py:461`, `app/api/views.py:666`, `app/api/views.py:781`; `app/api/views_core.py:436`, `app/api/views_core.py:448`, `app/api/views_core.py:658`)
- Juiz GPS registrado considera zero/par parcial sem GPS e é consultado pelo geofence depois da gravação; adquirir coordenadas ou medir frescura no front é outra pergunta. (`app/core/juizes.py:1196`, `app/core/juizes.py:1201`, `app/ponto/services/geofence.py:126`, `app/ponto/services/geofence.py:145`, `app/ponto/services/geofence.py:269`, `app/api/views.py:789`)
- **Derivação paralela já existente:** V2 calcula distância e `dentro = d <= POSTO.raio` para badge próprio apesar do juiz registrado `classificar_posicao`. Esse trecho não é introduzido pelo delta fornecido; não confundir com a cura do timeout. (`app/templates/ponto/bater_ponto_v2.html:199`, `app/templates/ponto/bater_ponto_v2.html:217`, `app/templates/ponto/bater_ponto_v2.html:218`, `app/core/juizes.py:1191`)
- Textos contraditórios continuam: V2 exige verbalmente “Tire a selfie para registrar”, embora `garantirRequisitos` só avise e continue com foto ausente. (`app/templates/ponto/bater_ponto_v2.html:100`, `app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:276`)

## 11/11 14:25 · sessao f9c39a062fdf · - · - · origem=Ronald (tmux)

- **Textos contraditórios: NÃO fechado.** Triagem mantém “Tire sua selfie para liberar o ponto”, “Selfie obrigatória” e “Selfie obrigatoria — registre o ponto”; V2 mantém “Tire a selfie para registrar”, apesar de a fila aceitar foto nula. (`app/templates/ponto/triagem/apto.html:66`, `:124`, `:194`, `:250`; `app/templates/ponto/bater_ponto_v2.html:100`; `app/static/pwa.js:192`)

- **PWA ausente offline: NÃO fechado.** As duas telas dependem de `JulianiPWA.registrarBatidaOffline`; ausência do objeto cai no erro e exige nova tentativa, sem gravação alternativa. O teste sem PWA continua restrito ao online. (`app/templates/ponto/triagem/apto.html:289`, `:308`; `app/templates/ponto/bater_ponto_v2.html:319`, `:325`, `:332`, `:338`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:465`)

- **Legado eliminado: mantido.** A seleção de template retorna triagem ou V2; o comentário do verificador registra a exclusão de `ponto/bater_ponto.html`. O nome textual permanece no ticket histórico. (`app/ponto/views.py:225`, `:356`; `app/scripts/check_a_lei_batida.py:20`; `app/docs/TICKETS.md:693`)

- **`estadoPermissao`: não participa do fluxo examinado.** `garantirRequisitos` chama diretamente a captura central e as tentativas limitadas de GPS. (`app/templates/ponto/bater_ponto_v2.html:269`, `:274`, `:279`)

- **Teto da hora tem declaração única, significado de duração em milissegundos e um consumidor operacional:** `calcularESalvarGap` passa `HORA_TETO_MS` a `_noTeto`, envolvendo fetch e leitura do corpo. A V2 e a casca de triagem conservam literais independentes de 2000 ms para relógios de apresentação; eles não leem a constante e não calculam o gap persistido. Há parametrização paralela, mas são saídas distintas; não constitui, por si, duplicação do juiz registrado de selfie/GPS. (`app/static/pwa.js:88`, `:93`, `:98`, `:103`; `app/templates/ponto/bater_ponto_v2.html:160`, `:167`, `:169`; `app/templates/ponto/triagem/base_triagem.html:38`, `:41`, `:46`; `app/core/juizes.py:1195`, `:1201`)

- **`SELFIE_TETO_MS` é uma duração base, não um teto absoluto único para toda operação:** há uma declaração de 1500 ms; captura lê diretamente e abertura lê multiplicado por dois. Essa diferença é explícita no código e no oráculo. (`app/static/pwa.js:129`, `:162`, `:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:359`)

- **O fallback no teto não grava gap novo.** Sucesso escreve `ntp_gap` e `ntp_calculado_em`; timeout apenas lê o gap anterior ou devolve zero. Sem medição válida anterior, cada nova obtenção continua elegível à consulta, pois o fallback não renova `ntp_calculado_em`. (`app/static/pwa.js:104`, `:105`, `:108`, `:113`)

- **Timestamp não é o instante do clique.** O escritor espera `obterGap()` e somente depois lê `Date.now()` para `timestamp`; lê outro relógio para `timestamp_dispositivo`. Assim, para o mesmo gap, uma `/hora/` pendurada faz o timestamp refletir a espera de aproximadamente dois segundos, enquanto rejeição imediata segue antes. O comentário “caminho de hoje sem rede” descreve o valor do fallback, não identidade temporal das duas execuções. (`app/static/pwa.js:86`, `:98`, `:108`, `:182`, `:183`, `:189`)

- **Os testes novos não verificam esse valor temporal.** `falhas_da_fila` julga chegada, duração até `add` e presença de foto; não compara `timestamp`, `timestamp_dispositivo`, gap nem instante do clique. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:484`, `:490`, `:493`)

- **Autoridade servidor continua distinta:** `selfie_ausente` recebe dados e duração, sem ler relógio; decide cobrança vencida conforme canal, não admissibilidade do clique. `_respostaSelfie` produz foto/stream e aviso para as telas. Não há duplicação desse predicado pelo delta examinado. (`app/ponto/registro_batida.py:67`, `:72`, `:80`, `:86`; `app/static/pwa.js:144`; `app/core/juizes.py:1193`)

- **Aviso da V2 agora usa o mesmo valor produzido pelo PWA:** `_respostaSelfie` escreve `aviso`; `abrirCamera` o lê em `span.textContent`; o ramo de captura também o lê no toast. Foto anterior é escrita pelo input e passada à captura central. (`app/static/pwa.js:145`, `:178`; `app/templates/ponto/bater_ponto_v2.html:250`, `:255`, `:274`, `:276`)

- **Duas cascas incluem PWA**, porém o teste força a casca pelo loader, não demonstra a seleção real de casca em requisição de produção. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:255`, `:258`)

Execução do navegador, smoke real, hash do arquivo em produção e efeitos em produção: **[nao verificado]**.

## 11/11 14:25 · sessao 7fb9cd9072df · - · - · origem=Ronald (tmux)

**B1 e B2: a nova prova tem mordida para os defeitos específicos, por inspeção; execução e morte efetiva dos mutantes são [nao verificado].** O executor escreve página, script e perfil temporários, portanto não o executei sob a restrição de somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:279`, `:286`, `:295`, `:298`)

- **FilaRealTest / Me — PROCEDE a correção da lacuna anterior.** A sonda já não substitui `registrarBatidaOffline`: chama o escritor real e observa o `add` em `batidas_pendentes`; exercita `/hora/` normal, fetch pendurado e corpo pendurado. Me troca o teto por `1e9` e exige ausência de chegada à fila, tanto diretamente quanto nas duas telas offline. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:146`, `:151`, `:214`, `:437`, `:477`, `:501`, `:610`)

- **O stub que ignora abort fortalece a pergunta do teto**, porque `/hora/` permanece pendurada mesmo após `ctl.abort()`: o controle depende da resolução de `_noTeto`, e o mutante perde essa resolução dentro dos 15 segundos do ensaio. O corpo pendurado também está dentro do trabalho limitado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:113`, `:116`, `:119`, `:439`; `app/static/pwa.js:93`, `:97`, `:98`, `:139`)

- **Mf — PROCEDE a correção do aviso invisível.** O oráculo consulta `body.innerText`, enquanto o mutante substitui o `appendChild` visível por `title`. A câmera rejeitada precisa mostrar o aviso **antes de liberar o clique**; por isso o toast posterior de `garantirRequisitos` não salva o mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `:190`, `:342`, `:621`; `app/templates/ponto/bater_ponto_v2.html:250`, `:277`)

- **`_Base.par` — PROCEDE a correção do controle desacoplado.** Controle e mutante entram no mesmo `roda`; controle com qualquer falha ou sem cenário reprova; resultados “sonda não devolveu veredito” e “juiz sem veredito” são retirados das mortes. Os chamadores exigem mortes e padrão específico. “Mesmo lote” significa execuções Chromium separadas, cada qual com seu perfil temporário, submetidas ao mesmo executor paralelo. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `:451`, `:453`, `:455`, `:547`, `:548`, `:279`, `:298`, `:318`)

**Limitações concretas:**

- **“FilaReal” não prova persistência real nem conclusão do escritor.** O fake grava em objeto JS e marca sucesso dentro do `add`; `tx.oncomplete` vem depois. O oráculo verifica chegada, tempo e foto, mas não exige “fila resolveu”, embora o escritor real espere `tx.oncomplete`. Um cenário poderia passar tendo alcançado o fake `add` e ficando pendurado antes da conclusão da promessa. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:135`, `:150`, `:151`, `:215`, `:484`; `app/static/pwa.js:194`)

- **O fake encobre abertura bloqueada e falhas de armazenamento.** Seu `open` sempre chama sucesso após 1 ms; não modela upgrade bloqueado, quota, aborto ou rollback. O `abrirDB` real espera somente `onsuccess`/`onerror`, sem `onblocked` nem teto; `obterGap` já o espera antes da pergunta HTTP. Assim, a declaração ampla “do chamado ao add a espera é limitada” excede a prova e o código. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:160`, `:173`, `:473`; `app/static/pwa.js:48`, `:58`, `:59`, `:74`, `:113`, `:182`)

- **O caso V2 online cobre rejeição imediata, não POST pendurado ou resposta perdida.** O stub só retorna rejeição imediata ou resposta 200 imediata; não há cenário de servidor que recebe e deixa a resposta pendente. O `fetch` real anterior à fila não tem teto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:107`, `:109`, `:111`, `:506`; `app/templates/ponto/bater_ponto_v2.html:308`, `:319`)

- **O relógio do ensaio é virtual e o armazenamento é substituído justamente para acompanhá-lo.** Isso prova os timers JS exercitados nesse modelo; tempo real, suspensão da aba e durabilidade IndexedDB são [nao verificado]. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:126`, `:177`, `:299`)

- **Controle de câmera “ok”: ressalva ainda existente, resultado [nao verificado].** A sonda fornece `MediaStream()` vazio e falsifica dimensões; mantém `drawImage(video)` real. Uma falha nesse desenho poderia impedir alcançar `toBlob`; com `_Base.par`, porém, isso torna o controle vermelho e impede declarar o mutante morto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:80`, `:87`, `:89`, `:452`; `app/static/pwa.js:172`)

## 11/11 14:10 · sessao a833efdfbdac · FIO-E0-V4-E4B · construtor · origem=Code

Quatro correções implementadas: dia pelo juiz existente, trava por âncora, texto limitado e coleta confirmada após savepoint. [Recibo completo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c7.txt:20).

Observado em memória: quatro REDs na c6, seis mutantes detectados e 21 testes OK. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c7.txt:115). [nao verificado]

ORM não executado; comandos no recibo. Certificação Code/opus e concorrência real: [nao verificado].

## 11/11 13:53 · sessao cf07b64d85a6 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** Há dois bloqueantes: a idempotência não cobre fechamentos concorrentes da mesma competência, e o novo escritor interpreta o dia civil de uma conversa como competência de fato. (`app/chamados/services/fecho_runtime.py:30`, `app/chamados/services/fecho_runtime.py:35`, `app/pautas/models.py:123`, `app/chamados/models.py:2794`)

1. **Mudanças de produção e cumprimento do aval**

| Arquivo | Mudança, evento e avaliação |
|---|---|
| `disputa_emissao.py` | No S143, substitui o retorno antecipado pela coleta da recusa. Isso libera os passos seguintes também nos chamadores compartilhados. Cumpre a retirada do retorno exigida pelo aval. (`app/chamados/services/disputa_emissao.py:1945`, `app/chamados/services/disputa_emissao.py:1950`) |
| `fecho_runtime.py` | Acrescenta coletor em memória, agrupamento por competência, consulta de Pauta existente e escrita depois da transição, dentro do mesmo `atomic`. Cumpre o encaixe nas bordas; tem os dois bloqueantes indicados acima. (`app/chamados/services/fecho_runtime.py:14`, `app/chamados/services/fecho_runtime.py:21`, `app/chamados/services/fecho_runtime.py:66`) |
| `acoes_chamado.py` | A resposta livre do gestor passa a chamar o adaptador, conservando a transição existente. (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:316`) |
| `fio.py` | A resposta rápida do DP passa pelo mesmo adaptador após registrar a mensagem. (`app/chamados/services/fio.py:718`, `app/chamados/services/fio.py:722`) |

S128 continua lendo C6, tranca e testemunha da resposta; a porta `carimbar` reaplica C6 antes de persistir. HX-RESPOSTA-VISIVEL continua lendo `irmas_respondidas_sem_consumo` e chamando `reabrir`; HX-DISPUTA-ORFA continua lendo C6 e usando `carimbar`. Não achei veto novo de exportada/limbo nesses passos. (`app/chamados/services/disputa_emissao.py:1964`, `app/chamados/services/disputa_emissao.py:1968`, `app/chamados/services/disputa_emissao.py:1977`, `app/chamados/services/disputa_emissao.py:2007`, `app/chamados/services/disputa_emissao.py:2046`, `app/chamados/models.py:3139`)

C7 continua pelo próprio `pode_fechar`, com consulta fresca da tranca. Ele julga a **disputa**, enquanto a transição julga o **chamado**: a disputa mista pode permanecer aberta e o chamado fechar. O teste exige precisamente essa distinção. (`app/chamados/services/disputa_emissao.py:2079`, `app/chamados/services/disputa_emissao.py:2089`, `app/chamados/models.py:1848`, `app/chamados/tests/test_e4b_c6_orm.py:114`)

A trava do chamado permanece: a competência entregue continua fora de `_vivas_entre`. A nova Pauta usa exclusivamente `pautas.services.escrever`, destinatário `dp`, remetente `ti` e âncora de competência; o motivo limbo vem da recusa original. (`app/chamados/models.py:364`, `app/chamados/services/fecho_runtime.py:42`, `app/chamados/services/fecho_runtime.py:46`)

Não achei backfill, lista persistente nova ou escrita em `PENDENTES_RONALD` no módulo acrescentado. O contexto só é instalado no adaptador das duas bordas; repetir transição de chamado já encerrado não instala coleta. A correspondência integral da cópia ao commit/base permanece **[nao verificado]**, pois não achei metadados Git nesta cópia. (`app/chamados/services/fecho_runtime.py:11`, `app/chamados/services/fecho_runtime.py:63`, `app/chamados/services/fecho_runtime.py:68`)

2. **Quem escreve, quem lê e significado dos campos**

| Campo/valor | Escritor ou produtor | Leitor e significado |
|---|---|---|
| `_RECUSAS`, token | Declaração com `default=None`; instalação no início do ato e restauração em `finally`. (`app/chamados/services/fecho_runtime.py:11`, `:68`, `:72`) | `registrar_recusa` lê o contexto; fora dele não coleta. (`app/chamados/services/fecho_runtime.py:16`) |
| `ato['chamado']` | Recebe `chamado.pk`. (`app/chamados/services/fecho_runtime.py:67`) | É comparado ao PK do chamado que disparou o resolver; não é PK de disputa. (`app/chamados/services/fecho_runtime.py:17`) |
| `ato['perguntas']` | `registrar_recusa` escreve `{pergunta.pk: (objeto, motivo)}`; nova coleta do mesmo PK sobrescreve a anterior. (`app/chamados/services/fecho_runtime.py:18`) | `_escrever_pautas` percorre os valores após a transição. (`app/chamados/services/fecho_runtime.py:28`, `:73`) |
| `res`/`motivo` | S143 produz `recusa:credor_toca_exportada` ou `recusa:credor_carrega_limbo:<PKs>[:credor_toca_exportada]`. (`app/chamados/services/disputa_emissao.py:1835`, `:1840`, `:1860`) | O resolver reconhece os prefixos; o escritor remove apenas `recusa:` para compor o texto. (`app/chamados/services/disputa_emissao.py:1946`, `app/chamados/services/fecho_runtime.py:42`) |
| `p.pk`, `p.disputa` | Nascimento da pergunta por `_nascer`, com disputa e inserção. (`app/chamados/models.py:3014`, `:3017`) | Identificam pergunta e relação usada para chegar ao colaborador/empresa. (`app/chamados/services/fecho_runtime.py:29`, `:32`) |
| `p.data_evento` | Atribuição central em `gravar_dia_do_fato`; `abrir` recebe dia do turno, `abrir_conversa` fornece dia civil. (`app/chamados/services/fio.py:791`, `:801`, `app/chamados/models.py:2776`, `:2801`) | O novo escritor lê cru, sem `conversa`; aqui está a colisão de significados. (`app/chamados/services/fecho_runtime.py:30`) |
| Empresa e corte | Empresa vem da relação do colaborador; corte é cadastro de `Empresa`, escrito na manutenção cadastral. (`app/colaboradores/models.py:321`, `:46`, `app/colaboradores/views.py:523`) | `janela_atual` lê o corte por `corte_da_empresa`; o runtime usa a empresa da disputa. (`app/ponto/janelas.py:36`, `:60`, `app/chamados/services/fecho_runtime.py:29`) |
| Competência, `fim.year/month` | `janela_atual(data_evento, empresa)` fornece a janela; o runtime usa seu fim. (`app/chamados/services/fecho_runtime.py:30`) | Forma `<empresa>:<AAAA-MM>`; para perguntas de fato, é competência por corte, não mês civil. (`app/chamados/services/fecho_runtime.py:31`, `app/ponto/janelas.py:24`) |
| `MARCADOR`, `ancora_tipo`, `ancora_id` | Constante local e composição da âncora; persistência pela porta. (`app/chamados/services/fecho_runtime.py:10`, `:31`, `app/pautas/services.py:184`) | Deduplicação por marcador+âncora em Pautas vivas; leitores gerais usam âncora para localizar Pautas. (`app/chamados/services/fecho_runtime.py:35`, `app/pautas/services.py:334`) |
| Texto, destinatário, remetente, autor | Runtime monta texto e passa `dp`, `ti`, `autor_sistema('ti')`; `escrever` valida e persiste. (`app/chamados/services/fecho_runtime.py:43`, `:46`, `app/pautas/services.py:157`, `:180`) | Aba DP filtra destinatário; payload entrega texto e âncora. Autor é informação de trilha. (`app/pautas/services.py:308`, `app/pautas/views.py:39`, `app/pautas/models.py:105`) |
| `feito_em`, `cancelado_em` | Portas de estado de Pauta; `marcar_feita` escreve o primeiro. (`app/pautas/services.py:233`, `:247`) | `.vivas()` significa não feita e não cancelada; portanto uma Pauta feita não impede outra entrega posterior. (`app/pautas/models.py:57`, `:64`, `app/chamados/services/fecho_runtime.py:36`) |
| `status_local`, `novo_status`, usuário | As bordas fornecem destino/usuário; `transicionar` delega ao escritor `_gravar_estado`, que salva status e campos associados. (`app/chamados/services/acoes_chamado.py:316`, `app/chamados/models.py:199`, `:228`, `:296`) | Adaptador lê `VIVOS→ENCERRADOS`; motor define os conjuntos; signal lê o status salvo. (`app/chamados/services/fecho_runtime.py:63`, `app/chamados/catalogo/motor.py:24`, `:30`, `app/chamados/signals.py:96`) |

**Não achei troca de PK de chamado por PK de disputa** nessa composição: a âncora do fio devolve chamado, o coletor guarda chamado, e a âncora da Pauta guarda empresa+competência. (`app/chamados/services/fio.py:117`, `app/chamados/services/fecho_runtime.py:67`, `app/pautas/models.py:34`)

**Achei dia civil tratado como dia de fato.** O discriminador já existe: `_dia_de_turno_da_pergunta` devolve `None` para conversa; o novo escritor não o considera. Os leitores crus anteriores estão declarados em censo, mas isso não elimina o novo uso incorreto. (`app/chamados/juizes.py:332`, `app/chamados/services/fecho_runtime.py:30`, `app/chamados/services/fio.py:1389`)

A deduplicação também não agrega novas recusas ao texto existente: encontrou marcador+âncora viva, executa `continue`. Portanto entrega posterior de outro chamado da mesma competência não acrescenta seus identificadores à Pauta existente. Isso segue a deduplicação escolhida, mas limita a rastreabilidade por chamado. (`app/chamados/services/fecho_runtime.py:35`, `app/chamados/services/fecho_runtime.py:37`)

3. **Fechamentos fora das duas bordas**

Fora do contexto, a recusa continua deixando nota no credor, mas sua coleta para Pauta é no-op. A diferença c6 nesses caminhos é liberar S128/HX/C7 após a recusa; não acrescentar entrega DP. (`app/chamados/services/disputa_emissao.py:1855`, `app/chamados/services/disputa_emissao.py:1950`, `app/chamados/services/fecho_runtime.py:17`)

| Caminho | Efeito depois da cura |
|---|---|
| S143 por save de status | Continua condicionado a status encerrado e escrita da coluna; executa resolver em savepoint. Sem contexto, nenhuma Pauta E4B. (`app/chamados/signals.py:96`, `:104`, `:114`, `app/chamados/services/fecho_runtime.py:17`) |
| `becos.fechar_beco` | Trabalha com chamados já terminais, chama resolver e depois C7; pode realizar limpezas antes interrompidas. Não instala coletor. (`app/chamados/services/becos.py:43`, `:54`, `:55`) |
| Reconciliador `_retratar` | Após retenção e reconciliação permitirem, chama resolver e segue para nota; ignora seu retorno. (`app/chamados/reconciliador.py:518`, `:523`, `:536`, `:538`) |
| Reconciliador/costura | Após reaponte aceito para candidato resolvido, chama resolver e retorna `True`. (`app/chamados/reconciliador.py:1642`, `:1648`, `:1649`) |
| `reconciliar_fantasmas` | No apply, pode chamar resolver diretamente; contagens e evento podem agora refletir limpezas antes suprimidas. (`app/ponto/management/commands/reconciliar_fantasmas.py:269`, `:281`, `:282`, `:288`) |
| Outros atos humanos | Exceção, agenda, escala, geofence e benefício continuam usando transição direta; `fechar()` também. (`app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`, `app/colaboradores/views.py:2068`, `app/chamados/models.py:454`) |
| Signals e cascatas | Disputa fechada reconcilia chamado; desligamento/vínculo usam portas existentes, sem coletor E4B. (`app/chamados/signals.py:42`, `app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/models.py:1619`) |
| Commands/cron | Meio período usa `fechar`; becos usa sua porta. O cron de becos está configurado como alarme. (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`, `app/chamados/management/commands/fechar_becos_disputa.py:57`, `app/config/crons.py:663`) |
| API | Não achei fechamento direto de chamado no endpoint consultado: seu POST anexa nota. Há fechamento indireto por disputa/regularização. (`app/api/views.py:988`, `app/api/foto_recebida.py:80`, `app/chamados/services/regularizacao_ext.py:205`) |
| Django admin | Registro de `ChamadoColaborador` no Django admin: **não achei**. [nao verificado: ausência por pesquisa textual] |

**Sim, permanece possível fechar com viva exportada sem entrega E4B:** usar `ch.fechar(admin)` no cenário c6 passa pela transição e pelo S143, mas não instala coletor; a exportada fica fora da F1. Isso é lacuna de alcance anterior, preservada pela delimitação às duas bordas, não nova supressão de uma entrega existente. Reprodução ORM dessa variante: **[nao verificado]**. (`app/chamados/models.py:454`, `app/chamados/models.py:364`, `app/chamados/signals.py:115`, `app/chamados/services/fecho_runtime.py:17`)

O teste de passivo não cobre todos esses fechamentos futuros: planta o chamado já fechado por SQL e só depois chama save/resolver/beco. (`app/chamados/tests/test_e4b_c6_orm.py:160`, `app/chamados/tests/test_e4b_c6_orm.py:163`)

4. **Atomicidade e ordem**

- **Se `escrever` levantar exceção:** ela sai do adaptador e desfaz a transição no banco, porque ambas estão no `atomic` externo. Não fica chamado fechado por esse ato com a Pauta falhada. (`app/chamados/services/fecho_runtime.py:66`, `app/chamados/services/fecho_runtime.py:70`, `app/chamados/services/fecho_runtime.py:73`)
- **Se a transição levantar exceção:** o `finally` restaura o contexto e a escrita de Pauta não é alcançada. (`app/chamados/services/fecho_runtime.py:69`, `app/chamados/services/fecho_runtime.py:72`)
- **Limite dessa garantia:** erro interno do S143 é engolido pelo signal. Antes da coleta, pode deixar o chamado fechado sem entrega; depois da coleta, pode desfazer as limpezas e ainda permitir Pauta, pois o dicionário em memória não participa do rollback. Esses cenários de falha não foram demonstrados no ORM fornecido. (`app/chamados/signals.py:114`, `app/chamados/signals.py:116`, `app/chamados/services/fecho_runtime.py:18`, `app/chamados/services/fecho_runtime.py:73`)
- **Concorrência:** não há garantia de uma Pauta por marcador+âncora. A consulta e a criação são separadas; o modelo tem índice de âncora, mas não unicidade dessa entrega. Atualizar o contador depois da criação não revalida a existência. (`app/chamados/services/fecho_runtime.py:35`, `app/pautas/services.py:180`, `app/pautas/models.py:116`, `app/pautas/models.py:123`, `app/pautas/services.py:188`)

5. **Classes de reprovação e força das provas**

Não achei segundo juiz nem segundo escritor de Pauta fora da porta. A competência por corte usa `janela_atual`; o defeito temporal é o **insumo civil da conversa**, não uma fórmula nova de mês. (`app/chamados/services/disputa_emissao.py:1964`, `app/chamados/services/disputa_emissao.py:2089`, `app/chamados/services/fecho_runtime.py:25`, `app/chamados/services/fecho_runtime.py:46`)

Não achei leitura nova de relógio no módulo c6. Entretanto continua existindo relógio dentro do C6, fora da borda: S143 passa `hoje=None` e `_dentro_da_janela_viva` pode usar `date.today()`. É comportamento preexistente, não corrigido nesta volta. (`app/chamados/services/disputa_emissao.py:1945`, `app/chamados/juizes.py:577`)

Não achei inversão nova da ordem Colaborador→Disputa→Chamado→Pergunta. A trava existente ocorre antes do save; porém ela não serializa colaboradores distintos pela âncora empresa+competência da Pauta. (`app/chamados/models.py:289`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`)

Os dois mutantes c6 têm mordida direta: retorno antecipado impede o carimbo da morta; `sem_pauta` elimina a entrega. O protocolo rejeita erro, precondição quebrada, sobrevivência e assertion de outro alvo. (`app/chamados/tests/e4b_c6_verificar.py:24`, `app/chamados/tests/e4b_c6_verificar.py:27`, `app/chamados/tests/e4b_c6_verificar.py:114`, `app/chamados/tests/test_e4b_c6_protocolo.py:39`)

O dado anexado demonstra GREEN 383, as quatro mortes c6 declaradas e oito mortes c4; não demonstra suíte inteira. No recorte c6, os dois ensaios da **base** não aparecem explicitamente certificados. Suíte inteira e execução independente nesta cópia: **[nao verificado]**. (`anexo c6_orm.log:2`; roteiro exigido em `app/chamados/tests/e4b_c6_verificar.py:109`)

Não achei teste c6 de concorrência, falha real de escrita da Pauta ou conversa. Os testes existentes exercitam reentrega serial e erro dentro da transição em memória. Logo seus verdes não fecham os bloqueantes encontrados. (`app/chamados/tests/test_e4b_c6_orm.py:136`, `app/chamados/tests/test_e4b_c6_memoria.py:82`, `app/chamados/tests/test_e4b_c6_memoria.py:93`)

6. **Contraexemplos concretos**

- **Duplicação concorrente — bloqueante.** Empresa 2, competência `2026-09`, dois chamados de colaboradores distintos, ambos com recusa exportada e nenhuma Pauta existente. A e B observam `exists=False`; A cria/commita; B continua sem reler e cria outra. Resultado permitido: **duas Pautas vivas com o mesmo marcador e âncora**. As travas por colaborador não impedem essa intercalação. Reprodução concorrente ORM: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:35`, `app/chamados/services/fecho_runtime.py:46`, `app/chamados/models.py:2666`, `app/pautas/models.py:123`)

- **Competência da conversa — bloqueante.** Empresa com corte 24; conversa `ancora_ausente` criada em 23/09, destino A e pai B distinto, sem resposta/carimbo/célula; depois competência 09 entregue. Essa combinação nasce pela porta existente. Ao fechar A, S127 não encontra disputa com pai A e F1 exclui a pergunta pela data civil entregue; C6 conserva a conversa viva. O runtime escreve Pauta `empresa:2026-09`, atribuindo competência de fato à conversa que não possui dia de turno. Ocorrência no dado de produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1339`, `app/chamados/services/disputa_emissao.py:1371`, `app/chamados/juizes.py:2322`, `app/chamados/models.py:364`, `app/chamados/juizes.py:336`, `app/chamados/juizes.py:1405`, `app/chamados/services/fecho_runtime.py:30`)

- **Motivo truncado — defeito adicional.** Vinte perguntas recusadas e motivo limbo contendo quarenta PKs de seis dígitos excedem o campo: limitar vinte IDs não limita a lista dentro do motivo. A porta grava apenas os primeiros 500 caracteres, podendo perder PKs e o sufixo `credor_toca_exportada`. O código permite essa perda; ocorrência em produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/fecho_runtime.py:39`, `app/chamados/services/fecho_runtime.py:42`, `app/pautas/services.py:185`)

**Veredito final: REPROVADO**, pelos bloqueantes de idempotência concorrente e interpretação do dia civil da conversa; a suíte inteira também permanece pendente de comprovação. (`app/chamados/services/fecho_runtime.py:30`, `app/chamados/services/fecho_runtime.py:35`; suíte inteira **[nao verificado]**)

## 11/11 13:49 · sessao 23a023a5f323 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- A mudança no resolver retira apenas o retorno antecipado e entrega a recusa ao coletor; S128, HX e C7 continuam nos juízes existentes. C6/C7/tranca estão registrados, sem autoridade paralela acrescentada. (`app/chamados/services/disputa_emissao.py:1945-1950`, `:1964`, `:1968`, `:1977`, `:2007`, `:2046`, `:2089`; `app/core/juizes.py:278-279`, `:292`)

- A porta de carimbo reaplica C6 após aplicar os campos em memória e desfaz se a pergunta continuar viva; HX resposta conserva a porta `reabrir` e suas recusas. (`app/chamados/models.py:3128-3145`, `:1390`; `app/chamados/services/disputa_emissao.py:2007-2009`)

- A ordem de trava continua Colaborador → Disputa → Chamado → Pergunta; o escritor de estado trava antes de salvar e o resolver reutiliza o mesmo protocolo. Não achei alteração dessa ordem no trecho c6. (`app/chamados/models.py:280-296`, `:2665-2693`; `app/chamados/services/disputa_emissao.py:1877-1879`)

- Não achei leitura nova de relógio no runtime: `janela_atual` recebe `p.data_evento`; o relógio de fallback C6 já existia. `janela_atual(None)` falha em `data.day`, sem substituir por hoje. (`app/chamados/services/fecho_runtime.py:30`; `app/chamados/juizes.py:577`; `app/ponto/janelas.py:24-27`)

- Há limitação de atomicidade das limpezas: S143 tem savepoint próprio e engole exceções. Uma recusa coletada antes de erro posterior permanece na ContextVar, embora as limpezas S128/HX/C7 daquele savepoint sejam desfeitas; a borda ainda pode escrever sua Pauta. Não implica chamado sem Pauta, mas impede equiparar “Pauta escrita” a “limpezas concluídas”. (`app/chamados/signals.py:106-117`; `app/chamados/services/fecho_runtime.py:18`, `:70-73`)

- **Novo leitor incorreto do campo com dois significados:** `abrir` grava dia do turno; `abrir_conversa` grava dia civil, e o juiz declara conversa sem competência de fato. O novo escritor usa `data_evento` cru sem distinguir `conversa`, materializando competência civil como competência de fato. (`app/chamados/models.py:2783-2802`; `app/chamados/juizes.py:332-336`; `app/chamados/services/fecho_runtime.py:29-31`)

Contraexemplo estrutural alcançável:

1. Empresa corte 24; em 23/09 criar conversa `ancora_ausente` com destino A e pai B distinto pela porta existente; depois entregar competência 09. A porta constrói precisamente pai B e destino A. (`app/chamados/services/disputa_emissao.py:1338-1362`, `:1371-1377`; `app/chamados/models.py:2801`)
2. Admin fecha A: S127 procura disputa cujo **pai** seja A, e F1 exclui a pergunta pelo mês entregue calculado sobre sua data civil; escolher `fechado` também evita a guarda S122 específica de `resolvido`. (`app/chamados/juizes.py:2322-2324`; `app/chamados/models.py:364`, `:181`)
3. A conversa sem resposta/carimbo/célula continua viva: seu dia do turno é `None`, portanto C6 não entra na janela viva e termina em `True`. (`app/chamados/juizes.py:336`, `:1394-1406`)
4. S143 recusa pela competência civil e o novo runtime escreve Pauta 09, embora a conversa não tenha competência de fato. Existência desse caso na produção fornecida: [nao verificado]. (`app/chamados/services/disputa_emissao.py:1835-1844`; `app/chamados/services/fecho_runtime.py:29-48`; `app/chamados/juizes.py:333-335`)

Os leitores crus preexistentes estão declarados em censo; isso distingue dívida anterior do **novo escritor** c6, ausente daquele censo. (`app/chamados/services/fio.py:1389-1410`)

## 11/11 13:49 · sessao 16d6a2088751 · - · - · origem=Ronald (tmux)

Censo de fechamento fora das duas bordas, somente leitura:

- **A mudança no resolver alcança todos os chamadores.** A recusa agora registra em memória e prossegue para os passos seguintes, em vez de devolver `{0,0}`; S128, HX e C7 continuam em `app/chamados/services/disputa_emissao.py:1950`, `:1964`, `:2007`, `:2046`, `:2089`. Fora de coletor, o registro é no-op (`app/chamados/services/fecho_runtime.py:11`, `:16`, `:17`).

- **Somente duas entradas produtivas instalam o coletor:** resposta livre do detalhe e resposta rápida do fio (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`). O único `.set()` está em `app/chamados/services/fecho_runtime.py:68`, o reset em `:72`, e a escrita depois da transição em `:73`. Outra entrada produtiva desse coletor: **não achei** [nao verificado: pesquisa textual não prova chamadas dinâmicas].

- **S143 por qualquer save de status encerrado:** aceita apenas `ENCERRADOS`, exclui raw, exige que o save tenha escrito status, executa resolver dentro de savepoint e engole exceções (`app/chamados/signals.py:96`, `:104`, `:114`, `:115`, `:116`). `.update()` não emite esse evento (`app/chamados/signals.py:95`). Sem coletor humano não nasce Pauta E4B (`app/chamados/services/fecho_runtime.py:17`).

- **Becos:** não fecha o chamado; trabalha com pais/destinos já terminais, trava a ordem, chama resolver e só então relê C7 e fecha disputa (`app/chamados/services/becos.py:43`, `:46`, `:51`, `:54`, `:55`, `:56`, `:57`). Após cura, recusa deixa de suprimir limpezas posteriores; não instala coletor (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/becos.py:54`; `app/chamados/services/fecho_runtime.py:17`).

- **Reconciliador `_retratar`:** retenção pode impedir; depois `reconciliar()` precisa devolver True; segue rejeição automática da justificativa pendente, resolver explícito e nota (`app/chamados/reconciliador.py:518`, `:523`, `:529`, `:532`, `:536`, `:538`). Retorno do resolver é ignorado (`:536`). Não passa a escrever Pauta E4B (`app/chamados/services/fecho_runtime.py:17`).

- **Reconciliador/costura:** reaponte ao candidato histórico precisa ser aceito; depois chama resolver do chamado já resolvido e retorna True (`app/chamados/reconciliador.py:1642`, `:1643`, `:1648`, `:1649`). Agora pode executar as limpezas após a recusa; sem coletor, nenhuma entrega E4B (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:17`).

- **`reconciliar_fantasmas`:** só aplica após opção aplicar; tenta `retratar`; se este devolver falso, chama resolver diretamente; contagens, nota e evento dependem do resultado (`app/ponto/management/commands/reconciliar_fantasmas.py:269`, `:280`, `:281`, `:282`, `:286`, `:288`). Pode passar a contar/carimbar/fechar disputas que antes o return impedia; não passa a entregar Pauta E4B (`app/chamados/services/disputa_emissao.py:1950`, `:2092`; `app/chamados/services/fecho_runtime.py:17`).

- **Outros fechamentos humanos continuam diretos:** aprovação de exceção (`app/chamados/services/acoes_chamado.py:70`), agenda (`:108`), escala (`:149`), autorização geofence (`:222`), recusa geofence (`:277`) e avaliação de benefício (`app/colaboradores/views.py:2068`). A porta `ChamadoColaborador.fechar` também chama diretamente `transicionar` (`app/chamados/models.py:454`). Nenhum desses instala coletor E4B (`app/chamados/services/fecho_runtime.py:68`, únicos consumidores em `app/chamados/services/acoes_chamado.py:316` e `app/chamados/services/fio.py:722`).

- **Signals fechadores:** disputa fechada tenta `ch.reconciliar()` antes da nota (`app/chamados/signals.py:28`, `:42`, `:45`); evento de batida chama reconciliador de chamados (`app/ponto/signals.py:214`, `:215`). O gate sistêmico decide premissa e pergunta viva e escreve pela porta de estado (`app/chamados/models.py:682`, `:685`, `:654`). Sem borda humana, não há entrega E4B (`app/chamados/services/fecho_runtime.py:17`).

- **Cascade e ajuste de vínculo:** desligamento percorre `NAO_FECHADOS` e usa `encerrar()` (`app/colaboradores/services/desligamento.py:150`, `:155`); troca de escala encerra chamados anteriores (`app/colaboradores/services/vinculo.py:384`, `:387`). `encerrar()` conserva gate de pergunta viva e escritor central (`app/chamados/models.py:1617`, `:1619`).

- **Commands/cron:** comando de meio período usa `ch.fechar()` no apply (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `:109`); comando de becos usa `fechar_beco()` no apply (`app/chamados/management/commands/fechar_becos_disputa.py:54`, `:57`). Cron de becos está configurado `--alarme` (`app/config/crons.py:663`); comando de ausência com `--alarme` apenas relata e retorna (`app/ponto/management/commands/fechar_chamados_ausencia.py:35`, `:40`), e apply delega à porta existente (`:44`; `app/ponto/services/ausencia.py:694`). Nenhuma dessas entradas ganha coletor E4B (`app/chamados/services/fecho_runtime.py:68`).

- **API:** fechamento direto de `ChamadoColaborador` em `app/api` **não achei** [nao verificado: pesquisa textual]. `api_chamados_v2` POST apenas anexa nota (`app/api/views.py:988`, `:990`); upload separado de foto pode fechar disputa (`app/api/foto_recebida.py:80`), cujo signal tenta reconciliar chamado-pai (`app/chamados/signals.py:42`). Resposta de regularização chama materializador (`app/api/views.py:3000`), que pode encerrar chamado pela porta (`app/chamados/services/regularizacao_ext.py:205`).

- **Django admin:** `app/chamados/admin.py` está vazio [nao verificado: não há linha citável]; registro de `ChamadoColaborador` no Django admin **não achei** [nao verificado: pesquisa textual global].

**Pode haver novo fechamento sem entrega da viva exportada fora das duas bordas.** A exclusão da exportada da F1 é explícita (`app/chamados/models.py:364`); `fechar`, `encerrar` e `reconciliar` não instalam coletor (`app/chamados/models.py:454`, `:1619`, `:654`). Assim, no cenário c6, chamar diretamente `ch.fechar(admin)` fecha sem Pauta E4B: transição → save → S143 → recusa → coletor ausente (`app/chamados/models.py:454`, `:296`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:17`). Execução desse variante: [nao verificado]. A ausência de entrega por essas entradas é anterior; não identifiquei nova supressão de entrega que já existisse [nao verificado: análise estática].

O teste de “passivo” c6 não cobre esse fechamento novo: planta o chamado já fechado por SQL antes de emitir o save/resolver/beco (`app/chamados/tests/test_e4b_c6_orm.py:160`, `:163`, `:164`, `:165`). Ele prova que passivo não recebe Pauta (`:166`), sem certificar todos os fechamentos futuros fora das bordas.

## 11/11 13:49 · sessao 6255d01866f6 · - · - · origem=Ronald (tmux)

- Os RED c6 mordem diretamente a cura: `retorno_antecipado` recoloca o retorno no lugar da coleta; `sem_pauta` elimina a chamada do escritor. As assertions correspondentes exigem morta carimbada e Pauta presente. O protocolo exige processo `rc=1`, exatamente um failure, caso e mensagem esperados e ausência de falha de precondição. (`app/chamados/tests/e4b_c6_verificar.py:24`, `:27`, `:114`; `app/chamados/tests/test_e4b_c6_orm.py:111`, `:122`)

- A sobreposição da base troca integralmente os três módulos produtivos preexistentes; `sobrepor_modulo` restaura globais e troca aliases já importados. Não achei atalho que deixe o adaptador c6 nas bordas da base. (`app/chamados/tests/e4b_c6_verificar.py:54`; `app/chamados/tests/e4b_c4_base.py:28`, `:35`, `:39`)

- O RED ORM usa C6/K6 reais como precondição, porta real de transição nas duas telas e spy de `reabrir` que delega ao método real. Confere carimbos, C7, chamado fechado, zero reabertura, Pauta com âncora e motivo, e exportação/FM inalterados. (`app/chamados/tests/test_e4b_c6_orm.py:76`, `:80`, `:86`, `:106`, `:111`, `:124`, `:131`)

- Limbo é exercitado pela irmã com texto sem `respondida_em`, com viva exportada no mesmo credor; o teste exige `credor_carrega_limbo:<pk>` no texto. Não prova limbo sem viva exportada nem pergunta sem data. (`app/chamados/tests/test_e4b_c6_orm.py:73`, `:82`, `:127`, `:147`; comportamento desses outros cenários [nao verificado])

- Não achei teste c6 com escrita real da Pauta falhando, rollback ORM desse erro ou concorrência. O teste de erro em memória falha dentro da transição e só confere restauração do ContextVar/ausência da chamada ao escritor; a idempotência exercitada é sequencial. (`app/chamados/tests/test_e4b_c6_memoria.py:61`, `:82`; `app/chamados/tests/test_e4b_c6_orm.py:136`)

- **Bloqueante de idempotência concorrente:** dois chamados de colaboradores diferentes na mesma empresa/competência podem ambos observar `exists=False`; depois cada um chama `escrever`, que faz `Pauta.objects.create`. Não há releitura nem exclusão mútua pela âncora. A trava do resolver é por colaborador; os constraints de Pauta só validam destinatário. Resultado permitido: duas Pautas vivas com mesmo marcador e âncora. Reprodução efetiva [nao verificado]. (`app/chamados/services/fecho_runtime.py:35`, `:46`; `app/pautas/services.py:180`; `app/chamados/services/disputa_emissao.py:1878`; `app/pautas/models.py:123`)

- A atualização comum do contador ocorre depois da criação da Pauta e não corrige essa corrida; eventual espera pela linha do contador não reexecuta o teste de existência. (`app/pautas/services.py:180`, `:188`, `:352`, `:354`)

- Competência usa a autoridade existente: `janela_atual(p.data_evento, empresa)` e ano/mês do fim da janela, mesma composição já usada por `competencia_do_dia_entregue`. O corte vem do cadastro da empresa. Não achei derivação nova por mês civil. (`app/chamados/services/fecho_runtime.py:29`; `app/chamados/services/vigia_fio.py:822`, `:838`; `app/ponto/janelas.py:24`, `:36`, `:60`)

- A âncora `competencia` significa empresa+competência. A leitura do DP filtra o departamento; o payload leva texto e âncora. “Viva” significa não feita e não cancelada; portanto marcar feita permite uma entrega posterior criar outra Pauta. (`app/pautas/models.py:34`, `:57`; `app/pautas/services.py:304`, `:322`; `app/pautas/views.py:39`; `app/chamados/services/fecho_runtime.py:35`)

- O texto pode exceder 500 caracteres: só os IDs das perguntas têm limite de vinte, enquanto os motivos carregam listas de limbo sem teto; a porta corta `texto[:500]`. Possível truncamento de IDs/motivos é consequência do código, mas não achei RED desse tamanho. (`app/chamados/services/fecho_runtime.py:39`, `:42`; `app/chamados/services/disputa_emissao.py:1840`; `app/pautas/services.py:185`)

- O recorte anexado demonstra GREEN 383 e quatro mortes c6 — dois mutantes × dois motivos —, além das oito mortes c4. Não mostra os dois rótulos de morte c6 da base nem a execução integral da suíte; esses resultados permanecem [nao verificado]. Os runners locais são explicitamente recortados por labels. (`app/chamados/tests/e4b_c6_verificar.py:97`, `:109`; `app/chamados/tests/e4b_c4_verificar.py:124`; execução: `c6_orm.log` anexado [nao verificado quanto a linhas não fornecidas])

## 11/11 13:45 · sessao 08cbd3a06898 · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

A cura dos bloqueios de câmera está na árvore. Permanecem uma espera sem teto antes da gravação offline e lacunas na prova dos testes. (`app/static/pwa.js:122`, `:170`, `:86`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:118`, `:94`)

Executei os dois testes estáticos de GPS, ambos verdes, e o censo de câmera em memória. Não executei o Chromium: seu executor cria arquivos, perfil e diretório temporário, contrariando a regra desta revisão. Execução dos testes de navegador, smoke nas duas cascas e efeitos em produção: **[nao verificado]**. (`app/ponto/tests/test_l030_forma_js.py:72`, `:77`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:483`, `:213`, `:229`, `:232`)

**1. Achados da volta 1: cura e mordida prevista**

“Fechado no código” abaixo não significa teste de navegador executado nesta revisão. **[nao verificado]** [nao verificado]

| Achado anterior | Situação em 618b13d98 e teste correspondente |
|---|---|
| Triagem offline esconde confirmar quando a câmera falha | **Fechado no código:** mostra o botão antes de abrir a câmera e conserva-o sem stream. Mutante M1 recoloca a ocultação e exige ausência de envio. (`app/templates/ponto/triagem/apto.html:252`, `:259`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:403`) |
| Triagem online fica presa em `getUserMedia` pendente | **Fechado no código:** abertura passa pelo teto de 3 segundos; sem stream, libera a batida. Matriz inclui câmera pendente; M3 remove a liberação. (`app/static/pwa.js:145`, `:150`; `app/templates/ponto/triagem/apto.html:154`, `:164`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:51`, `:356`, `:413`) |
| Triagem online depende exclusivamente do callback de `toBlob` | **Fechado no código:** libera controles antes da captura e aguarda a resposta limitada do PWA. Matriz inclui `toBlob` mudo e conversão lançando; teste direto verifica o teto. (`app/templates/ponto/triagem/apto.html:176`, `:177`, `:231`; `app/static/pwa.js:165`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:55`, `:56`, `:371`) |
| Triagem offline só enfileira dentro do callback | **Fechado no código:** captura resolve primeiro; enfileiramento fica fora do callback. M4 reinsere a espera infinita pelo callback. (`app/templates/ponto/triagem/apto.html:276`, `:289`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:418`) |
| V2 veta `_fotoBlob` nulo | **Fechado no código:** `garantirRequisitos` não retorna recusa por foto; o chamador prossegue. M2 reinsere o veto. (`app/templates/ponto/bater_ponto_v2.html:268`, `:297`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:408`) |
| V2 fica pendente quando `toBlob` cala e `toDataURL` lança | **Fechado no código:** fallback do timer está protegido e termina em `null`. Matriz cobre essa combinação; Md remove o teto. (`app/static/pwa.js:126`, `:165`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:56`, `:371`, `:461`) |
| Legado online/offline conserva barreiras | **Eliminado com o template**, sem renderizador vivo encontrado; M6 aparece como histórico da remoção. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/views.py:356`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`) |
| Aviso centralizado, mas captura e espera duplicadas | **Corrigido para a operação da câmera:** abertura, captura, conversão e teto estão no PWA; censo fecha os produtores. Persistem textos próprios contraditórios, tratados adiante. (`app/static/pwa.js:122`, `:145`, `:156`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:476`, `:497`; `app/templates/ponto/triagem/apto.html:124`) |
| Mutante vivo: `await` infinito em `garantirRequisitos` | **Agora tem teste específico:** Ma injeta exatamente essa espera e exige “NAO chegou” na V2 online/offline. Morte efetivamente executada: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:446`) |
| Mutante vivo: `semSelfie → false` | A função antiga saiu; **Mb ataca o equivalente atual**, trocando `aviso` por `false` em `_respostaSelfie`. Morte efetivamente executada: **[nao verificado]**. (`app/static/pwa.js:132`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:451`) |
| `throw` no ramo sem foto | **Agora tem teste específico:** Mc injeta a exceção e exige ausência de envio. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:456`) |
| PWA não carregou | **Online protegido; offline não fechado:** chamadas de selfie têm guarda; o teste sem PWA cobre apenas online. A fila continua dependendo do escritor desse script. (`app/templates/ponto/triagem/apto.html:154`, `:232`, `:289`; `app/templates/ponto/bater_ponto_v2.html:273`, `:331`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:360`) |
| `estadoPermissao` ainda tinha chamador | Definição e chamada não permanecem no fluxo atual; `garantirRequisitos` consulta diretamente captura e GPS. (`app/templates/ponto/bater_ponto_v2.html:268`) |
| Transporte incompleto de timestamps/GPS | **Não fechado:** fila guarda timestamp corrigido, gap, precisão e instante GPS; sync transmite timestamp bruto, sem esses três últimos insumos. (`app/static/pwa.js:176`, `:179`, `:205`, `:209`) |

**2. Quem decide hoje**

| Sítio | Predicado e efeito |
|---|---|
| Juiz registrado do servidor | `origem == 'app'`, ausência de ambas as fontes de foto e `decorrido_min >= prazo`. Prazo PWA: **0 minutos**; APK/desconhecido: **90 minutos**, com sobreposição explícita para APK/desconhecido. Decide **cobrança**, não admissibilidade. (`app/core/juizes.py:1192`; `app/ponto/registro_batida.py:71`, `:80`) |
| API principal | Sob `not foto`, consulta o juiz com `tem_foto=False`, duração zero; acrescenta motivo/contexto. Criação segue pela porta; anexação é condicional. (`app/api/views.py:436`, `:449`, `:461`, `:666`, `:781`) |
| API core | Mesmo predicado e comportamento. (`app/api/views_core.py:423`, `:436`, `:448`, `:658`, `:776`) |
| Retro | Consulta o juiz usando canal gravado, `FotoBatida OR Batida.foto` e duração em minutos; emite cobrança pela porta de disputa. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:74`, `:96`) |
| POST web da triagem | Grava primeiro; `foto_sessao or request.FILES.get("foto")` controla somente anexação. Não cobra selfie inline nesse ramo. (`app/ponto/services/triagem_batida.py:701`, `:748`) |
| PWA | Sem frame, usa foto anterior ou `null`; captura/conversão falha ou pendente termina pelo teto; ausência produz aviso. (`app/static/pwa.js:126`, `:133`, `:158`, `:165`) |
| Triagem online | `r.stream` escolhe confirmação de selfie ou liberação sem câmera; `sel.foto` decide anexação, sem recusa por ausência. (`app/templates/ponto/triagem/apto.html:155`, `:164`, `:233`, `:241`) |
| Triagem offline | Confirmar permanece disponível; `sel.foto` ou `null` vai ao escritor da fila. (`app/templates/ponto/triagem/apto.html:252`, `:277`, `:289`) |
| V2 online/offline | Captura pode atualizar `_fotoBlob` e agendar aviso; não veta foto nula. Online anexa condicionalmente; offline entrega o valor à fila. (`app/templates/ponto/bater_ponto_v2.html:273`, `:275`, `:297`, `:305`, `:331`) |
| Confirmação de turno | Formulário envia entrada sem campo de foto, captura ou fila própria. (`app/templates/ponto/triagem/turno_confirmar.html:8`) |
| Fila e sincronizador | Escritor normaliza ausência para `null`; sincronizador omite arquivo quando `!b.foto` e envia o restante. (`app/static/pwa.js:180`, `:214`, `:215`) |

**`_respostaSelfie` isoladamente não é o único juiz operacional:** ela apenas monta `{stream|foto, aviso}`. A operação central está no conjunto `_noTeto`/`abrirCameraSelfie`/`capturarSelfie`; as telas ainda controlam apresentação e transporte. Não encontrei nelas veto independente por permissão negada, câmera ocupada, câmera pendente ou conversão falha: esses resultados passam pela mesma resposta limitada. Câmera ocupada não tem cenário nominal próprio no teste; sua execução específica é **[nao verificado]**. (`app/static/pwa.js:122`, `:132`, `:145`, `:156`; `app/templates/ponto/triagem/apto.html:154`, `:255`; `app/templates/ponto/bater_ponto_v2.html:240`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:50`)

A linguagem ainda decide por conta própria em alguns pontos: “Selfie obrigatória” e “Tire a selfie para registrar” contradizem o comportamento permissivo. São mensagens próprias, embora não constituam veto técnico. (`app/templates/ponto/triagem/apto.html:66`, `:124`, `:194`, `:250`; `app/templates/ponto/bater_ponto_v2.html:100`)

**3. Teto, porta e clique perdido**

O valor mora em `SELFIE_TETO_MS = 1500`, no PWA: captura usa **1,5 segundo**; abertura usa **3 segundos**. `_noTeto` resolve erros e expiração como valor ou `null`; ambas as operações passam depois por `_respostaSelfie`. (`app/static/pwa.js:117`, `:122`, `:150`, `:165`)

Esse teto limita a selfie, não o clique inteiro: a triagem pode esperar GPS por 8 segundos, e a V2 faz duas tentativas de 10 e 12 segundos antes do envio. (`app/templates/ponto/triagem/apto.html:218`; `app/templates/ponto/bater_ponto_v2.html:279`)

A batida liberada sem foto segue **pela mesma porta de sua rota normal**: triagem usa o mesmo formulário e `registrar_batida`; V2 usa `/api/ponto/bater/`; fila sincroniza nesse endpoint, cujos dois kernels chamam `registrar_batida`. Não há porta especial para “estourou o teto”. (`app/templates/ponto/triagem/apto.html:241`; `app/ponto/services/triagem_batida.py:701`; `app/templates/ponto/bater_ponto_v2.html:307`; `app/static/pwa.js:215`; `app/api/views.py:666`; `app/api/views_core.py:658`)

**B1 — permanece um caminho de clique sem gravação offline:** `registrarBatidaOffline` espera `obterGap()` antes do `add`. Com gap ausente/vencido, esse caminho espera `fetch('/hora/')` sem timeout. Se a requisição ficar pendente, a selfie já terá resolvido, mas a fila ainda não terá recebido a batida. É contraexemplo condicional pelo código; incidência em produção: **[nao verificado]**. (`app/static/pwa.js:86`, `:100`, `:170`, `:175`)

O teste de navegador não detecta B1: substitui o escritor por uma função que marca “fila” e resolve imediatamente. Também conserva `/hora/` respondendo na sonda, inclusive nos cenários classificados como offline. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:106`, `:118`)

Há outras limitações de entrega já presentes: sem PWA não existe fila; triagem online usa submit nativo; V2 enfileira rejeição de rede, mas respostas HTTP de erro apenas mostram erro e liberam nova tentativa. Chegada real ao servidor nesses caminhos: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:241`, `:289`; `app/templates/ponto/bater_ponto_v2.html:313`, `:316`, `:331`)

**4. Template apagado**

Confirmo o censo estático sem referência viva a `ponto/bater_ponto.html`: a view seleciona V2 ou templates de triagem; os nomes de URL `bater_ponto`/`bater_ponto_v2` continuam sendo rotas, não referências ao arquivo removido. (`app/ponto/views.py:225`, `:356`; `app/ponto/urls.py:11`)

As três referências textuais restantes são comentário de exclusão, comentário histórico do M6 e ticket “MATAR”. O verificador de URLs já o retirou da lista. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`; `app/docs/TICKETS.md:693`; `app/tools/check_frontend_urls.py:25`)

**5. Escritores, leitores e significados**

| Campo/valor | Quem escreve → quem lê |
|---|---|
| `{stream, aviso}` / `{foto, aviso}` | PWA produz; triagem/V2 leem para controles, aviso e transporte. Ausência é `null`; aviso é texto ou `null`. (`app/static/pwa.js:133`, `:151`, `:166`; `app/templates/ponto/triagem/apto.html:155`, `:233`; `app/templates/ponto/bater_ponto_v2.html:274`) |
| Foto anterior/arquivo | Input da câmera do sistema escreve `_fotoBlob`; captura preserva `anterior` quando necessário; V2 envia ou enfileira. (`app/templates/ponto/bater_ponto_v2.html:252`, `:273`, `:305`, `:331`; `app/static/pwa.js:166`) |
| `fila.foto = null` | `registrarBatidaOffline` é o escritor IndexedDB; sync lê e omite arquivo. Ausência já pertence ao contrato da fila. (`app/static/pwa.js:175`, `:180`, `:194`, `:214`) |
| POST sem foto | Triagem/V2/sync deixam de anexar; APIs leem com `request.FILES.get`. A criação não exige foto. (`app/templates/ponto/triagem/apto.html:233`; `app/templates/ponto/bater_ponto_v2.html:305`; `app/static/pwa.js:214`; `app/api/views.py:436`, `:666`; `app/api/views_core.py:423`, `:658`) |
| Foto em sessão | View escreve base64/nome; serviço recupera, converte e escolhe entre sessão e upload. (`app/ponto/views.py:324`; `app/ponto/services/triagem_batida.py:512`, `:748`) |
| Foto persistente | `anexar_foto` escreve `Batida.foto` pela porta e, conforme `registro`, `FotoBatida`. Modelo aceita nulo; retro lê ambas as fontes. (`app/ponto/registro_batida.py:296`; `app/ponto/models.py:78`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`) |
| `foto_ausente` | APIs/retro entregam motivo/contexto à disputa; catálogo pede confirmação de presença; upload posterior apara/fecha disputas vinculadas à batida. É **ausência cobrável**, não booleano físico da foto. (`app/api/views.py:464`; `app/api/views_core.py:451`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/chamados/catalogo/perguntas.py:93`; `app/api/foto_recebida.py:57`) |
| Sync/remoção | PWA lê fila, envia e remove após sucesso ou HTTP 400; rede/5xx conservam item. SW delega às páginas; sem página, notifica. (`app/static/pwa.js:188`, `:220`, `:228`, `:239`; `app/static/sw.js:112`) |

Nos leitores examinados, ausência não exige foto sempre presente. Quando a foto da fila é truthy, o multipart pressupõe Blob/File. Upload separado exige foto para **anexar**, sem transformar isso em exigência para criar batida. Consumidores externos à cópia: **[nao verificado]**. (`app/static/pwa.js:214`; `app/ponto/models.py:78`; `app/api/views.py:850`; `app/api/views_core.py:835`)

**6. Checklist das classes**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando registrado | Não identifiquei duplicação da cobrança: servidor decide prazo cobrável; PWA limita captura e devolve resultado/aviso. (`app/core/juizes.py:1192`; `app/ponto/registro_batida.py:80`; `app/static/pwa.js:122`, `:133`) |
| Segundo escritor fora da porta | Cura usa escritor existente da fila; servidor continua criando/anexando pelas portas existentes. (`app/static/pwa.js:169`; `app/api/views.py:666`, `:784`; `app/api/views_core.py:658`, `:779`; `app/ponto/services/triagem_batida.py:701`, `:751`) |
| Relógio fora da borda | Captura usa timer de duração; juiz servidor recebe duração. Permanecem relógios próprios de feedback e timestamp da fila, sem unificação nova nesta cura. (`app/static/pwa.js:127`, `:171`; `app/ponto/registro_batida.py:80`; `app/templates/ponto/triagem/apto.html:290`; `app/templates/ponto/bater_ponto_v2.html:332`) |
| Derivação paralela | Câmera/conversão centralizadas; textos próprios de obrigatoriedade permanecem. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:497`; `app/templates/ponto/triagem/apto.html:124`; `app/templates/ponto/bater_ponto_v2.html:100`) |
| Selo/RED sem mordida suficiente | **B2:** aviso “visto” é busca em `body.innerHTML`; aceita aviso somente em `title`. `_morre` executa apenas a mutação, sem exigir controle original verde no mesmo ensaio. Assim, aviso invisível pode satisfazer o oráculo e falha anterior pode aparentar matar mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:94`, `:270`, `:387`, `:398`; `app/templates/ponto/bater_ponto_v2.html:249`) |
| Campo com dois significados | `foto_ausente` mantém sentido de cobrança; `foto` mantém Blob/File/null nas bordas. Não confundir ausência física imediata com ausência vencida do APK. (`app/ponto/registro_batida.py:80`; `app/static/pwa.js:166`, `:180`, `:214`) |
| Duas cascas | Ambas incluem PWA e diálogo comum; teste compõe cada casca real. Porém força essa composição pelo loader: não prova seleção real da casca. Os dois kernels API preservam o mesmo predicado de selfie. (`app/templates/base.html:20`, `:388`; `app/templates/base_app.html:20`, `:145`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:185`, `:333`; `app/api/views.py:461`; `app/api/views_core.py:448`) |

Existe ainda uma suspeita sobre o controle “câmera ok”: a sonda fornece `MediaStream()` vazio e falsifica dimensões, enquanto `drawImage(video)` permanece real. Se esse desenho falhar, o controle perde foto antes da conversão; resultado efetivo no Chromium: **[nao verificado]**. Não uso essa suspeita como bloqueante comprovado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:75`, `:82`; `app/static/pwa.js:160`)

REPROVADO — bloqueantes: **B1**, espera sem teto antes de gravar a fila, encoberta pela substituição do escritor no smoke (`app/static/pwa.js:86`, `:170`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:118`); **B2**, prova de aviso aceita atributo invisível e mutantes não exigem controle verde acoplado (`app/ponto/tests/test_selfie_nao_barra_chromium.py:94`, `:387`; `app/templates/ponto/bater_ponto_v2.html:249`).

## 11/11 13:44 · sessao 530cb5ae113b · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE — alternativa (a).** O selo aceita uma fonte fora das previstas pelo aval. (`bin/mapa20_valida.py:73`; `aval_mapa20_pronto_x_de_n_1111.txt:1`, anexo, item 1.)

1. **Critério aplicado:** a célula deve conter ao menos uma correspondência de `arquivo:linha`; todas as correspondências devem ter caminho relativo sem componente `..`, arquivo legível em UTF-8 e linha existente, ≥ 1 e não vazia. Não verifica se é uma fonte autorizada nem se contém o PRONTO da obra. (`bin/mapa20_valida.py:51`; `bin/mapa20_valida.py:73–90`.)

2. **Entrada concreta aceita:** `X de N = 1 de 2`, **fonte = `bin/relato.sh:2`**, falta = `item 2: prova`. A linha citada é apenas o comentário “PUBLICA o relato e os indices…”. Essa é a entrada padrão do teste “verde”, esperado rc 0; reproduzida em memória, passou com zero problemas. A aceitação da fonte termina em `return ''`; sem problemas, o processo retorna 0. (`bin/relato.sh:2`; `bin/tests/test_mapa20_valida.sh:27`; `bin/tests/test_mapa20_valida.sh:42`; `bin/mapa20_valida.py:90`; `bin/mapa20_valida.py:225`.)

3. **Leitura literal:** o aval define “N = itens numerados do PRONTO da obra (fonte: o aval da obra, app/docs/TICKETS.md ou BACKLOG, com linha citada)” e exige rc 1 “se N sem fonte citada”. O item 4 usa a fonte definida no item 1; não cabe interpretá-lo como autorização para qualquer comentário existente. O código substitui essa condição pela mera existência da referência, deixando passar a entrada acima. (`aval_mapa20_pronto_x_de_n_1111.txt:1`, anexo, itens 1 e 4; `bin/mapa20_valida.py:73–90`.)

## 11/11 13:41 · sessao f25cd63cd977 · - · - · origem=Ronald (tmux)

Auditoria estática dos testes, sem execução: [nao verificado]

- **Ma agora tem mordida prevista:** injeta `await` infinito em `garantirRequisitos`, exercita V2 online/offline e exige “NAO chegou”. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:446`)
- **Mb agora tem mordida prevista:** troca aviso por `false`, exercita V2 online e apto offline e exige ausência do aviso. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:451`)
- **Demais regressões nomeadas:** M1 botão offline oculto (:403), M2 veto sem foto (:408), M3 online não libera (:413), M4 espera callback (:418), M5 texto paralelo (:424), M7 exceção no juiz (:429, :434), M8 juiz sem guarda (:441), Mc `throw` sem foto (:456), Md teto removido (:461). M6 é declarado eliminado junto com o legado (:377). Todas as referências deste item são `app/ponto/tests/test_selfie_nao_barra_chromium.py`.
- **Duas cascas estão efetivamente compostas e exercitadas:** loader troca `base.html` pelo conteúdo da casca escolhida; matriz percorre cascas × rotas × câmeras. Mutantes usam somente `base_app.html`. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:185`, `:333`, `:356`, `:391`)
- **Persistência não é testada pelo smoke:** a sonda substitui `registrarBatidaOffline` por função que marca alcance e retorna `Promise.resolve()`. Portanto esse verde prova chamada à fila, não escrita IndexedDB, sync ou chegada ao servidor. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:116`)
- **Controle verde não está acoplado aos mutantes:** `_morre` só executa fonte mutada; exige alguma falha e regex. O original tem testes separados. Se o original já falhar pelo mesmo motivo, o mutante pode parecer morto sem causalidade demonstrada. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:387`, `:398`, `:356`)
- **Oráculo de aviso aceita conteúdo invisível:** `temAviso()` procura texto em `body.innerHTML`; V2 grava o aviso em `title`. Esse atributo basta para “aviso visto” e “aviso ao liberar”, sem toast visível. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:94`, `:131`, `:163`, `:270`; `app/templates/ponto/bater_ponto_v2.html:249`)
- **Possível falso RED que exige execução para confirmar:** câmera “ok” devolve `new MediaStream()` sem tracks; a sonda falsifica dimensões, mas deixa `drawImage(video)` real. Se o canvas rejeitar vídeo sem frame, até o controle “ok” perde foto e os mutantes podem morrer pelo motivo preexistente. Resultado no Chromium: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:75`, `:82`; `app/static/pwa.js:160`)
- **`test_l030_*` conserva fiscalização estática de GPS:** o teste renderizado aplica o mesmo oráculo de tokens; fiscalização selfie foi retirada e apontada ao Chromium. (`app/ponto/tests/test_l030_templates_renderizados.py:14`, `:37`; `app/ponto/tests/test_l030_forma_js.py:117`)

Execução efetiva de qualquer teste ou mutante: **[nao verificado]**. [nao verificado]

## 11/11 13:41 · sessao b08d61d550d2 · - · - · origem=Ronald (tmux)

O censo não encontrou uso vivo do template apagado `bater_ponto.html`: a view renderiza V2 ou o seletor de triagem; as rotas conservam nomes `bater_ponto`/`bater_ponto_v2`, que não são referências ao arquivo removido. (`app/ponto/views.py:225`, `app/ponto/views.py:356`; `app/ponto/urls.py:11`)

As referências restantes ao arquivo removido são comentário de exclusão, comentário histórico dos mutantes e ticket “MATAR”; o verificador de URLs já não o inclui. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`; `app/docs/TICKETS.md:693`; `app/tools/check_frontend_urls.py:25`)

No front de batida, as chamadas reais a câmera/conversão estão concentradas em PWA; a outra ocorrência é onboarding, declarado como foto de referência no censo fechado. (`app/static/pwa.js:137`, `app/static/pwa.js:148`, `app/static/pwa.js:163`; `app/templates/core/onboarding.html:154`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:476`)

As telas continuam interpretando a resposta para apresentação e transporte, mas não retornam recusando por foto nula: triagem libera sem stream, aguarda captura protegida e envia; offline mantém confirmar e entrega Blob/null à fila; V2 aguarda requisitos e anexa somente se houver foto. (`app/templates/ponto/triagem/apto.html:154`, `app/templates/ponto/triagem/apto.html:231`, `app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:273`, `app/templates/ponto/bater_ponto_v2.html:297`, `app/templates/ponto/bater_ponto_v2.html:305`)

Persistem textos contraditórios próprios: “Tire sua selfie para liberar o ponto”, “Selfie obrigatória”, o texto de refazer e “Tire a selfie para registrar”; não constituem veto técnico, mas a linguagem de obrigatoriedade não foi centralizada. (`app/templates/ponto/triagem/apto.html:66`, `app/templates/ponto/triagem/apto.html:124`, `app/templates/ponto/triagem/apto.html:194`, `app/templates/ponto/triagem/apto.html:250`; `app/templates/ponto/bater_ponto_v2.html:100`)

As duas cascas carregam o mesmo PWA e diálogo global; diferem na estrutura de navegação e outros scripts, sem outra política de câmera localizada. Os testes trocam o loader de `base.html` pela casca real, portanto verificam composição nas duas cascas; seleção real da casca e execução dos testes: [nao verificado]. (`app/templates/base.html:20`, `app/templates/base.html:385`, `app/templates/base.html:388`; `app/templates/base_app.html:20`, `app/templates/base_app.html:142`, `app/templates/base_app.html:145`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:185`)

A confirmação de turno aberto continua podendo enviar formulário sem foto, fora do juiz visual da câmera; não contém captura nem fila local. (`app/templates/ponto/triagem/turno_confirmar.html:8`)

Continuam riscos fora do teto da câmera: V2 não enfileira HTTP de erro resolvido, apenas rejeição de `fetch`; triagem online usa submit nativo; todos os caminhos offline dependem do escritor PWA. (`app/templates/ponto/bater_ponto_v2.html:307`, `app/templates/ponto/bater_ponto_v2.html:313`, `app/templates/ponto/bater_ponto_v2.html:316`; `app/templates/ponto/triagem/apto.html:241`, `app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:331`)

## 11/11 13:41 · sessao 8bdba0451286 · PROPORCAO-POR-RISCO · verificador · origem=Code

A v3 fecha vários exemplos da v2, mas ainda permite escrita com **rc 0 deduzido por leitura**. A seleção continua dependendo do nome imediato da chamada e da interseção dos nós reconhecidos com linhas adicionadas. (`bin/selo_proporcao.py:64`, `bin/selo_proporcao.py:69`, `bin/selo_proporcao.py:130`, `bin/selo_proporcao.py:132`)

Não executei o selo, testes ou exemplos; os retornos abaixo são deduções. Execução efetiva, efeitos persistidos e situação em produção: **[nao verificado]**. [nao verificado]

**1. Fechamento dos achados da volta 2**

| Achado da v2 | Situação na v3 e cura | Caso que o morde |
|---|---|---|
| Continuação `()` sobre atributo antigo | **Fecha o exemplo direto:** a `Call` reconhecida cobre sua extensão inteira. (`bin/selo_proporcao.py:69`, `bin/selo_proporcao.py:132`) | `continuacao_sobre_attr_antigo`. (`bin/tests/test_selo_proporcao.sh:60`) |
| Argumento novo em chamada escritora antiga | **Fecha quando a função é `Name`/`Attribute` reconhecido.** A chamada aninhada continua escapando, demonstrada abaixo. (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`) | `argumento_novo_em_update_antigo`, sobre preparo antigo. (`bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:61`) |
| `exec("obj.save()")` | **Fecha o literal direto**, por `OPACO`. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:82`) | `exec_string`. (`bin/tests/test_selo_proporcao.sh:62`) |
| `eval("obj.save()")` | **Fecha o literal direto**, pelo mesmo mecanismo. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`) | `eval_string`. (`bin/tests/test_selo_proporcao.sh:63`) |
| Decorador da v2 com `@eval(...)` | **Fecha aquele exemplo**, pois contém `eval`; não fecha decoradores em geral. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:82`) | `eval_string` cobre o gatilho, mas não reproduz o decorador. (`bin/tests/test_selo_proporcao.sh:63`) |
| Nome de método em f-string | **Fecha o exemplo:** segundo argumento de `getattr` não é `Constant`. (`bin/selo_proporcao.py:73`) | `fstring_nome`. (`bin/tests/test_selo_proporcao.sh:66`) |
| `setattr` instala alias escritor | **Fecha o exemplo direto**, conservadoramente pelo próprio `setattr`. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`) | `setattr_alias`; não instala nem invoca um escritor real, mas isola esse gatilho. (`bin/tests/test_selo_proporcao.sh:64`) |
| QuerySet com método calculado | **Fecha o exemplo**, sem precisar resolver o tipo de `qs`. (`bin/selo_proporcao.py:73`) | `getattr_calculado`. (`bin/tests/test_selo_proporcao.sh:65`) |
| Alias novo de cursor e SQL fragmentado | **Fecha aquele diff** pela referência nova a `.execute`. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:80`) | `cursor_execute` contém somente a referência, suficiente para provar esse mecanismo. (`bin/tests/test_selo_proporcao.sh:68`) |
| SQL na f-string `UPDATE {tabela} SET ...` | **Fecha:** substitui expressões por ` x ` antes de procurar SQL; `.execute` também passa a ser reconhecido. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:76`) | `sql_fstring` isola a f-string, sem cursor. (`bin/tests/test_selo_proporcao.sh:67`) |
| Payload `.pyw` carregado por `runpy.run_path` | **Fecha o executor apresentado**, por `run_path`; `.pyw` continua fora da análise. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:124`) | `import_dinamico` testa `import_module`, **não** `run_path` nem o payload `.pyw`. (`bin/tests/test_selo_proporcao.sh:70`) |
| Arquivo `.sql` e executor | **Fecha o exemplo:** destino `.sql` é A, e `.execute` novo também seria detectado. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:122`) | `arquivo_sql` prova a regra por caminho. (`bin/tests/test_selo_proporcao.sh:69`) |
| Escritor novo em teste importado por linha antiga | **Não fecha; passou a limite declarado.** O arquivo continua excluído. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:124`) | Nenhum caso reproduz essa costura; `prod_importa_teste` adiciona somente o import. (`bin/tests/test_selo_proporcao.sh:49`) |
| `import *` via reexportação antiga de teste | **Não fecha e não está explicitamente declarado no docstring.** Import de módulo comum não revela a origem reexportada. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:124`) | Sem caso correspondente. (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:70`) |
| Template chama método escritor | **Não fecha; limite declarado.** HTML continua excluído. (`bin/selo_proporcao.py:23`, `bin/selo_proporcao.py:124`) | `tela_pura` só muda texto; não chama método. (`bin/tests/test_selo_proporcao.sh:29`) |
| Argumentos excedentes | **Fecha:** exige três ou quatro argumentos. (`bin/selo_proporcao.py:94`) | `arg_a_mais`. (`bin/tests/test_selo_proporcao.sh:78`) |
| Retorno de `bash -e` não afirmado | **Fecha a ausência da asserção**, mantendo problema no preparo especial. (`bin/tests/test_selo_proporcao.sh:76`, `bin/tests/test_selo_proporcao.sh:77`) | `rc_bash_e` agora exige exatamente 1. (`bin/tests/test_selo_proporcao.sh:77`) |
| Normalização universal de erros | **Não fecha:** wrapper permanece um `exec`, sem normalização de falhas de lançamento. (`bin/selo_proporcao.sh:4`) | Sem caso de Python ausente, execução recusada ou sinal. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:79`) |

As curas anteriores permanecem: oito métodos ORM, espaço após ponto, alias por referência, `getattr` literal, `#` em string, signals, caminhos NUL, migration por rename, `--text` e revisão iniciada por opção. Os mecanismos estão em `bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:118` e `bin/selo_proporcao.py:56`; os respectivos casos estão em `bin/tests/test_selo_proporcao.sh:30`, `bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:35`, `bin/tests/test_selo_proporcao.sh:36`, `bin/tests/test_selo_proporcao.sh:37`, `bin/tests/test_selo_proporcao.sh:39`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:48` e `bin/tests/test_selo_proporcao.sh:73`.

Continuam sem reprodução específica: `(obj.save)()`, caminho com tabulação e conteúdo falso `+++ /dev/null`; os casos próximos não têm essas formas. (`bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:34`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:48`)

Quanto ao custo dos limites:

- **Helper que já grava:** está compreensível: chamada comum pode passar, e o texto atribui a detecção ao smoke. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:26`)
- **Template escritor:** a exclusão está explícita, mas deveria dizer diretamente que renderizar o template pode causar escrita real não detectada; existe método `rebaixar()` que chega a `self.save()`. Renderização e objeto no contexto: **[nao verificado]**. (`bin/selo_proporcao.py:23`, `app/chamados/models.py:488`, `app/chamados/models.py:498`)
- **Teste importado por linha antiga:** explica a exclusão técnica, mas chamar isso de “custo do aval, não fuga” omite a consequência: **escrita nova no corpo do teste pode alcançar produção com B verde**. Isso é mais amplo que apenas chamar helper que já escrevia. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:124`)
- **`import *`:** falta declaração expressa sobre reexportação e seu custo. Aceitação específica desse limite pelo Ronald: **[nao verificado]**. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:85`)

**2. Novos contraexemplos à v3**

Cada diff abaixo é independente e declarado B. Os `.py` ficam fora dos caminhos especiais e de testes; os objetos mencionados precisam existir no contexto. Os **rc 0 são deduzidos** das regras de seleção; funcionamento desses contextos em produção: **[nao verificado]**. (`bin/selo_proporcao.py:118`, `bin/selo_proporcao.py:124`, `bin/selo_proporcao.py:132`, `bin/selo_proporcao.py:145`)

**a) Chamada aninhada, alterando somente argumento.** Em um `ModelForm`, o exemplo muda a opção de persistência; presença desse formulário na aplicação: **[nao verificado]**.

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,3 +1,3 @@
 getattr(form, "save")(
-    commit=False, [nao verificado]
+    commit=True, [nao verificado]
 )
```

O `Call` externo cruza a linha adicionada, mas sua função é outro `Call`; `nome_chamado` devolve `None`. O `getattr` e a constante `"save"` estão inteiramente na linha antiga. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:132`)

**b) Lambda antiga passa a ser executada.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,2 @@
 (lambda: obj.save()
-)
+)()
```

A chamada tem `Lambda` como função; o atributo escritor permanece na linha antiga. A alteração passa a invocar a expressão que antes apenas construía a função. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:132`)

**c) Decorador passa a executar a lambda escritora.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,5 +1,5 @@
 @(
     lambda f, acao=(lambda: obj.save()):
-        f
+        (acao(), f)[1]
 )
 def tela(): pass
```

A referência `.save` está na linha antiga; a chamada nova chama `acao`, nome comum. Nem as lambdas nem a tupla são classificadas como escrita. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:132`)

**d) `__dict__` e `vars()`, com nome fragmentado.** São alternativas independentes para um contexto com `Model` e `obj` disponíveis. Disponibilidade concreta: **[nao verificado]**.

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+Model.__dict__["sa" + "ve"](obj)
```

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+vars(Model)["sa" + "ve"](obj)
```

`__dict__`, `vars` e as constantes separadas não pertencem aos conjuntos; a função externa é um `Subscript`. **Rc 0 em ambas.** Usar `"save"` inteiro seria detectado. (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:83`)

**e) `operator.methodcaller`.**

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+import operator
+operator.methodcaller("sa" + "ve")(obj)
```

`methodcaller` não está nos conjuntos, o nome é fragmentado e a chamada externa tem outra `Call` como função. **Rc 0.** (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`)

**f) Reflexão por `type(...)`.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+type(obj).__getattribute__(obj, "sa" + "ve")()
```

`type` e `__getattribute__` não são reconhecidos; a chamada final tem uma `Call` como função. **Rc 0.** (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:83`)

**g) Alias de `exec` por import.**

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+from builtins import exec as rodar
+rodar("obj.save()")
```

O import só verifica origens de teste/signals; não aplica `OPACO` aos nomes importados. A chamada vê `rodar`, e a string `"obj.save()"` não é um nome exato nem SQL. **Rc 0.** Em contraste, `rodar = exec` seria detectado pelo `Name exec`. (`bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`)

**h) `.sql` renomeado para sufixo ignorado.**

```diff
diff --git a/app/x/q.sql b/app/x/q.txt
rename from app/x/q.sql
rename to app/x/q.txt
```

Considere `q.sql` contendo escrita e um executor antigo que só consome `q.txt` quando esse arquivo existe. A renomeação pode ativar o payload sem mudar Python; existência desse executor: **[nao verificado]**.

O selo considera o destino; `.txt` é ignorado. Mesmo que Git reporte deleção e adição, a deleção é ignorada e o destino continua fora. **Rc 0.** Rename mantendo destino `.sql` dá **rc 1**, portanto não constitui escape. (`bin/selo_proporcao.py:110`, `bin/selo_proporcao.py:116`, `bin/selo_proporcao.py:122`, `bin/selo_proporcao.py:124`)

**i) F-string sem SQL nas partes constantes.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1 +1,2 @@
 run = cur.execute
+run(f"{'DE' + 'LETE'} {'FR' + 'OM'} tabela")
```

A referência `.execute` é antiga; `run` não é resolvido como alias. O `JoinedStr` vira aproximadamente ` x   x  tabela`, e nenhuma constante contém SQL completo. **Rc 0.** (`bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:76`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:132`)

**3. Falsos positivos e controles**

| Código adicionado em `.py` comum | Rc B deduzido |
|---|---|
| `getattr(obj, "x")` | **0**: segundo argumento continua sendo `Constant`, sem nome reconhecido. (`bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:83`) |
| `getattr(obj, nome)` usado apenas para leitura | **1, novo falso positivo:** nome calculado basta. (`bin/selo_proporcao.py:73`) |
| `motor_de_template.execute()` sem persistência | **1, novo falso positivo:** não resolve o tipo do receptor. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`) |
| `texto = f"UPDATE {nome} SET rótulo"` apenas para exibição | **1, novo falso positivo:** o texto montado satisfaz a regex SQL. (`bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:76`) |
| `texto = f"update {nome}"` | **0**: não contém `SET`, e a constante `"update "` não é o nome exato `"update"`. (`bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:76`, `bin/selo_proporcao.py:83`) |
| `texto = f"update{nome}"` | **1, já ocorria na v2:** a constante exata `"update"` basta. (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:83`) |
| Palavra `update` em template `.html` | **0**: HTML permanece fora. (`bin/selo_proporcao.py:124`) |
| Referência a `exec`, `eval` ou `compile` sem execução | **1:** `Name` em `OPACO` basta. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:82`) |
| `.sql` contendo apenas `SELECT`, comentário ou texto | **1:** regra incondicional por caminho. (`bin/selo_proporcao.py:122`) |

Pelo critério fornecido, esses falsos positivos que empurram para A não são bloqueantes; o código também declara essa direção conservadora. (`bin/selo_proporcao.py:27`)

**4. Retornos**

- **Argumento a mais agora dá 2**, inclusive na chamada testada com `repo extra`. (`bin/selo_proporcao.py:94`, `bin/tests/test_selo_proporcao.sh:78`)
- **Argumentos inválidos, revisão inválida, Git não zero, parse inválido e exceções ordinárias alcançadas pela fronteira Python dão 2.** (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:134`, `bin/selo_proporcao.py:152`)
- **Não são todos os caminhos de erro:** falha ao lançar Python pode preservar 126/127; término por sinal não passa pela captura de `Exception`. Ocorrência concreta: **[nao verificado]**. O wrapper permanece sem normalização. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:152`)
- **`bash -e` recebe o rc 1 inteiro**, se o selo o produzir: `exec` preserva o retorno, e o shell externo captura a saída do shell com `-e`. O teste agora afirma exatamente 1. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:143`, `bin/tests/test_selo_proporcao.sh:77`)
- **Rc 1 continua significando classificação conservadora**, incluindo falsos positivos; não comprova escrita efetiva. (`bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:143`)

**5. Qualidade dos casos**

| Caso ou anúncio | Avaliação |
|---|---|
| `so_remocao` | **Corrigido:** base contém `a = 1` e `o.save()`; `sed` só remove a segunda linha. O arquivo permanece Python válido, sem linha adicionada. (`bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:51`) |
| `apagou_migration` | **Corrigido:** migration existe na base e é realmente removida. Exercita o ramo `D`. (`bin/tests/test_selo_proporcao.sh:15`, `bin/tests/test_selo_proporcao.sh:56`, `bin/selo_proporcao.py:116`) |
| `sql_escrita` | **Ganhou duplicidade na v3:** `.execute` e a constante SQL bastam independentemente. Não isola a regex SQL. (`bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`, `bin/selo_proporcao.py:83`) |
| `signal_nome` | **Duplicidade permanece:** módulo terminado em `signals` ou nome importado `post_save` bastam independentemente. (`bin/tests/test_selo_proporcao.sh:40`, `bin/selo_proporcao.py:88`) |
| Chamadas diretas ORM | Agora podem disparar por `Attribute` e por `Call`; não isolam a nova regra de extensão. Os dois casos com chamada antiga isolam melhor essa cura. (`bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:60`, `bin/tests/test_selo_proporcao.sh:61`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`) |
| `command_alias` | Continua provando atributo genérico `.create`, sem invocar alias e sem provar mecanismo específico de command. (`bin/tests/test_selo_proporcao.sh:45`, `bin/selo_proporcao.py:80`) |
| `prod_importa_teste` | Continua sem criar módulo escritor: prova a regra conservadora do import, não a costura produção→teste→escrita. (`bin/tests/test_selo_proporcao.sh:49`) |
| Preparo comum | Checkout, resultado final do script, commit e mudança de HEAD são conferidos; comandos anteriores separados por `;` ainda podem falhar e ser mascarados pelo último. (`bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:46`) |
| Preparo de `rc_bash_e` | **Continua sem conferência:** checkout, escrita, add e commit estão encadeados por `;`. A asserção do rc existe, mas pode julgar outra árvore se esse preparo falhar. (`bin/tests/test_selo_proporcao.sh:76`, `bin/tests/test_selo_proporcao.sh:77`) |
| “Cada escape” e “um mecanismo por caso” | O anúncio continua mais forte que a cobertura: persistem duplicidades e ausências específicas descritas acima. (`bin/tests/test_selo_proporcao.sh:3`, `bin/tests/test_selo_proporcao.sh:4`, `bin/tests/test_selo_proporcao.sh:38`, `bin/tests/test_selo_proporcao.sh:40`) |

**6. Quem escreve, chama e lê**

- **Produz o veredito:** Python imprime achados e retorna 0/1/2; o wrapper executa Python. (`bin/selo_proporcao.py:137`, `bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:145`, `bin/selo_proporcao.py:152`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** `test_selo_proporcao.sh` define `SELO`, chama sobre repositório sintético, captura `$?` e compara com o esperado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_selo_proporcao.sh:26`)
- **Outros leitores diretos:** as verificações de argumentos/revisões e as duas verificações de propagação do rc. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:79`)
- **Leitor indireto:** a régua chama o teste pelo glob e transforma falha do teste em rc 1 da régua. Isso testa cenários sintéticos; não classifica o diff real de pouso. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:25`)
- **Consumidor do pouso real:** o contrato exige que merge+deploy espere o rc; chamada operacional, leitor efetivo e registro no RELATO continuam **[nao verificado]**. (`contrato.txt:2`, `contrato.txt:3`, anexo) [nao verificado]
- **CORTES/RELATO:** não encontrei o registro específico PROPORCAO-POR-RISCO na busca textual desses documentos; atendimento desses itens do aval: **[nao verificado]**.
- **Pre-push:** o código declara que não roda o loop de host e chama outros selos individualmente; uso deste selo pelo hook instalado: **[nao verifi

[truncado em 20000 de 21776 caracteres]

## 11/11 13:40 · sessao eb2c0d0b2703 · - · - · origem=Ronald (tmux)

- **Juiz do servidor = cobrança, não veto.** `selfie_ausente` retorna `origem in ('app',) and not tem_foto and decorrido_min >= prazo`; prazo PWA 0 min, APK/canal desconhecido 90 min, sobreposição só APK/desconhecido. Não lê banco nem relógio. Registro vivo identifica essa autoridade. (`app/ponto/registro_batida.py:71`, `:75`, `:80`; `app/core/juizes.py:1192`)

- **Três chamadores operacionais encontrados:** API principal e API core sob `if not foto`, com `tem_foto=False, decorrido_min=0`, acrescentando motivo/contexto sem impedir escrita; retro usa canal gravado, existência `FotoBatida OR Batida.foto` e duração em minutos. (`app/api/views.py:449`, `:461`; `app/api/views_core.py:436`, `:448`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:70`, `:74`)

- **As APIs usam a mesma porta com ou sem foto:** `registrar_batida` recebe campos da batida sem exigir foto; anexação ocorre depois, somente `if foto`, por `anexar_foto(...registro='criar')`. A porta cria `Batida` sem predicado de foto; seus vetos próprios são competência/colisão, além dos retornos de deduplicação. (`app/api/views.py:665`, `:781`; `app/api/views_core.py:657`, `:776`; `app/ponto/registro_batida.py:94`, `:113`, `:132`, `:145`, `:157`)

- **POST triagem web:** escreve por `registrar_batida` antes de ler/anexar foto; `foto_sessao or request.FILES.get("foto")` só controla anexação. Não consulta `selfie_ausente` nesse ato; a cobrança poderá vir do retro, que lê o canal gravado e ambas as fontes de foto. Isso preserva diferença temporal entre casca web e APIs: web sem foto não cobra inline, API PWA cobra inline. (`app/ponto/services/triagem_batida.py:700`, `:748`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`; `app/api/views.py:461`; `app/api/views_core.py:448`)

- **Escritor persistente de foto:** `anexar_foto` escreve `Batida.foto` pela porta `anotar`; pode também criar/atualizar `FotoBatida` conforme `registro`. Modelo admite foto nula. Upload separado exige foto como insumo daquela operação, sem impor foto à criação da batida. (`app/ponto/registro_batida.py:296`; `app/ponto/models.py:78`; `app/api/views.py:847`; `app/api/views_core.py:832`; `app/ponto/views.py:2895`)

- **`foto_ausente` é motivo de cobrança**, não campo booleano de `Batida`: APIs e retro o entregam a `criar_ou_atualizar_disputa`; catálogo transforma em confirmação de presença; upload posterior procura disputas desse motivo, vincula por `contexto.ultima_batida.id`, apara/fecha pelas portas. (`app/api/views.py:464`, `:767`; `app/api/views_core.py:451`, `:762`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/chamados/catalogo/perguntas.py:93`; `app/api/foto_recebida.py:57`)

- **Fila admite null e sync omite arquivo:** escritor normaliza `foto: fotoBlob || null`; leitor só faz multipart de foto se `b.foto` truthy, mantendo exigência de Blob/File quando presente. Modelo, retro e leitores de apresentação examinados tratam ausência; não encontrei exigência de foto sempre presente nesses leitores. Consumidores externos à cópia: [nao verificado]. (`app/static/pwa.js:169`, `:180`, `:214`; `app/ponto/models.py:78`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; `app/core/views.py:1005`; `app/colaboradores/services/detalhe.py:87`; `app/templates/core/_d2_feed.html:6`)

- **Possível bloqueante remanescente real na fila:** `registrarBatidaOffline` aguarda `obterGap()` antes de abrir/gravar `batidas_pendentes`; gap ausente/velho chama `calcularESalvarGap`, cujo `fetch('/hora/')` não tem timeout. Se esse fetch permanecer pendente, a batida não chega ao `add`, embora a captura já tenha resolvido pelo teto. Incidência em produção: [nao verificado]. O Chromium não prova persistência porque substitui `registrarBatidaOffline` por espia que resolve imediatamente. (`app/static/pwa.js:85`, `:100`, `:169`, `:175`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:116`)

- **Sync não pressupõe foto:** lê fila, envia mesmo endpoint `/api/ponto/bater/`, remove após sucesso ou HTTP 400 e conserva em rede/5xx; SW delega às páginas e, sem página, notifica. Sucesso de chamada da espia não comprova esse ciclo. (`app/static/pwa.js:188`, `:214`, `:220`, `:228`, `:239`, `:384`; `app/static/sw.js:112`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:118`)

- **Lacuna anterior de transporte permanece:** fila armazena timestamp corrigido, timestamp bruto, gap, precisão e instante GPS; sincronizador envia timestamp bruto, mas não envia gap, precisão nem instante GPS. Isso não é campo de foto com dois significados; é perda de insumos no transporte. (`app/static/pwa.js:171`, `:176`; `app/static/pwa.js:205`)

- **Testes de servidor existentes:** sem foto cria batida e disputa, com foto não cria motivo `foto_ausente`; juiz testa limiares e retro lê ambas as fontes. Execução nesta revisão: [nao verificado]. (`app/api/tests/test_api_bater_pwa.py:64`, `:95`; `app/ponto/tests/test_selfie_ausente_juiz.py:27`, `:34`, `:103`, `:111`)

## 11/11 13:36 · sessao 578de191b847 · - · - · origem=Ronald (tmux)

- **Quem escreve o veredito:** Python imprime achados e devolve 1 para B com achados, 0 para restante julgado e 2 para erros tratados; o wrapper faz `exec python3`. (`bin/selo_proporcao.py:134`, `bin/selo_proporcao.py:137`, `bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:145`, `bin/selo_proporcao.py:152`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** o teste define `SELO`, chama o wrapper para o repositório sintético, captura `$?` e compara com o esperado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_selo_proporcao.sh:26`)
- **Leitores adicionais no teste:** argumentos/classes/revisões são comparados com 2; a chamada com `bash -e` agora tem resultado comparado explicitamente com 1, assim como a chamada direta. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:72`, `bin/tests/test_selo_proporcao.sh:73`, `bin/tests/test_selo_proporcao.sh:74`, `bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:79`)
- **Leitor indireto:** a régua chama todos os `tests/test_*.sh`, portanto inclui este teste quando alcança o loop; falha do teste vira rc 1 da régua. O repositório julgado pelo teste é sintético, não o diff de um pouso real. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`)
- **Pouso real:** não encontrei chamada operacional, captura do rc ou registro PROPORCAO em CORTES/RELATO na busca textual do código/docs da cópia, excluindo `.git`, `logs/` e `app/logs/`; integração operacional/manual **[nao verificado]**. As únicas referências literais encontradas a `selo_proporcao`/`PROPORCAO-POR-RISCO` pertencem aos três arquivos do instrumento; exaustividade frente a chamada construída dinamicamente **[nao verificado]**. [nao verificado]
- **Pre-push:** declara que não executa o loop de host e chama individualmente outros selos; instalação efetiva e uso deste selo em produção **[nao verificado]**. (`bin/pre-push.sh:14`, `bin/pre-push.sh:15`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`)
- **Atalho da régua permanece:** `FP` considera apenas `.py`/`.html` em `app/`; `ARVORE_FP` é calculado, mas o retorno “JA VERDE” compara somente `FP` e status e ocorre antes do loop de host. Assim, mudar somente `bin/` pode conservar esse atalho, condicionado a carimbo anterior OK. (`bin/regua.sh:26`, `bin/regua.sh:33`, `bin/regua.sh:38`, `bin/regua.sh:45`, `bin/regua.sh:84`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)
- **Worktree:** o próprio teste resolve seu diretório e passa o repo sintético explicitamente; execução em worktree **[nao verificado]**. O problema independente de outro teste permanece: `test_hook_nao_e_copia.sh` exige `.git/hooks/pre-push` e falha se ausente; o teste vizinho já consulta `core.hooksPath`/diretório comum. (`bin/tests/test_selo_proporcao.sh:7`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:27`, `bin/tests/test_commit_so_o_declarado.sh:56`, `bin/tests/test_commit_so_o_declarado.sh:57`)

## 11/11 13:33 · sessao 3d47551f7e4d · O168 · censo · origem=Code

O buraco está confirmado na cópia: o supra consulta `pode_executar_csf`; o cartório reconcilia sem essa guarda, inclusive antes do bloco que protege respostas textuais. A retirada do `--executar` continua condicionada à guarda no evento e à prova de `CSF_RETRATADO=0`. (`app/ponto/management/commands/supra_juiz.py:261`; `app/ponto/services/cartorio.py:1525`; `app/ponto/services/cartorio.py:1559`; `app/ponto/services/cartorio.py:1623`; `app/docs/BACKLOG.md:1395`.)

O raspado fornecido não contém a medição atual nem resultados de testes: **produção atual, zero e aprovação da suíte [nao verificado]**. [nao verificado]

1. **Guarda e predicados de conversa**

`pode_executar_csf(ch)` permite executar exatamente quando **nenhuma** pergunta das duas relações abaixo tem `respondida_em is not None`:

- Todas as perguntas de **todas** as disputas em `ch.disputa_origem.all()`.
- Todas as perguntas de `ch.perguntas_que_resolvem.all()`.

Relações vazias permitem executar. Não exige texto, pergunta viva, disputa aberta, validação, materialização, data ou `PerguntaDisputa.conversa=True`; também não consulta mensagens de `FollowupChamado`. (`app/ponto/supra_juiz.py:539`.)

O único chamador de produção encontrado dessa função é o comando supra; a importação está em `:257` e a chamada em `:261`. (`app/ponto/management/commands/supra_juiz.py:257`.)

Há outra implementação de proteção por resposta no cartório:

| Função | Predicado | Universo e uso |
|---|---|---|
| `pode_executar_csf(ch)` | Ausência de qualquer `respondida_em != None`. | Duas relações completas acima; protege o retratador do supra. (`app/ponto/supra_juiz.py:543`; `app/ponto/management/commands/supra_juiz.py:261`.) |
| `_tem_resposta_do_colab(perguntas)` | `any((q.resposta_colab or '').strip() for q in perguntas)`. | Nos dois leitores recebe somente `perguntas_que_resolvem`; protege FIO-DUP/ressuscitação, depois dos fechamentos CSF. Ignora timestamp e `disputa_origem`. (`app/ponto/services/cartorio.py:1623`; `:1631`; `:1634`; `:1651`.) |

Os seguintes predicados relacionados **não respondem à mesma pergunta com o mesmo universo**:

| Função | Predicado e universo |
|---|---|
| `PerguntaDisputa.respondida` | Timestamp presente; uma pergunta. (`app/chamados/models.py:3496`.) |
| `fio_vivo(perguntas)` | Alguma pergunta viva segundo `pergunta_viva`; coleção fornecida pelo chamador. (`app/chamados/juizes.py:1488`.) |
| `DisputaSupervisao.respostas_sem_veredito` | Perguntas da disputa sem validação/materialização, com timestamp **ou** texto, fora da tranca salvo opção explícita. (`app/chamados/models.py:1826`; `:1842`.) |
| `DisputaSupervisao.pode_fechar` | Nenhuma resposta sem veredito e nenhum fio vivo; uma disputa. (`app/chamados/models.py:1848`.) |
| `irmas_respondidas_sem_consumo(ch)` | Timestamp e texto presentes, sem validação/materialização. Crédito ao destino `ch`, ou ao pai `ch` quando o destino não está vivo. (`app/chamados/juizes.py:2012`.) |
| `retencao_da_retratacao(ch)` | Irmãs anteriores ou justificativa alheia pendente durante outra aprovação. Não protege toda resposta histórica. (`app/chamados/reconciliador.py:437`.) |
| `disputa_viva_do_chamado(ch)` | Primeira disputa direta aberta sem perguntas ou com fio vivo. (`app/chamados/juizes.py:2371`.) |
| Arquivador `Command._disputa_viva(ch)` | Uma disputa aberta resolvida pelo fio, com alguma pergunta viva; erro retorna sentinela conservadora. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:148`.) |
| `_disputa_do_chamado(ch)` | Localiza uma disputa aberta, primeiro direta, depois por pergunta de destino; não mede resposta. (`app/chamados/services/fio.py:120`.) |
| `canonica_do_grupo(perguntas)` | Prioriza texto não branco e depois menor PK; universo de perguntas do mesmo fato. (`app/chamados/services/fio.py:1225`.) |

O registro vivo distingue pergunta viva, disputa, fio e outras autoridades; não declara `pode_executar_csf` como autoridade da família chamado nesse bloco. (`app/core/juizes.py:272`.) Ausência de qualquer outro respondedor indireto ou dinâmico em toda a árvore: [nao verificado].

2. **EVENTO: portas do cartório**

A entrada por batida chama `_julgar_dias`; `julgar_celula` entrega o julgamento a `julgar_colab`. (`app/ponto/signals.py:132`; `app/ponto/services/cartorio.py:1997`; `:2026`.)

O cartório usa a ata deste julgamento. Seu `_protesta` exige `cel.trabalha is not False` e `marcos_acesos(...) is not True`; `classificar_dia` recebe `len(_protestam)`, não simplesmente todos os chamados vivos. Por isso proteger somente o `if COBRANCA_SEM_FURO` deixa de fora o fechamento por marco que pode ocorrer antes. (`app/ponto/services/cartorio.py:1422`; `:1444`; `:1450`; `:1525`.)

Os seis sítios diretos de reconciliação são:

| Porta | Quando fecha | Guarda de conversa hoje |
|---|---|---|
| Adiados após lavra → E2 | Reconcilia o lote adiado porque havia fio vivo. | Não consulta `pode_executar_csf`. (`app/ponto/services/cartorio.py:987`; `:993`; `:998`.) |
| RECONCILIA-MARCO | Não protesta: marcos acesos ou `trabalha=False`, descontados os adiados. | Não consulta a guarda. (`app/ponto/services/cartorio.py:1519`; `:1525`.) |
| MARCO-INEXISTENTE | Marcos cobrados deixaram de existir na ata atual. | Não consulta a guarda; anexa nota própria. (`app/ponto/services/cartorio.py:1536`; `:1538`; `:1542`.) |
| RECONCILIA por código | `COBRANCA_SEM_FURO` **ou** `COBRANCA_EM_DIA_SEM_TRABALHO`. | Não consulta a guarda. (`app/ponto/services/cartorio.py:1551`; `:1559`.) |
| FIO-DUP | Todas as perguntas diretas têm via duplicata. | Guarda textual local, diferente da do supra. (`app/ponto/services/cartorio.py:1634`; `:1636`; `:1637`.) |
| FIO-DUP-C8 | Fecha outros duplicados apoiados no sobrevivente escolhido pelo gate. | Mesma guarda textual local. (`app/ponto/services/cartorio.py:1647`; `:1651`; `:1652`.) |

As quatro primeiras não têm guarda de conversa no chamador; todas passam pelas guardas gerais de premissa e pergunta viva em `reconciliar`, que são predicados diferentes. (`app/chamados/models.py:651`; `:682`; `:685`.)

Também existem estes caminhos indiretos; não devem ser confundidos automaticamente com o motivo CSF:

- **E2 → disputa → pai:** o cartório carimba perguntas, E2 fecha disputas e o signal reconcilia o chamado-pai. (`app/ponto/services/cartorio.py:993`; `app/chamados/reconciliador.py:595`; `:611`; `app/chamados/signals.py:42`.)
- **Cluster:** o cartório chama `cluster_espurio.reconciliar_colab`, que usa a mesma porta para outro motivo. (`app/ponto/services/cartorio.py:1028`; `app/ponto/services/cluster_espurio.py:189`.)
- **Pai acordado após fecho:** o signal pode agendar `reconciliar_chamados_colab`, que executa E2, órfãs e premissa morta. (`app/chamados/signals.py:350`; `:368`; `:369`; `app/chamados/reconciliador.py:1432`; `:1433`; `:1441`.)
- **Lavra → EventoCelula → consumidor:** pode retratar por absolvição da célula, premissa morta ou lastro. Nenhum desses caminhos consulta hoje a guarda do supra. (`app/ponto/portas/celula.py:796`; `:896`; `app/chamados/reconciliador.py:1368`; `:1372`; `:1383`; `app/ponto/services/lastro.py:400`.)

3. **VARREDURA, barrados e contador**

`classificar_dia` não recebe objetos de chamado nem consulta conversa. O CSF puro exige:

`tipo_dia == 'trabalho' and prev and real >= prev and chamados_vivos and todas_casadas`

`Todas_casadas` exige células não vazias e número de casadas igual ao total. (`app/ponto/supra_juiz.py:204`; `:463`; `:467`.)

O comando seleciona colaboradores ativos da empresa, pula quem não possui batidas na janela e carrega chamados dos `FURO_MODULES`, associados ao dia pelo juiz do catálogo. (`app/ponto/management/commands/supra_juiz.py:110`; `:113`; `:117`; `:186`; `:199`.)

Hoje há duas decisões distintas:

- A classificação local alimenta placar, exemplos e alarme.
- A escrita obedece à **lavra** `cobranca_indevida` e aos códigos dos protestos; sem lavra conta `CSF_SEM_LAVRA`, sem código conta `CSF_LAVRA_SEM_CODIGO`, sem retratar. (`app/ponto/management/commands/supra_juiz.py:239`; `:246`; `:248`; `:250`; `:253`.)

A guarda roda antes da cobrança órfã e também em dry-run. Chamado barrado soma `CSF_COM_CONVERSA_PRESERVADO` e recebe `continue`; não chega a `retratar`. (`app/ponto/management/commands/supra_juiz.py:261`; `:263`; `:275`.)

Com `--executar` e trava T6.1 desligada, chama o retratador. Retorno verdadeiro soma `CSF_RETRATADO`; falso soma `CSF_RETRATACAO_RECUSADA`; sem flag soma `CSF_RETRATAVEL_DRY`. (`app/ponto/management/commands/supra_juiz.py:45`; `:279`; `:286`; `:288`; `:290`; `:295`.)

| Operação sobre `CSF_RETRATADO` | Local |
|---|---|
| Nasce como chave possível de `Counter` local, por execução. | `app/ponto/management/commands/supra_juiz.py:105` |
| Única escrita do contador encontrada: incremento após retorno verdadeiro. | `app/ponto/management/commands/supra_juiz.py:288` |
| Leitura para stdout por `most_common()`. Chave não incrementada não gera linha explícita de zero. | `app/ponto/management/commands/supra_juiz.py:299` |
| Testes leem presença/ausência da chave. | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:122`; `:153`; `:167` |

O alarme A6 não lê esse contador: consulta `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`. O cron ainda contém `supra_juiz --executar`, dependente do cartório. (`app/ponto/management/commands/supra_juiz.py:305`; `app/config/crons.py:599`.)

4. **Porta comum e alcance das alternativas, sem escolher**

O caminho comum é:

`cartório → reconciliar`  
`supra → retratar → _retratar → reconciliar`  
`reconciliar → _decisao_do_fechamento → _gravar_estado`

(`app/ponto/services/cartorio.py:1559`; `app/ponto/management/commands/supra_juiz.py:286`; `app/chamados/reconciliador.py:544`; `:523`; `app/chamados/models.py:651`; `:654`.)

`_gravar_estado` escreve status, carimbo de resolução, via e urgência. A reconciliação usa via **`sistema`**, não uma via exclusiva de CSF ou cartório. (`app/chamados/models.py:201`; `:228`; `:242`; `:654`; `app/chamados/catalogo/motor.py:95`.)

**Dentro da decisão da porta:** `_decisao_do_fechamento` é compartilhada por `reconciliar(registrar=True)` e `reconciliaria(registrar=False)`. Uma guarda ali mantém ato e previsão juntos, mas, se incondicional, muda toda reconciliação. (`app/chamados/models.py:651`; `:661`; `:663`.)

**Nos dois caminhos:** preserva os demais usos da porta, mas cartório representa vários sítios e efeitos indiretos, não somente `:1559`; precisa impedir que o mesmo chamado protegido feche por marco, adiado, signal ou consumidor. (`app/ponto/services/cartorio.py:998`; `:1525`; `:1559`; `app/chamados/signals.py:42`; `app/chamados/reconciliador.py:1368`.)

Censo dos demais chamadores **diretos** de `reconciliar`, fora do cartório:

| Arquivo | Função e sítios |
|---|---|
| `app/chamados/signals.py` | `disputa_fechada_fecha_chamado_pai`:42 |
| `app/chamados/reconciliador.py` | `_retratar`:523 |
| `app/chamados/services/adesao.py` | `sincronizar_adesao`:115,150 |
| `app/chamados/services/feriado_reavaliar.py` | `encerrar`:47 |
| `app/chamados/services/limbo_folgas.py` | `aceitar_sugestao`:101; `agrupar_furo`:283,286 |
| `app/chamados/services/silencio_ausencia.py` | `reconciliar_por_ausencia`:101 |
| `app/chamados/services/materializacao.py` | `_materializar_ausencia_de_resposta`:587; `_reconciliar_destino_e_clones/_fecha`:776 |
| `app/chamados/management/commands/auditar_chamados_orfaos.py` | `handle`:67 |
| `app/colaboradores/services/pos_vinculo.py` | `fechar_adesao_com_batida`:42 |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `_marcar`:140 |
| `app/ponto/services/triagem_batida.py` | `_autoresolve_f2_f4`:597 |
| `app/ponto/services/veredito_celula.py` | `fechar_o_fio_do_dia`:202 |
| `app/ponto/services/ausencia.py` | `fechar_chamado_da_ausencia`:694 |
| `app/ponto/services/cluster_espurio.py` | `reconciliar_colab`:189 |
| `app/ponto/services/entrada_sincronizada.py` | `resolver_cobranca_da_entrada`:31 |
| `app/ponto/services/cobertura.py` | `designar_cobertura`:122; `declarar_cobertura_passada`:317 |
| `app/ponto/services/lastro.py` | `fechar_pelo_marco_colab`:400 |
| `app/ponto/management/commands/detectar_intervalo_ausente.py` | `handle`:138 |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `_ata_sem_furo`:151 |
| `app/ponto/management/commands/detectar_ausencias.py` | `handle`:158; `_fechar_por_afastamento`:824; `_fechar_por_cobertura`:834; `_fechar_chamado`:845 |

A mudança dentro de `_decisao_do_fechamento` afeta também estes leitores de previsão: lastro, vigia de reconciliação e dry/alarme de intervalo. (`app/ponto/services/lastro.py:318`; `app/chamados/management/commands/reconciliar_chamados.py:79`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`.)

Uma guarda somente em `retratar` não alcança o cartório direto. Além do supra, esse wrapper atende justificativa, geofence, grade, fantasmas, vínculo, alertas de turno e retratação por lei; `_retratar` tem ainda entradas internas de absolvição e premissa morta. (`app/ponto/services/justificativa.py:46`; `app/ponto/services/geofence.py:441`; `app/ponto/management/commands/reconciliar_grade.py:309`; `app/ponto/management/commands/reconciliar_fantasmas.py:280`; `app/ponto/management/commands/reconciliar_vinculo.py:68`; `app/ponto/management/commands/processar_alertas_turno.py:216`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`; `app/chamados/reconciliador.py:1004`; `:1464`.)

Uma guarda indiscriminada em `_gravar_estado` alcançaria também transição humana, cancelamento, análise, reabertura, cascade e tranca. `encerrar()` é outra porta; não é a chamada direta usada pelo CSF desses dois caminhos. (`app/chamados/models.py:450`; `:788`; `:806`; `:1442`; `:1619`; `:1639`; `app/chamados/reconciliador.py:523`.)

5. **Testes e selos existentes** [nao verificado]

| Cobertura | Referências |
|---|---|
| Guarda CSF: vazio, sem timestamp, resposta na disputa e resposta direta. | `app/ponto/tests/test_supra_juiz_executor.py:38` |
| Classificação CSF: todas casadas; missing, `None`, sem vivo e realizado insuficiente. | `app/ponto/tests/test_supra_juiz_cobranca.py:19` |
| `--executar` real: lavra concorde preserva; indevida retrata; lavra prevalece sobre classificação local; ausência de lavra/código; recusa não conta como retratação. | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89`; `:106`; `:116`; `:124`; `:134`; `:142`; `:155` |
| T6.1: constante única, valor vigente, flag/trava, placar travado e texto delegado ao juiz. | `app/ponto/tests/test_t61_retratacao_em_seco.py:29`; `:40`; `:47`; `:57`; `:64` |
| Cartório: CSF na passada pura, fechamento aplicado e fio não ressuscitado. | `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`; `:412`; `:416`; `:420` |
| Batida real → cartório → E2 → encerramento sem pergunta viva pendurada em terminal. | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`; `:390` |
| Porta geral: recusa pergunta viva; permite reata; dry sem escrita. | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; `:94`; `:135`; `:143`; `:152` |
| Consumidor/lastro: marco satisfeito fecha apesar de outro furo; marco apagado permanece vivo. | `app/ponto/tests/test_bug115_lampada_do_marco.py:185`; `:207` |
| Universo cartório/supra e exclusão de chamado sem dia. | `app/ponto/tests/test_cartorio_so_chamado_de_batida.py:53`; `:63`; `:74` |
| Texto protege contra ressuscitação; não demonstra proteção CSF. | `app/ponto/tests/test_hx_cartorio_ressuscita.py:68` |
| Custo do caminho real e da guarda, com chamados fechando ou permanecendo. | `app/ponto/tests/test_selo_chokepoint_escrita.py:387`; `:392`; `:422`; `:427`; `:432` |

Esses casos não constituem prova executada da guarda CSF no evento; execução e resultado nesta cópia [nao verificado].

6. **Lista fechada para conferência de arquivos em comum**

Sem escolher entre guarda na porta e nos caminhos, uma lista única exata de alterações futuras seria presumir a implementação. Abaixo está a **união fechada de reserva proposta** para as duas alternativas, incluindo instrumentação e testes; não é um diff realizado. Alterações futuras [nao verificado]. [nao verificado]

| Arquivo reservado | Razão e sítio atual |
|---|---|
| `app/chamados/juizes.py` | Destino possível do predicado compartilhado; juízes de leitura já residem aqui. (`:1980`; `:2021`.) |
| `app/core/juizes.py` | Registrar o endereço da autoridade existente. (`:272`.) |
| `app/ponto/supra_juiz.py` | Extrair/reexportar a guarda. (`:539`.) |
| `app/ponto/management/commands/supra_juiz.py` | Consumir autoridade e tornar a medição explícita. (`:257`; `:288`; `:299`.) |
| `app/ponto/services/cartorio.py` | Caminhos diretos e observabilidade do evento. (`:998`; `:1525`; `:1559`.) |
| `app/chamados/models.py` | Alternativa de guarda na decisão comum e contexto da trilha. (`:663`; `:292`.) |
| `app/chamados/reconciliador.py` | Retratador, E2 e consumidor pós-lavra na alternativa por caminhos. (`:503`; `:549`; `:1368`.) |
| `app/chamados/signals.py` | Fechamento indireto do pai na alternativa por caminhos. (`:42`.) |
| `app/ponto/services/lastro.py` | Fechamento pelo consumidor na alternativa por caminhos. (`:400`.) |
| `app/config/crons.py` | Segunda etapa: retirar execução somente depois da prova exigida. (`:599`; `app/docs/BACKLOG.md:1395`.) |
| `app/ponto/tests/test_supra_juiz_executor.py` | Matriz exata do predicado. (`:38`.) |
| `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py` | Integração do comando e contador. (`:89`; `:155`.) |
| `app/ponto/tests/test_o261_cobrado_le_o_fio.py` | CSF no cartório. (`:393`.) |
| `app/chamados/tests/test_o261e_f1_porta_encerramento.py` | Porta comum, dry e evento real. (`:152`; `:358`.) |
| `app/ponto/tests/test_bug115_lampada_do_marco.py` | Fechamento alternativo por marco/consumidor. (`:185`.) |
| `app/ponto/tests/test_hx_cartorio_ressuscita.py` | Diferença entre texto e timestamp no bloco do fio. (`:68`.) |
| `app/ponto/tests/test_selo_chokepoint_escrita.py` | Custo da guarda no caminho quente. (`:432`.) |
| `app/docs/JUIZES.md` | Ponteiro documental da autoridade movida. (`:10`.) |

Arquivos das outras obras — E4B c6, TRIAGEM, UI-PAINEL e BO-ROTULO — não vieram no dado; interseção e liberação para abrir construtor [nao verificado].

7. **Sonda somente leitura**

A medição histórica **exata** solicitada não é recuperável pelo esquema mostrado: a trilha grava status anterior, status novo e via, sem motivo CSF ou sítio chamador; o ator identifica processo/request, não a pilha. Além disso, relações e timestamps atuais podem ter mudado: ressuscitação limpa `respondida_em`. (`app/chamados/models.py:292`; `app/core/observ.py:22`; `:94`; `app/chamados/models.py:1563`.)

`CSF_RETRATADO` também não possui valor atual persistido: é contador local impresso pelo comando. Portanto a sonda mantém ambos os resultados exatos como **“nao medido”** e entrega separadamente o que o banco permite: um **proxy** de reconciliações atribuídas ao processo `processar_cartorio`, com a guarda barrando hoje e timestamps anteriores ao fecho nas relações atuais. Isso não identifica todos os eventos do cartório nem prova motivo CSF. (`app/ponto/management/commands/supra_juiz.py:105`; `:288`; `:299`; `app/core/observ.py:31`; `app/chamados/models.py:292`.)

O agrupamento abaixo usa **motivo da pergunta**, não motivo do fechamento; um evento com motivos diferentes entra em mais de um grupo. Os campos vêm de `PerguntaDisputa` e da trilha; a avaliação atual chama o predicado real. (`app/chamados/models.py:2432`; `app/core/models.py:298`; `:300`; `:307`; `app/ponto/supra_juiz.py:539`.)

Código para fornecer ao `bin/sonda_leitura.sh <arquivo.py>`; produção é o padrão e o wrapper executa shell Django no schema configurado. Não foi executado em produção [nao verificado]. (`bin/sonda_leitura.sh:56`; `:93`; `:235`.)

```python
import json

resultado = {
    "CSF_CARTORIO_30D_POR_EMPRESA_E_MOTIVO": "nao medido",
    "CSF_RETRATADO": "nao medido",
    "proxy": "nao medido",
}
tr

[truncado em 20000 de 25069 caracteres]

## 11/11 13:32 · sessao fadf31a99bdf · MAPA-20 · censo · origem=Code

A premissa de escritor único é contradita pelo anexo: ele relata geração pelo montador e edição externa por `PRONTO-AUSENTE-RASCUNHO`. O código continua usando essa exclusividade como fundamento do parser (`opus_final_ffa_achados.txt:6`; `bin/mapa20_valida.py:10`).

As referências aos anexos abaixo contam as linhas do texto fornecido: cada aval ocupa a linha 1; no opus, o cabeçalho é a linha 1 e os cinco bloqueantes ocupam as linhas 2–6. A identidade Git da cópia, o mapa vivo, o montador e o estado publicado estão [nao verificado]. [nao verificado]

**1. Escritores**

| Escritor/caminho | Evidência e limite |
|---|---|
| `logs/MAPA-20/montar_mapa.py` | Declarado como produtor em `bin/mapa20_valida.py:10` e `bin/mapa20_valida.py:128`. Implementação e linha que grava o mapa: [nao verificado]. |
| Edição externa `PRONTO-AUSENTE-RASCUNHO` | Segundo escritor informado pelo anexo; comando, arquivo executor e linha da gravação: [nao verificado] (`opus_final_ffa_achados.txt:6`). |
| Outro script de produção | [nao verificado]. |
| Escritores das fixtures | Regravação em `bin/tests/test_mapa20_valida.sh:154`, cópia verde em `:177` e escrita vermelha em `:178`; todos no repositório temporário criado em `:146`. |
| Publicador | Lê o mapa e escreve uma foto temporária; essa operação não regrava o mapa de origem (`bin/relato.sh:94`, `bin/relato.sh:97`). |

Portanto, “único escritor = montador” não está provado: é premissa do código, contrariada pelo dado fornecido (`bin/mapa20_valida.py:10`; `opus_final_ffa_achados.txt:6`).

A lei exige **“1 escritor canonico + tripwire”** (`CLAUDE.md:32`). Os dois avais determinam regravação por evento e julgamento no publicador, mas não nomeiam um escritor exclusivo; escolher montador, edição manual ou outro por Ronald seria [nao verificado] (`aval_mapa_ate_o_20_1111.txt:1`, item 3; `aval_mapa20_pronto_x_de_n_1111.txt:1`, item 4). [nao verificado]

**2. Matriz da porta**

Convenções de publicação:

- **A — aceita:** foto selecionada, raspada, adicionada ao Git e enviada; rc final 0 se as etapas seguintes concluírem (`bin/relato.sh:97`, `:98`, `:143`, `:153`, `:164`, `:165`).
- **R — retém:** candidato excluído; cópia anterior, se houver, é raspada novamente; resto segue; rc final 1 (`bin/relato.sh:100`, `:102`, `:105`, `:158`, `:165`).
- Os pares de rc abaixo são **validador → publicador**, supondo sucesso das etapas posteriores. Falhas posteriores também podem produzir rc 1 (`bin/mapa20_valida.py:225`; `bin/relato.sh:143`, `:163`, `:164`).

| Estado de entrada | Juiz → operação | Publicação | rc | Prova que morde / lacuna |
|---|---|---|---|---|
| Tabela sintaticamente válida | Cabeçalho, estrutura, células, X/N e universo (`bin/mapa20_valida.py:154`, `:166`, `:178`, `:190`, `:202`) | A | 0→0 | Verde em `bin/tests/test_mapa20_valida.sh:42`; igualdade X=N em `:43`. Não prova a verdade de X nem de N (`bin/mapa20_valida.py:190`, `:192`). |
| Obra ausente ou coluna vazia | Acumula problema (`bin/mapa20_valida.py:178`, `:202`) | R | 1→1 | Cada obra e SEM DONO: `bin/tests/test_mapa20_valida.sh:45`, `:46`, `:47`. |
| “encerrada” em célula, X<N | `_afirma_encerrada` e comparação de completude (`bin/mapa20_valida.py:68`, `:200`) | R | 1→1 | `bin/tests/test_mapa20_valida.sh:55`; PRONTO AUSENTE encerrada em `:56`; ênfase em `:65`, `:66`. |
| “encerrada” fora da tabela: ATX, setext, negrito ou lista, separados da tabela | **SEM juiz de estado:** só percorre células do bloco (`bin/mapa20_valida.py:147`, `:166`, `:200`) | A | 0→0 | **SEM PROVA** dessas quatro formas. Os casos `bin/tests/test_mapa20_valida.sh:62` e `:63` tratam outras tabelas, não essa prosa. |
| Invisível da categoria Cf, como U+200B | Recusa global por categoria Unicode (`bin/mapa20_valida.py:132`) | R | 1→1 | `bin/tests/test_mapa20_valida.sh:71`. |
| Combinantes U+034F/U+FE00 ou homóglifo cirílico em “encerrada” | **SEM juiz dessas classes:** filtro só Cf; reconhecimento literal do estado (`bin/mapa20_valida.py:58`, `:132`) | A | 0→0 | **SEM PROVA** dessas classes; o teste vizinho cobre somente U+200B (`bin/tests/test_mapa20_valida.sh:71`). |
| FF, FS ou U+2028 entre E3 e E4B, sem LF | `splitlines()` cria duas linhas para o juiz; **SEM juiz da divergência com GFM** (`bin/mapa20_valida.py:125`) | A | 0→0 | **SEM PROVA**; contracaso descrito em `opus_final_ffa_achados.txt:4`. |
| N com fonte alheia a aval/TICKETS/BACKLOG | `_fonte_ruim` confere existência, raiz e linha não vazia; não confere elegibilidade, vínculo à obra ou contagem (`bin/mapa20_valida.py:73`, `:78`, `:88`) | A | 0→0 | **SEM PROVA da regra de fonte.** O verde usa o comentário `bin/relato.sh:2` como fonte (`bin/tests/test_mapa20_valida.sh:27`, `:42`). |
| SEM DONO com `n/a (SEM DONO)` | Confere literal e executa `continue` (`bin/mapa20_valida.py:182`, `:185`) | A | 0→0 | Presença/vazias em `bin/tests/test_mapa20_valida.sh:45`; rejeição de X/N em `:72`. |
| SEM DONO com “encerrada” | **SEM juiz de encerramento:** sai antes da guarda (`bin/mapa20_valida.py:185`, `:200`) | A | 0→0 | **SEM PROVA** desse estado; `bin/tests/test_mapa20_valida.sh:72` testa outro conteúdo. |
| RASCUNHO como título/prosa antes de tabela válida | **SEM juiz específico:** prosa sem pipes não entra na avaliação de estado (`bin/mapa20_valida.py:137`, `:144`, `:166`) | A | 0→0 | **SEM PROVA** específica. Se substituir X/N por `RASCUNHO`, é recusado pela gramática (`bin/mapa20_valida.py:198`). Motivo do rc 1 do mapa vivo: [nao verificado]. |
| Legenda literal de `montar_mapa.py:103` | Literal e implementação: [nao verificado] | [nao verificado] | [nao verificado] | **SEM PROVA** específica; exigência de preservá-la consta de `opus_final_ffa_achados.txt:3`. |
| X declarado sem prova escrita no RELATO | **SEM juiz de lastro:** compara inteiros e verifica somente a citação de N (`bin/mapa20_valida.py:190`, `:192`) | A | 0→0 | **SEM PROVA** de correspondência item→prova; fixture usa falta genérica em `bin/tests/test_mapa20_valida.sh:27`. |
| `status/encerrada.sim` em célula | Token removido como caminho antes da busca por estado (`bin/mapa20_valida.py:52`, `:65`) | A | 0→0 | **SEM PROVA** desse literal. Há teste de `logs/encerrada.md:10` em `bin/tests/test_mapa20_valida.sh:102`; correspondência ao `test:414` do opus: [nao verificado]. |

Os 105 casos existentes de conteúdo foram avaliados em memória contra `problemas()`, sem divergências das expectativas codificadas; isso cobre as fixtures de `bin/tests/test_mapa20_valida.sh:42` até `:114`, não execução integral do shell, publicação ou render. O shell cria arquivos e repositórios, portanto não foi executado nesta leitura (`bin/tests/test_mapa20_valida.sh:13`, `:37`, `:169`).

**Transições depois do julgamento**

| Entrada/decisão | Operação → resultado previsto | rc ao chamador | Prova |
|---|---|---|---|
| Candidato aprovado | Raspa, adiciona, commita e envia (`bin/relato.sh:143`, `:153`, `:163`, `:164`) | 0 (`bin/relato.sh:165`) | `bin/tests/test_mapa20_valida.sh:196`, `:197`. |
| Candidato recusado ou ausente | Conserva conteúdo anterior após nova raspagem; restante pode publicar (`bin/relato.sh:100`, `:105`) | 1, inclusive sem mudanças (`bin/relato.sh:158`, `:165`) | `bin/tests/test_mapa20_valida.sh:198`, `:199`, `:200`. |
| Raspagem/rename do retido falha | Remove cópia local; remoção entra no commit (`bin/relato.sh:109`, `:152`, `:153`) | 1 (`bin/relato.sh:165`) | `bin/tests/test_mapa20_valida.sh:201`. |
| Remoção também falha | Interrompe antes do commit/push (`bin/relato.sh:110`) | 1 | `bin/tests/test_mapa20_valida.sh:204`. |
| Vivo muda depois da foto | Publica foto anterior à regravação (`bin/relato.sh:97`, `:98`) | 0 no cenário testado | `bin/tests/test_mapa20_valida.sh:154`, `:203`. |
| Raspagem transforma candidato aprovado | Texto alterado passa só pela conferência de privacidade; **SEM novo julgamento MAPA-20** (`bin/raspar_publico.py:31`, `:36`, `:40`; `bin/relato.sh:143`) | Pode terminar 0 (`bin/relato.sh:165`) | **SEM PROVA de conformidade após raspagem**; raspador falso apenas prefixa texto (`bin/tests/test_mapa20_valida.sh:166`, `:189`). |
| Commit falha | Interrompe antes do push (`bin/relato.sh:163`) | 1 | `bin/tests/test_mapa20_valida.sh:202`. |
| Push falha | Retorna falha; estado remoto resultante: [nao verificado] (`bin/relato.sh:164`) | 1 | **SEM PROVA específica** entre os casos de `bin/tests/test_mapa20_valida.sh:196`–`:204`. |

**3. GFM e confronto com o aval nas lacunas**

As referências literais de lei são:

- **A3:** “o publicador sai rc 1 se faltar no mapa alguma obra […] ou se uma linha tiver coluna vazia sem ‘nao medido’” (`aval_mapa_ate_o_20_1111.txt:1`, item 3). [nao verificado]
- **B1:** “N = itens numerados do PRONTO da obra”, com fonte no aval da obra, TICKETS ou BACKLOG; “X = itens com prova escrita no RELATO” (`aval_mapa20_pronto_x_de_n_1111.txt:1`, item 1).
- **B3/B4:** “‘encerrada’ so com X == N”; “rc 1 […] se aparecer ‘encerrada’ com X < N” (`aval_mapa20_pronto_x_de_n_1111.txt:1`, itens 3–4).

| Lacuna | O que chega ao leitor GFM / confronto |
|---|---|
| Encerrada fora da tabela | O anexo relata título ATX/setext e negrito exibindo a afirmação que o juiz ignora; lista também fica fora do percurso das células. Com obra X<N, contraria B3/B4 (`opus_final_ffa_achados.txt:3`; `bin/mapa20_valida.py:166`, `:200`). Render remoto: [nao verificado]. |
| Mn e homóglifo | A grafia modificada passa com 1 de 2; aparência exata depende do render/fonte [nao verificado]. Se apresenta “encerrada” ao leitor, contorna B3/B4 (`opus_final_ffa_achados.txt:2`; `bin/mapa20_valida.py:58`, `:132`). |
| FF/FS/U+2028 | Segundo o anexo, GFM não faz a mesma quebra e E4B perde sua linha visível; contraria A3, apesar da presença lógica para Python (`opus_final_ffa_achados.txt:4`; `bin/mapa20_valida.py:125`). |
| Fonte imprópria ou N inventado | A tabela pode exibir X/N e uma citação existente que não fundamenta N; contraria B1. O próprio verde legitima esse caso (`bin/tests/test_mapa20_valida.sh:27`, `:42`; `bin/mapa20_valida.py:73`). |
| X sem prova | A tabela exibe “resultados provados” sem conferência do lastro; aceitar X inventado contraria B1. Que X do mapa vivo está nessa situação: [nao verificado] (`bin/mapa20_valida.py:190`, `:192`). |
| SEM DONO encerrada | O texto “encerrada” pode coexistir com `n/a (SEM DONO)`; o código não demonstrou X=N. Os avais não fornecem autorização literal dessa exceção de encerramento (`bin/mapa20_valida.py:182`, `:185`; `aval_mapa20_pronto_x_de_n_1111.txt:1`, item 3). |
| RASCUNHO | Um prefixo separado aparece junto da tabela; o rótulo sozinho não é proibido pelos avais nem dispensa A3/B1/B4. O defeito concreto do mapa vivo não foi fornecido: [nao verificado] (`bin/mapa20_valida.py:137`, `:166`; `aval_mapa20_pronto_x_de_n_1111.txt:1`). |
| Legenda literal | Conteúdo e render: [nao verificado]. Não cabe inventar a legenda nem rejeitar toda ocorrência de “encerrada” indiscriminadamente; o anexo exige que a legenda passe (`opus_final_ffa_achados.txt:3`). |
| Caminho `status/encerrada.sim` | O texto do caminho permanece no documento, embora seja apagado da análise de estado. Um nome de arquivo isolado não prova afirmação de encerramento; uso enganoso no mapa vivo: [nao verificado] (`bin/mapa20_valida.py:52`, `:65`). |
| Transformação pela raspagem | O leitor recebe os bytes transformados, sem selo MAPA-20 posterior. Violação concreta de A3/B4 pelo raspado vivo: [nao verificado] (`bin/raspar_publico.py:31`, `:40`; `bin/relato.sh:143`). |
| Retenção e falha de push | O resultado visível depende da cópia anterior e do remoto; ambos [nao verificado]. Rc 1 não demonstra atualização nem conformidade da página anterior (`bin/relato.sh:105`, `:158`, `:164`, `:165`). |

**4. Ordem de cura por dependência e perímetro fechado**

Esta é uma proposta de desenho, sem aplicar mudanças; a cura na origem e a testemunha lendo a mesma autoridade são exigências existentes (`CLAUDE.md:17`, `:20`).

1. **Definir o escritor canônico e seu tripwire.** O juiz não pode apoiar sua abrangência numa exclusividade apenas declarada (`CLAUDE.md:32`; `bin/mapa20_valida.py:10`).
2. **Ancorar N e X nas autoridades do aval.** Existência de linha não substitui lista numerada vinculada à obra; igualdade numérica não substitui prova de X (`aval_mapa20_pronto_x_de_n_1111.txt:1`, item 1; `bin/mapa20_valida.py:73`, `:190`).
3. **Ancorar o julgamento no documento que o leitor recebe.** Cobrir conteúdo visível fora da tabela, classes Unicode e separação de linhas, preservando a legenda literal quando disponível (`bin/mapa20_valida.py:125`, `:132`, `:166`; `opus_final_ffa_achados.txt:3`).
4. **Ancorar a publicação nos bytes finais julgados.** A raspagem muda texto depois do selo; o mesmo juiz MAPA-20 precisa alcançar esse resultado antes de sua publicação (`bin/relato.sh:97`, `:143`; `bin/raspar_publico.py:31`).
5. **Ancorar o tratamento operacional no resultado da porta.** Preservar o rc 1 exigido pelo aval, distinguindo retenção, publicação parcial e falha operacional para o consumidor (`aval_mapa20_pronto_x_de_n_1111.txt:1`, item 4; `bin/relato.sh:100`, `:110`, `:164`; `bin/hasner-relato.service:15`).
6. **Fazer os testes morderem cada transição e os bytes finais.** Os atuais testes de publicação substituem o raspador real (`CLAUDE.md:28`; `bin/tests/test_mapa20_valida.sh:158`, `:166`).

Perímetro fechado proposto para revisão:

| Arquivo | Papel |
|---|---|
| `logs/MAPA-20/montar_mapa.py` | Produtor referido pelo juiz; implementação e escolha como escritor canônico [nao verificado] (`bin/mapa20_valida.py:10`). |
| `logs/MAPA-20/mapa.md` | Artefato de entrada; conteúdo vivo [nao verificado] (`bin/relato.sh:97`). |
| `bin/mapa20_valida.py` | Juiz único de conformidade (`bin/mapa20_valida.py:124`). |
| `bin/relato.sh` | Foto, raspagem, retenção, Git e rc (`bin/relato.sh:94`, `:105`, `:143`, `:153`, `:165`). |
| `bin/tests/test_mapa20_valida.sh` | Regressões do juiz e das transições (`bin/tests/test_mapa20_valida.sh:42`, `:138`, `:196`). |
| `bin/hasner-relato.service` | Consumidor operacional da execução (`bin/hasner-relato.service:15`). |

O raspador e seu juiz de privacidade são dependências lidas; a proposta acima resolve a ordem no publicador, sem propor mudança na regra de privacidade (`bin/raspar_publico.py:16`, `:31`; `bin/relato.sh:143`). Fechamento executável desse perímetro antes de conhecer o produtor e a legenda: [nao verificado].

**5. Quem lê o rc**

| Consumidor | Leitura efetiva |
|---|---|
| systemd, via `hasner-relato.service` | Recebe a saída do processo `ExecStart`, declarado `oneshot`; o arquivo não configura tratamento específico de retenção ou discriminação de resultados (`bin/hasner-relato.service:13`, `:15`, `:19`, `:21`). Unit instalada e alertas externos: [nao verificado]. |
| `hasner-relato.timer` | Agenda execuções; não contém leitura ou classificação do rc (`bin/hasner-relato.timer:5`, `:6`, `:8`). |
| Teste do publicador | Executa recortes, captura `$?`, lê e compara rc com conteúdo do remoto sintético (`bin/tests/test_mapa20_valida.sh:138`, `:182`, `:183`, `:187`, `:191`). |
| Outros chamadores produtivos, aliases ou monitoramento | [nao verificado]. |

Logo, “sem leitor” deve ser qualificado: há o consumidor do processo pelo serviço e há o teste; **tratamento que interprete a retenção do MAPA-20** não aparece na unit fornecida (`bin/hasner-relato.service:15`; `bin/tests/test_mapa20_valida.sh:191`).

Rc 1 é ambíguo: representa retenção com restante publicado, nenhuma mudança, interrupção antes de publicar ou falha de push (`bin/relato.sh:100`, `:110`, `:158`, `:164`, `:165`).

Pergunta de lei pendente: **quem Ronald designa como único escritor canônico do mapa?** A obrigação de unicidade existe; a nomeação não consta dos dois avais fornecidos (`CLAUDE.md:32`; `aval_mapa_ate_o_20_1111.txt:1`; `aval_mapa20_pronto_x_de_n_1111.txt:1`).

perguntas de lei: 1

## 11/11 13:32 · sessao c24a41a8a57f · PROPORCAO-POR-RISCO · verificador · origem=Code

A v2 fecha os exemplos literais de grafia e cabeçalho da v1, mas mantém falsos verdes por seleção dos nós do AST e pela exclusão de testes. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`)

Todos os rc abaixo são **deduzidos por leitura**, sem executar selo ou suíte. Execução dos exemplos, efeitos em produção, banco e logs: **[nao verificado]**. [nao verificado]

1. **Fechamento dos achados da v1**

| Achado v1 | Situação na v2 / cura | Caso que o morde |
|---|---|---|
| Oito métodos ORM com espaço depois do ponto | **Fecha o exemplo.** Compara `Attribute.attr` com os oito nomes, independentemente da grafia. (`bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:55`) | Oito `orm_$m`, mais `espaco_no_ponto`. (`bin/tests/test_selo_proporcao.sh:28`, `bin/tests/test_selo_proporcao.sh:31`) |
| Quebra antes de `()` | **Fecha quando o atributo também é adicionado. Não fecha continuação isolada**, demonstrada adiante. (`bin/selo_proporcao.py:106`) | `quebra_de_linha` adiciona o arquivo inteiro; não cobre atributo antigo. (`bin/tests/test_selo_proporcao.sh:32`) |
| Alias `gravar = obj.save; gravar()` | **Fecha o exemplo:** a referência ao atributo basta. (`bin/selo_proporcao.py:55`) | `alias` contém somente a atribuição; prova reconhecimento da referência. (`bin/tests/test_selo_proporcao.sh:33`) |
| `getattr(obj, "save")()` | **Fecha a string literal exata.** (`bin/selo_proporcao.py:59`) | `getattr_string`. (`bin/tests/test_selo_proporcao.sh:34`) |
| `(obj.save)()` | **Fecha pelo atributo.** (`bin/selo_proporcao.py:55`) | Sem caso específico; os casos próximos não reproduzem essa forma. (`bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:32`) |
| `#` dentro de string | **Fecha:** analisa o fonte pelo AST, sem truncar no `#`. (`bin/selo_proporcao.py:101`) | `cerquilha_string`. (`bin/tests/test_selo_proporcao.sh:35`) |
| Signal por alias e `getattr` | **Fecha o exemplo v1 pelo `.connect`**, mesmo com nome de signal calculado. (`bin/selo_proporcao.py:30`, `bin/selo_proporcao.py:55`) | `signal_connect` isola esse gatilho; não reproduz o exemplo inteiro. (`bin/tests/test_selo_proporcao.sh:39`) |
| Command que escreve por alias | **Fecha o exemplo pela referência `.create`.** Continua sem regra específica de command. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:91`) | `command_alias`; não contém invocação do alias. (`bin/tests/test_selo_proporcao.sh:43`) |
| Migration por rename sem linhas adicionadas | **Fecha:** lê destino pelo protocolo NUL e aplica regra por caminho. (`bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:94`) | `migration_rename`. (`bin/tests/test_selo_proporcao.sh:42`) |
| `.py` escondido como binário/`-diff` | **Fecha o exemplo:** fonte por `git show`, diff com `--text`. (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:100`) | `binario_attr`. (`bin/tests/test_selo_proporcao.sh:44`) |
| Caminho com espaço/tabulação ou Unicode escapado | **Fecha a dependência do cabeçalho textual:** caminhos NUL e `surrogateescape`. (`bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:89`) | `caminho_espaco` e `caminho_unicode`; falta caso específico com tabulação. (`bin/tests/test_selo_proporcao.sh:45`, `bin/tests/test_selo_proporcao.sh:46`) |
| `+++ /dev/null` dentro do conteúdo | **Fecha:** caminho não vem mais de `+++`. (`bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:100`) | Sem caso correspondente na lista. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:55`) |
| Revisão interpretada como opção Git | **Fecha:** rejeita `-` inicial e resolve ambos os commits antes do diff. (`bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:80`) | `opcao_como_revisao`. (`bin/tests/test_selo_proporcao.sh:58`) |
| Produção importa módulo excluído de teste | **Parcial:** import estático adicionado é marcado; import antigo, dinâmico ou reexportado continua permitindo fuga. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`) | `prod_importa_teste` testa apenas o import, sem criar módulo escritor; não cobre `tests.writer` separadamente. (`bin/tests/test_selo_proporcao.sh:47`) |

Os achados v1 sobre isolamento/preparo dos testes estão detalhados no item 5; os de rc, consumidores, atalho da régua e worktree, nos itens 4 e 6. (`bin/tests/test_selo_proporcao.sh:20`, `bin/tests/test_selo_proporcao.sh:38`, `bin/regua.sh:86`, `bin/tests/test_hook_nao_e_copia.sh:21`)

2. **Escapes da v2**

A expressão precisa é “nenhum **nó reconhecido como escrita** cruza a linha adicionada”: `Call`, `Expr`, `FunctionDef` e `Lambda` podem cruzá-la, mas não são candidatos em `nome_de_escrita`. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:104`)

Nos fragmentos abaixo, cada caso é independente e declarado B; os `.py` ficam fora de `tests/`, `migrations/` e `signals.py`, salvo indicação explícita. Essas condições determinam a seleção feita pelo selo. (`bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:98`)

**a) Continuação adicionada transforma referência antiga em chamada — rc 0.** O `Attribute` continua inteiramente na linha antiga; o `Call` novo não é reconhecido. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,3 @@
 (obj.save
+()
 )
```

**b) `exec` e `eval` com escrita literal no código encapsulado — rc 0 em cada alternativa.** A string `"obj.save()"` não é exatamente `"save"` nem SQL; não existe análise recursiva desse código. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+exec("obj.save()")
```

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+eval("obj.save()")
```

**c) Decorador com código encapsulado — rc 0.** A expressão avaliada produz um decorador que chama `save`; para o selo, continua sendo uma constante não reconhecida. Compartilha o mecanismo de `eval`, não é uma exclusão geral de decoradores. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+@eval("lambda f: (obj.save(), f)[1]")
+def f(): pass
```

**d) F-string formando nome de método — rc 0.** Os fragmentos constantes são `"sa"` e `"ve"`; o selo não calcula o resultado da expressão. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+getattr(obj, f"sa{'ve'}")()
```

**e) `setattr` instala um alias escritor — rc 0.** Nem `"acao"`, `"sa"`, `"ve"` nem o atributo `.acao` pertencem aos conjuntos reconhecidos. (`bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+setattr(obj, "acao", getattr(obj, "sa" + "ve"))
+obj.acao()
```

**f) QuerySet por variável e método calculado — rc 0.** Não há resolução do método calculado nem do tipo de `qs`. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+qs = M.objects.all()
+getattr(qs, "up" + "date")(ativo=False)
```

**g) Cursor por alias e SQL fragmentado — rc 0.** `.execute` não é reconhecido e nenhuma constante isolada satisfaz a regex SQL. (`bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:31`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,3 @@
+cur = connection.cursor()
+run = cur.execute
+run("DE" + "LETE FROM tabela")
```

**h) SQL em f-string — rc 0.** `"UPDATE "` e `" SET ativo=0"` ficam em constantes separadas; nenhuma contém o padrão completo exigido. (`bin/selo_proporcao.py:31`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+cur.execute(f"UPDATE {tabela} SET ativo=0")
```

**i) Python com outro sufixo e carregamento explícito — rc 0.** O payload é descartado pelo sufixo; o carregador não contém nome reconhecido como escrita. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/payload.pyw
@@ -0,0 +1,2 @@
+from app.x.models import M
+M.objects.create()
--- /dev/null
+++ b/app/x/carregar.py
@@ -0,0 +1,2 @@
+import runpy
+runpy.run_path("app/x/payload.pyw")
```

**j) `.sql` com executor adicionado — rc 0.** O SQL fica fora dos `.py`; no executor, a constante é apenas um caminho. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/gravar.sql
@@ -0,0 +1 @@
+DELETE FROM tabela;
--- /dev/null
+++ b/app/x/executar.py
@@ -0,0 +1,3 @@
+from pathlib import Path
+with connection.cursor() as cur:
+    cur.execute(Path("app/x/gravar.sql").read_text())
```

**k) Import antigo de teste, com escritor novo no módulo excluído — rc 0.** Pressupondo que produção já importe `gravar`, alterar somente o corpo abaixo é ignorado. A escrita é adicionada agora; não se trata apenas de chamar helper que já escrevia. (`bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/test_writer.py
+++ b/app/x/test_writer.py
@@ -1,2 +1,2 @@
 def gravar(obj):
-    pass
+    obj.save()
```

**l) `import *` por reexportação — rc 0.** Se `app/x/__init__.py` já reexporta `gravar` de `.tests.fixt`, o import novo não menciona teste e o arquivo escritor é excluído. `import *` sozinho não oculta escrita adicionada em um `.py` comum; aqui a fuga combina reexportação e exclusão. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:63`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/tests/fixt.py
+++ b/app/x/tests/fixt.py
@@ -1,2 +1,2 @@
 def gravar(obj):
-    pass
+    obj.save()
--- /dev/null
+++ b/app/x/usar.py
@@ -0,0 +1,2 @@
+from app.x import *
+gravar(obj)
```

**m) Template chama método escritor — rc 0 do selo.** O `.html` é ignorado; o método existente `rebaixar()` pode chegar a `self.save()`. Renderização efetiva, objeto no contexto e efeito persistido: **[nao verificado]**. Esse caso de helper já existente está no limite declarado do instrumento. (`bin/selo_proporcao.py:19`, `bin/selo_proporcao.py:98`, `app/chamados/models.py:488`, `app/chamados/models.py:498`)

```diff
--- a/app/templates/chamados/fio_colaborador.html
+++ b/app/templates/chamados/fio_colaborador.html
@@ -175,0 +176 @@
+{{ chamado.rebaixar }}
```

Não são escapes, isoladamente: `qs.update(...)`, `f"{obj.save()}"` e referência nova `run = cur.execute` acompanhada de `"DELETE FROM tabela"` dão **rc 1** pelos atributos ou pela constante SQL. Decorador que contenha diretamente atributo novo `.save` também é reconhecido. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:103`, `bin/selo_proporcao.py:117`)

3. **Falsos positivos**

| Exemplo adicionado | Rc B deduzido / motivo |
|---|---|
| Palavra `update` em template `.html` | **0**; o arquivo é ignorado. Não é falso positivo da v2. (`bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`) |
| `rotulo = "update"` ou docstring exatamente `"create"` em `.py` | **1**; constante exata, sem exigir chamada. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:117`) |
| `d.update(...)`, com `d` sendo dict | **1**; não resolve tipo. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:117`) |
| `s.update(...)`, com `s` sendo set | **1**, pelo mesmo atributo. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:117`) |
| `[].delete()` | **1**; aceita sintaxe, sem verificar existência do método. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:101`, `bin/selo_proporcao.py:117`) |
| Import de teste usado somente para leitura | **1**; o import basta. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:117`) |
| Alterar só comentário em migration ou `signals.py` | **1**; a regra por caminho é incondicional para arquivo não deletado, embora o cabeçalho diga “NOVO ou RENOMEADO”. (`bin/selo_proporcao.py:10`, `bin/selo_proporcao.py:92`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:96`) |

**Aceitos na direção conservadora para A**, conforme a orientação fornecida; o código declara expressamente esse custo. Portanto, esses falsos positivos não constituem bloqueante por si. (`bin/selo_proporcao.py:19`)

4. **Retornos**

- **Erros ordinários tratados dão 2:** argumentos insuficientes/classe inválida, revisão inválida, Git não zero, parse inválido e demais `Exception` dentro da fronteira de `main`. (`bin/selo_proporcao.py:34`, `bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:124`)
- **Nem todo caminho de erro dá 2:** o wrapper faz somente `exec`; Python ausente pode devolver 127, execução recusada 126, e sinais não são normalizados. Ocorrência nesse ambiente: **[nao verificado]**. (`bin/selo_proporcao.sh:4`)
- **Argumentos excedentes não são rejeitados:** `base head B repo lixo` ignora `lixo`; com diff inócuo pode devolver 0. (`bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:74`, `bin/selo_proporcao.py:119`)
- **Rc 1 não prova escrita real:** significa `achados && classe B`, incluindo os falsos positivos acima. (`bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:117`)
- **O defeito interno de rc 3 da v1 está fechado:** não há mais pipeline intermediário; `exec` preserva o retorno Python. Um chamador `bash -e` recebe 1 e encerra nessa chamada. O teste atual não confere esse cenário. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:117`, `bin/tests/test_selo_proporcao.sh:62`)
- Encadear externamente com `| cat` ainda pode esconder o rc se o chamador não usar `pipefail`; esse uso hoje é **[nao verificado]**.

5. **Qualidade dos casos**

| Caso/achado v1 | Avaliação v2 |
|---|---|
| `signal_receiver` tinha dois gatilhos | **Corrigido nesse caso:** agora só `@receiver`. Porém `signal_nome` introduz outra duplicidade: módulo terminado em `signals` **ou** nome importado `post_save` bastam independentemente. (`bin/tests/test_selo_proporcao.sh:37`, `bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:64`) |
| Fixture tinha duas exclusões | **Corrigido:** diretório e nome estão separados. (`bin/tests/test_selo_proporcao.sh:51`, `bin/tests/test_selo_proporcao.sh:52`) |
| Faltavam `bulk_update` e `get_or_create` | **Corrigido:** casos separados no laço dos oito métodos. (`bin/tests/test_selo_proporcao.sh:28`) |
| Command não provava tratamento próprio | **Permanece:** `command_alias` passa pela regra genérica de atributo; não prova mecanismo específico de command. (`bin/tests/test_selo_proporcao.sh:43`, `bin/selo_proporcao.py:55`) |
| Preparo podia julgar outro diff | **Melhorou, mas parcialmente:** checkout, resultado global, commit e HEAD são conferidos; scripts internos com `;` ainda podem mascarar falha anterior. O bloco especial de rc também não confere seu preparo. (`bin/tests/test_selo_proporcao.sh:20`, `bin/tests/test_selo_proporcao.sh:21`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:61`) |
| `apagou_migration` | **Nome não corresponde ao preparo:** apenas cria `velho2.py`; não exercita exclusão de migration nem o ramo `D`. (`bin/tests/test_selo_proporcao.sh:54`, `bin/selo_proporcao.py:92`) |
| `bash -e` | **Não prova o anunciado:** descarta o rc dessa chamada; a comparação seguinte usa outro shell, sem `-e`. (`bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`) |
| `prod_importa_teste` | Prova classificação conservadora de import; não cria módulo escritor nem demonstra a costura produção→teste→escrita. (`bin/tests/test_selo_proporcao.sh:47`) |
| `so_remocao` | Remove escrita e adiciona `pass`; não é diff exclusivamente de remoção. (`bin/tests/test_selo_proporcao.sh:49`) |

A afirmação do comentário de que “cada escape” virou caso excede a cobertura: faltam, entre outros, falso `+++`, parênteses envolvendo método e continuação adicionada sobre atributo antigo. (`bin/tests/test_selo_proporcao.sh:4`, `bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:32`, `bin/tests/test_selo_proporcao.sh:55`)

6. **QUEM ESCREVE, QUEM CHAMA e QUEM LÊ**

- **Quem produz o veredito:** Python imprime achados e devolve 0/1/2; o shell apenas o executa. (`bin/selo_proporcao.py:111`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:129`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** `test_selo_proporcao.sh`, via `SELO`; lê e compara rc nos casos e nos argumentos. A chamada com `bash -e` descarta o resultado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:56`, `bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`)
- **Leitor indireto:** a régua executa o teste pelo glob e converte falha do teste em rc 1. Isso verifica um repositório sintético, não classifica o diff real do pouso. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:23`)
- **Pouso real:** o contrato fornecido exige chamada antes do merge, mas não identifiquei essa integração na cópia; chamada manual, leitor operacional do rc e registro no RELATO: **[nao verificado]**. (`contrato.txt:2`, `contrato.txt:3`, anexos) [nao verificado]
- **Pre-push:** chama testes específicos, e o próprio código registra que não executa o loop de host. Integração deste selo no hook efetivamente instalado: **[nao verificado]**. (`bin/pre-push.sh:14`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`)
- **Atalho da régua da v1 permanece:** fingerprint só de `.py`/`.html` de `app/`; o “JA VERDE” retorna antes dos testes de host sem comparar `ARVORE_FP`. (`bin/regua.sh:33`, `bin/regua.sh:45`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)
- **Worktree:** o teste deste selo resolve seu caminho e passa repo sintético explicitamente; execução em worktree: **[nao verificado]**. A quebra independente da pasta inteira permanece: outro teste exige `.git/hooks/pre-push`, enquanto um vizinho já resolve o diretório comum. (`bin/tests/test_selo_proporcao.sh:7`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:27`, `bin/tests/test_commit_so_o_declarado.sh:56`, `bin/tests/test_commit_so_o_declarado.sh:57`)

Este veredito trata do instrumento. O aval manda encaminhar achados de forma para fila, sem reabrir voltas das obras B abrangidas pela autorização. (`aval_proporcao_por_risco_1111.txt:1`, anexo, itens 2 e 5)

**REPROVADO — bloqueantes do instrumento: falsos verdes por continuação e código dinâmico (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:106`); exclusão de testes ainda escapável (`bin/selo_proporcao.py:98`); cobertura anunciada não demonstrada (`bin/tests/test_selo_proporcao.sh:54`, `bin/tests/test_selo_proporcao.sh:62`); normalização universal de erros incompleta (`bin/selo_proporcao.sh:4`); consumidor do pouso real [nao verificado].**

_3902 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
