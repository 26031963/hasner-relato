# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 22:57** · publicado em **30/10/2026 22:58** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 22:57 · sessao abbc739c1d90 · FIO-ESTADO · verificador · origem=Code

**O desenho deixa três lacunas centrais: o esperado novo não está demonstrado, o leitor atual ignora `alcance` e não existe `cor` na sua saída.** (`app/chamados/management/commands/vigiar_fio_morto.py:91`; `bin/fio_estado.py:69–92`.) O valor **388** aparece como requisito de teste no contrato; uma medição atual de produção permanece **[não verificado]**.

1. **Quem escreve e quem lê cada campo**

   | Campo do carimbo | Escritor | Leitor atual / leitura pretendida |
   |---|---|---|
   | `alcance.alcance_universo`, `alcance.alcance_cego` | No anexo, `censo_alcance` produz; `vigiar_fio_morto` grava `alcance=alc`. (`anexo_f5.txt:200–225`; implementação integral **[não verificado]**.) | O leitor atual ignora ambos; usa `censo.cego`, que pertence ao universo antigo. (`bin/fio_estado.py:69–92`.) |
   | `alcance.fio_morto_alcance`, `alcance.alcance_a`…`alcance_e` | Mesmo produtor e escritor do anexo. (`anexo_f5.txt:200–225`; classificação completa **[não verificado]**.) | Hoje não são lidos; o contrato pretende usá-los como manchete e partição **[não verificado: leitor proposto não fornecido]**. (`bin/fio_estado.py:86–92`, `:101–104`.) |
   | `alcance.alcance_porta_fila`, `alcance_porta_pergunta`, `alcance_de_hoje_pra_frente` e os quatro `alcance_<balde>` | Mesmo fluxo do anexo. (`anexo_f5.txt:200–225`; incrementos **[não verificado]**.) | Hoje ignorados; seriam as parcelas da conferência do universo **[não verificado: implementação futura]**. (`bin/fio_estado.py:69–92`.) |
   | `alcance.alcance_competencias`, `alcance_e_detalhe`, `alcance_dias` | O anexo os inclui no dicionário carimbado. (`anexo_f5.txt:200–225`; preenchimento integral **[não verificado]**.) | Hoje ignorados; o contrato não especifica quais desses campos serão publicados **[não verificado]**. (`bin/fio_estado.py:82–92`.) |
   | `censo.fio_morto_alcance` | A F5 copia `alc['fio_morto_alcance']` para o censo carimbado. (`anexo_f5.txt:200–225`.) | Hoje só participaria de `fora_do_esperado` **se também existir a chave em `esperado`**; não é exportado como contador próprio. (`bin/fio_estado.py:70–73`, `:82–92`.) |
   | `esperado.fio_morto_alcance` | **Não há escritor demonstrado para essa chave:** a declaração atual não a contém, e o trecho F5 apenas copia `ESPERADO_DO_CARIMBO`. (`app/chamados/management/commands/vigiar_fio_morto.py:86–91`; `anexo_f5.txt:200–225`.) | A comparação genérica atual a leria automaticamente, caso existisse; a manchete continuaria antiga. (`bin/fio_estado.py:70–73`, `:101–108`.) |

   Portanto, o texto “esperado 0” de `linhas_alcance` não demonstra `esperado.fio_morto_alcance` no JSON **[não verificado: declaração F5 não fornecida]**. O contrato exige esse dado no carimbo; usar `0` como substituto no leitor contrariaria o desenho atual de expectativa transportada pelo escritor. (`bin/fio_estado.py:14–22`, `:70–73`.)

2. **Estados do carimbo e sua apresentação**

   A coluna “contrato” abaixo descreve a especificação fornecida; sua implementação futura está **[não verificado]**. Hoje o JSON não contém `cor`: “amarelo/verde” aparecem como convenções nos comentários e no selo. (`bin/fio_estado.py:67–92`; `bin/tests/test_fio_estado.sh:14`, `:56–60`.)

   | Estado | Código atual | Contrato |
   |---|---|---|
   | Ausente, sem permissão ou JSON sintaticamente inválido | `medido=False`, texto **NUNCA MEDIDO**. (`bin/fio_estado.py:63–68`, `:97–100`.) | AMARELO. |
   | Antigo: sem `alcance` | Publica o contador antigo normalmente; não reconhece “F5 não pousou”. Pode satisfazer o caso chamado verde pelo selo. (`bin/fio_estado.py:101–104`; `bin/tests/test_fio_estado.sh:40–60`.) | Aviso “carimbo sem alcance”; nunca VERDE. |
   | Novo, íntegro, com universo positivo | Continua publicando `censo.fio_morto`; ignora a partição. Só acusa o contador novo se ele estiver em `esperado`. (`bin/fio_estado.py:70–73`, `:101–108`.) | Manchete de alcance contra esperado lido; VERDE depende das validações. |
   | Novo, cego ou universo zero/ausente | Ignora a cegueira de alcance; só escreve **CEGO** quando `censo.cego` é verdadeiro. (`bin/fio_estado.py:91`, `:109–111`.) | VERMELHO. |
   | Partição quebrada ou espelho `censo` divergente | Não há essas conferências. (`bin/fio_estado.py:69–92`.) | VERMELHO. |
   | Rodada com `--empresa` | Não altera o carimbo: o painel continua lendo a última fotografia de frota, ou fica nunca medido se ela não existir. (`app/chamados/management/commands/vigiar_fio_morto.py:194–212`; `bin/fio_estado.py:62–68`.) | Não constitui um formato novo de carimbo. |
   | Temporalmente velho | Acima de 26 horas, marca `velho=True` e avisa no texto. Data ausente/inválida deixa idade desconhecida e `velho=False`. (`bin/fio_estado.py:56`, `:74–84`, `:112–117`.) | A precedência entre idade, erro estrutural e cor não foi especificada **[não verificado]**. |

3. **“Nenhum caminho levanta exceção para o ESTADO” não vale hoje**

   A proteção cobre abertura e decodificação, mas não valida os tipos depois disso: JSON `[1]` falha no `.get`; `censo=[1]` falha na comparação; `medido_em=7` causa `TypeError`, enquanto o tratamento captura apenas `ValueError`. (`bin/fio_estado.py:63–80`.)

   Essas exceções chegam à composição do ESTADO porque importação, leitura e formatação do fio são chamadas sem proteção nessa seção. (`bin/gerar_estado.py:260–261`.) O contrato precisa cobrir também estrutura, tipos e composição da linha; tratar apenas JSON ilegível não basta. (`bin/fio_estado.py:69–120`.)

4. **Há consumidores que mudam de comportamento**

   `gerar_estado` consome o dicionário Python por `ler`/`linha`, não o JSON serializado nem uma chave `cor`; a nova manchete chegará ao painel pela alteração de `linha`. Apenas acrescentar `cor` não a publica. (`bin/gerar_estado.py:260–261`, `:313–315`, `:350`.)

   O consumidor explícito do JSON encontrado é o selo, que lê `medido`, `velho` e as chaves de `fora_do_esperado`. Acrescentar campos preserva essa interface; alterar esses campos muda o teste. Consumidores externos à cópia: **[não verificado]**. (`bin/tests/test_fio_estado.sh:51`.)

   Reaproveitar a chave pública `fio_morto` para guardar alcance mudaria sua semântica: hoje ela significa exatamente `censo.fio_morto`. Preservá-la e acrescentar `fio_morto_alcance` mantém a distinção entre os universos. (`bin/fio_estado.py:86`, `:101–104`.)

5. **Há uma ambiguidade entre manchete nova e regressão antiga**

   Mesmo com alcance correto, o leitor atual continua comparando **todas** as expectativas antigas. Assim, alcance conforme e contador antigo divergente ainda produzem `fora_do_esperado`. (`bin/fio_estado.py:70–73`; `app/chamados/management/commands/vigiar_fio_morto.py:86–91`.)

   O contrato manda rotular o antigo como regressão, mas não determina se essa regressão participa da nova `cor` **[não verificado]**. Essa decisão precisa ficar explícita; trocar apenas a manchete preserva um veredito com escopo antigo. (`bin/fio_estado.py:101–108`.)

6. **Conferir a soma pode ser propriedade fixa, sem virar segundo censo**

   Conferir, entre campos já carimbados, `a+b+c+d+e == fio_morto_alcance`, as partes contra o universo e a igualdade dos dois exemplares do contador é validação de integridade. A L-110 nomeia expressamente “partes somam o total” como propriedade fixa. (`CLAUDE.md:45–51`.)

   A fronteira da LEI-AKITA 2 é reconstruir universo ou classificação: percorrer `alcance_dias`, reaplicar datas, decidir causas ou corrigir os contadores seria derivação própria. (`CLAUDE.md:20–22`; `bin/fio_estado.py:29–34`.) Uma soma que fecha também não prova que os dias foram classificados corretamente; o trecho integral dessa classificação não foi fornecido **[não verificado]**.

7. **Onde pode aparecer “VERDE sem medida”** [nao verificado]

   Hoje, estes casos satisfazem o critério `medido=True`, `velho=False`, `fora_do_esperado={}` que o selo chama verde — embora o leitor não emita uma cor propriamente dita. (`bin/tests/test_fio_estado.sh:51–60`.)

   - `{}` ou `null`: tornam-se registro vazio, com `medido=True`, expectativa vazia e idade desconhecida. (`bin/fio_estado.py:65`, `:69–84`.)
   - `esperado` ausente/vazio: nenhuma comparação ocorre, mesmo havendo contador não zero. (`bin/fio_estado.py:70–73`.)
   - Carimbo antigo com contadores conformes: nenhuma medida de alcance é exigida. (`bin/fio_estado.py:69–92`.)
   - Alcance cego ou universo vazio com censo antigo conforme: a cegueira nova é ignorada. (`bin/fio_estado.py:86–92`, `:109–111`.)
   - `censo.cego=True`: aparece o aviso textual, mas não entra automaticamente em `fora_do_esperado`; o escritor atual não inclui `cego` em `esperado`. (`app/chamados/management/commands/vigiar_fio_morto.py:80–91`; `bin/fio_estado.py:72–73`, `:109–111`.)
   - Medição sem data válida: `velho=False` não significa que a fotografia seja recente. (`bin/fio_estado.py:74–84`.)

   Há ainda um desencontro possível de destino: o escritor aceita `VIGIA_FIO_CARIMBO`, enquanto o leitor usa caminho fixo; o selo compara somente as constantes. Se houver redirecionamento, o painel pode continuar lendo outra fotografia. Configuração de produção: **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:97–107`; `bin/fio_estado.py:55`, `:62`; `bin/tests/test_fio_estado.sh:93–102`.)

8. **O selo atual e a promessa de pre-push não fecham a garantia nova** [nao verificado]

   O selo fabrica somente formato antigo; não consulta `cor`; seu caso cego exige apenas a palavra **CEGO**. Ele pode continuar passando sem provar alcance, vermelho por cegueira ou aviso de formato antigo. (`bin/tests/test_fio_estado.sh:40–51`, `:104–107`.) As provas por mutação dos casos novos estão **[não verificado]**; a regra exige que o selo morda a costura. (`CLAUDE.md:350–355`.)

   Colocar o novo selo em `bin/tests/test_*.sh` o inclui na régua, mas **não garante execução direta no pre-push**: o próprio script declara que esse loop não roda ali. (`bin/regua.sh:138–145`; `bin/pre-push.sh:12–16`.)

9. **O primeiro carimbo novo não basta para apagar o ramo antigo**

   Uma rodada de recorte não substitui o arquivo; o escritor atual ainda produz o formato antigo. Portanto, observar um primeiro carimbo F5 não demonstra que nenhum antigo possa voltar a existir. (`app/chamados/management/commands/vigiar_fio_morto.py:194–212`.) Essa impossibilidade operacional permanece **[não verificado]**; a L-111 exige retirar o sítio quando ele perde o último uso, junto com testes e pendência. (`CLAUDE.md:341–344`.)

## 30/10 22:55 · sessao bfc8fe859b37 · - · - · origem=Ronald (tmux)

- **Soma como propriedade fixa:** `CLAUDE.md:45-51` define expressamente “partes somam o total” como propriedade fixa da L-110. Conferir igualdades entre contadores já carimbados valida integridade; reconstruir universo, classificar dias ou corrigir números seria derivação própria proibida por `CLAUDE.md:20-22`. Há precedente no selo do produtor: `app/chamados/tests/test_o261_vigia_fio.py:647-668`.

- **Selo atual pode continuar verde sem validar alcance:** gera exclusivamente carimbos antigos (`bin/tests/test_fio_estado.sh:40-48`), observa somente `medido`, `velho` e `fora_do_esperado` (`bin/tests/test_fio_estado.sh:51`) e considera casos antigos “VERDE” (`bin/tests/test_fio_estado.sh:56-91`). Acrescentar `cor=AMARELO` sem alterar essas três saídas não faria esses casos falharem (`bin/tests/test_fio_estado.sh:51`).

- **Cegueira atual não prova vermelho:** o caso apenas procura `CEGO` no texto, sem conferir campo `cor` ou veredito (`bin/tests/test_fio_estado.sh:104-107`). O selo exige caso que morda a costura; texto presente sozinho não demonstra a cor exigida (`CLAUDE.md:350-355`).

- **Mutações prometidas:** o arquivo atual encerra declarando sete casos, mas não executa mutações de leitor (`bin/tests/test_fio_estado.sh:109`). A comprovação por mutação dos novos casos permanece **[não verificado]**.

- **L-111:** apagar o ramo antigo exige demonstrar que perdeu seu último uso; primeiro carimbo novo isoladamente não demonstra impossibilidade de carimbo antigo retornar **[não verificado]**. A lei manda retirar sítio sem chamador, testes e pendência no mesmo commit (`CLAUDE.md:341-344`). [nao verificado]

- **Entrada dos selos:** a régua enumera `bin/tests/test_*.sh` (`bin/regua.sh:138`); o pre-push documenta que esse diretório não roda diretamente ali (`bin/pre-push.sh:15`).

## 30/10 22:55 · sessao 70a74bc5d59a · - · - · origem=Ronald (tmux)

- O consumidor de produção encontrado é `gerar_estado`: importa `ler`/`linha`, passa o dicionário inteiro a `linha` e publica o texto resultante. Não consulta `cor`, `alcance` ou contadores diretamente; a mudança de manchete chega ao ESTADO pela alteração de `linha`. (`bin/gerar_estado.py:260-261`, `bin/gerar_estado.py:313-315`, `bin/gerar_estado.py:350`)

- O consumidor do JSON encontrado é o selo atual: consulta somente `medido`, `velho` e as chaves de `fora_do_esperado`; também importa `CARIMBO`. Acrescentar campos preservando esses campos mantém essa interface; renomeá-los/removê-los rompe o selo. (`bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:97`)

- O JSON público atual contém `fio_morto=censo.fio_morto`; a linha também lê esse campo. Reutilizar `fio_morto` para conter alcance mudaria sua semântica; acrescentar `fio_morto_alcance` e preservar o antigo evita essa troca. Consumidores externos à cópia: [não verificado]. (`bin/fio_estado.py:86`, `bin/fio_estado.py:101-104`, `bin/fio_estado.py:129`)

- F5 só escrever `esperado.fio_morto_alcance` e `censo.fio_morto_alcance` já faria o leitor atual acrescentar esse desvio a `fora_do_esperado`, porque ele itera todas as expectativas. Isso não muda a manchete, que continua no contador antigo. (`bin/fio_estado.py:69-73`, `bin/fio_estado.py:101-108`)

- O selo antigo só cria carimbos sem `alcance`. Seus casos chamados VERDE verificam `True|False|-`; não verificam `cor`. Portanto acrescentar `cor=AMARELO` para formato antigo pode preservar esses testes, mas o selo continuará chamando esses casos de VERDE sem testar o novo significado. (`bin/tests/test_fio_estado.sh:40-48`, `bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:56-60`, `bin/tests/test_fio_estado.sh:82-86`)

- A régua executa automaticamente `bin/tests/test_*.sh`; o pre-push não executa esse loop e declara isso expressamente. Colocar o novo selo em `bin/tests/` garante sua descoberta pela régua, mas não sua execução direta a cada pre-push. O hook instalado real é [não verificado]. (`bin/regua.sh:138-145`, `bin/pre-push.sh:12-19`, `bin/pre-push.sh:61-66`)

- O instalador vigente usa `bin/pre-push.sh` como fonte e gera wrapper que executa esse caminho; a cópia histórica de `app/core/hooks/pre-push.sh` não é o alvo declarado por esse instalador. (`bin/hooks.sh:9-11`, `bin/hooks.sh:51-56`)

## 30/10 22:54 · sessao 9ae75d7ba88f · O261-V-FASE · verificador · origem=Code

**Sem achado procedente no delta examinado:** C9 e C10 passam pelo mesmo mapa dono→porta; BATIDA abre regularização, ESTRUTURA fica sem URL e somente CADASTRO recebe a porta específica da assinatura (`app/escala/services/cadastro_realidade.py:481`, `app/escala/services/cadastro_realidade.py:490`, `app/escala/services/cadastro_realidade.py:518`).

1. **Quem escreve e quem lê**

- **C9:** `lavrar_teto_do_dia` chama medição e persistência; a medição pergunta a `InsumosDoDono.dono()` e grava `dono` no detalhe do snapshot (`app/ponto/management/commands/lavrar_teto_do_dia.py:28`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`). `_teto_do_dia()` lê a lavra e copia esse valor para `dono_do_dia` (`app/escala/services/cadastro_realidade.py:320`, `app/escala/services/cadastro_realidade.py:325`).
- **C10:** o censo monta `InsumosDoDono` com os dias de fase daquela corrida, pergunta o dono de cada dia e grava `dono_do_dia`; `fase_12x36.lavrar()` persiste o detalhe (`app/ponto/management/commands/censo_fase_12x36.py:260`, `app/ponto/management/commands/censo_fase_12x36.py:262`, `app/ponto/management/commands/censo_fase_12x36.py:272`, `app/escala/services/fase_12x36.py:90`). `_fase_deslocada()` lê a lavra até `hoje` e preserva o dono (`app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:346`).
- **Juiz comum:** `InsumosDoDono.dono()` chama `dono_da_divergencia()`; a lista recebe o resultado lavrado de cada assinatura e calcula `acao`/`url` (`app/ponto/management/commands/e6_oraculo.py:166`, `app/ponto/management/commands/e6_oraculo.py:173`, `app/escala/services/cadastro_realidade.py:657`, `app/escala/services/cadastro_realidade.py:661`, `app/escala/services/cadastro_realidade.py:664`).
- **Tela/PDF:** a view fornece a mesma lista aos dois destinos. O template lê dono, ação e URL; o PDF imprime dono e ação, sem hyperlink da porta (`app/escala/views.py:490`, `app/escala/views.py:492`, `app/escala/views.py:495`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:794`, `app/escala/services/cadastro_realidade.py:797`).
- **Vigia:** lê `dono_do_dia` por `dias_na_lista()`, sem consultar o texto do botão. C9/C10 conflitantes resultam em `None`; somente BATIDA/CADASTRO entram em `fio_do_admin`, mantendo ESTRUTURA no alarme (`app/escala/services/cadastro_realidade.py:728`, `app/escala/services/cadastro_realidade.py:731`, `app/escala/services/cadastro_realidade.py:753`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`, `app/chamados/services/vigia_fio.py:314`).

2. **Mapa, rótulos e leitores antigos**

Não restaram dois mapas nesses caminhos. As diferenças entre C9 e C10 são os destinos de **CADASTRO**: template/vínculo na C9; fio ou “só listado”, conforme `aplicavel`, na C10. BATIDA passa à regularização inclusive antes do piso; ESTRUTURA e dono ausente não recebem porta de cadastro (`app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:484`, `app/escala/services/cadastro_realidade.py:519`).

O novo rótulo “o do juiz do dia, e6_oraculo…” alimenta `por_assinatura` e aparece no **title do selo superior**. O PDF imprime o dono efetivo do dia — BATIDA/CADASTRO/ESTRUTURA — e não essa frase agregada (`app/escala/services/cadastro_realidade.py:118`, `app/escala/services/cadastro_realidade.py:699`, `app/templates/escala/cadastro_x_realidade.html:19`, `app/escala/services/cadastro_realidade.py:796`).

Não localizei leitor executável dependente dos textos antigos. Os testes C9 encontrados prendem dono e URLs; “Regularizar a saida do dia” permanece no relato histórico (`app/ponto/tests/test_col129_teto_vira_lista.py:215`, `app/ponto/tests/test_col129_teto_vira_lista.py:226`, `app/docs/RELATO.md:151`). A prosa alterada explicita a morte de `fora_de_fase` e a leitura da lavra (`app/chamados/services/proposta_no_fio.py:22`, `app/core/tests/test_selo_performance.py:141`).

3. **Os testes mordem?** [nao verificado]

**Sim, para os dois defeitos visados:** um cria batida extra real, percorre censo→lavra→lista e exige dono BATIDA com URL de regularização; o outro exige identificação do juiz no rótulo agregado (`app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:206`).

A cobertura nova é parcial: não prende ESTRUTURA, dono ausente, BATIDA antes do piso nem renderização do title/PDF; o teste do rótulo aceita uma substring (`app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:209`).

Exercitei em memória 20 combinações de C9/C10, piso e dono, coerentes com o mapa; a comparação isolada reproduziu base→fio e cópia→regularização para C10/BATIDA (`app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:511`). **Execução dos testes Django e renderização real: [não verificado]**; faltam as dependências neste ambiente.

## 30/10 22:53 · sessao d7a63fdce16d · - · - · origem=Ronald (tmux)

Nenhum defeito concreto encontrado neste recorte.

- Os dois testes novos exercitam comportamento: Batida extra real → censo/lavra → lista → dono BATIDA e URL de regularização; outro verifica a identificação do juiz no rótulo agregado. `app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:206`.
- Confirmei por execução de AST em memória, usando `python3 -B`, 32 combinações entre base/alvo, C9/C10, quatro donos e piso aplicável/inaplicável: base encaminhava C10 aplicável BATIDA/ESTRUTURA/sem dono ao fio; alvo encaminha BATIDA à regularização, ESTRUTURA/sem dono a nenhuma URL, preservando CADASTRO conforme piso. Código exercitado: `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:511`. Não executei a integração Django: `[não verificado]`.
- Lacunas dos dois testes adicionados: não verificam ESTRUTURA, ausência de dono, BATIDA anterior ao piso, renderização do `title` nem extração do PDF; a asserção do rótulo é somente substring. `app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:209`.
- O texto novo aparece no `title` do selo agregado, lido de `por_assinatura`; a linha da tela usa dono efetivo. `app/templates/escala/cadastro_x_realidade.html:19`, `app/templates/escala/cadastro_x_realidade.html:28`.
- O PDF não imprime o rótulo novo agregado: imprime `a['dono']` por dia e `a['acao']`, após a sobreposição do dono lavrado. `app/escala/services/cadastro_realidade.py:660`, `app/escala/services/cadastro_realidade.py:666`, `app/escala/services/cadastro_realidade.py:794`.
- Não achei leitor/teste do texto antigo exato; única ocorrência restante de “Regularizar a saida do dia” está no relato histórico. `app/docs/RELATO.md:151`. Os testes C9 existentes dependem das URLs e donos, preservados: `app/ponto/tests/test_col129_teto_vira_lista.py:215`, `app/ponto/tests/test_col129_teto_vira_lista.py:226`.
- A prosa alterada declara a morte de `fora_de_fase` e a leitura da lavra; o parágrafo anterior permanece como explicação histórica de custo. `app/chamados/services/proposta_no_fio.py:15`, `app/chamados/services/proposta_no_fio.py:22`, `app/core/tests/test_selo_performance.py:141`.

## 30/10 22:53 · sessao ba5d8129f418 · - · - · origem=Ronald (tmux)

Não encontrei defeito procedente na cadeia dono→porta examinada; C9 e C10 chamam o mesmo mapa, que só entrega a porta de cadastro para `CADASTRO` (`app/escala/services/cadastro_realidade.py:481`, `:490`, `:518`).

- **C9 escreve:** `medir()` monta `InsumosDoDono` e grava seu resultado em `dono` (`app/ponto/services/teto_na_lista.py:72`, `:86`); `lavrar()` persiste o detalhe em `MetricaSnapshot` (`:110`, `:113`). O comando chama medição e grava somente com `--apply` (`app/ponto/management/commands/lavrar_teto_do_dia.py:28`, `:31`, `:34`).
- **C10 escreve:** o censo identifica os dias e respectivos vínculos/templates (`app/ponto/management/commands/censo_fase_12x36.py:210`), pergunta pelo dono ao mesmo `InsumosDoDono`, passando os dias de fase desta corrida (`:255`, `:260`, `:262`), e inclui `dono_do_dia` nos itens (`:272`). `fase_12x36.lavrar()` persiste esse detalhe (`app/escala/services/fase_12x36.py:84`, `:90`).
- **Juiz comum:** `InsumosDoDono.dono()` passa as informações de batida, fase e cadastro para `dono_da_divergencia()` (`app/ponto/management/commands/e6_oraculo.py:166`, `:169`, `:173`, `:176`). A precedência implementada é cadastro inválido, batida fora do turno/contagem ímpar, fase, demais casos (`:97`).
- **Leitura e composição:** C9 lê o último snapshot (`app/ponto/services/teto_na_lista.py:120`) e copia `dono` para `dono_do_dia` (`app/escala/services/cadastro_realidade.py:318`, `:325`); C10 lê a lavra até `hoje` (`app/escala/services/fase_12x36.py:96`, `:103`; `app/escala/services/cadastro_realidade.py:343`, `:346`). A lista passa o dono de cada dia à ação e sobrepõe o dono genérico da assinatura (`app/escala/services/cadastro_realidade.py:653`, `:657`, `:660`, `:661`, `:664`).
- **Porta comum:** BATIDA→`Regularizar a batida do dia` + regularização do colaborador; CADASTRO→porta específica recebida; ESTRUTURA→sistema sem URL; ausente→aguardar lavra sem URL (`app/escala/services/cadastro_realidade.py:518`). A porta CADASTRO da C9 é o template do dia ou vínculo (`:481`); da C10 é fio se aplicável, senão só listado (`:490`).
- **Tela e PDF:** a view entrega a mesma lista para HTML ou PDF (`app/escala/views.py:490`, `:491`, `:495`). O HTML da linha lê dono efetivo, ação e URL (`app/templates/escala/cadastro_x_realidade.html:28`); PDF lê dono efetivo e ação, sem URL (`app/escala/services/cadastro_realidade.py:794`, `:796`, `:797`).
- **Novo rótulo C10:** a frase “o do juiz do dia…” está na definição (`app/escala/services/cadastro_realidade.py:118`), alimenta `por_assinatura` (`:699`, `:700`) e aparece no `title` do selo superior (`app/templates/escala/cadastro_x_realidade.html:19`). Não aparece como frase no PDF; o PDF usa o dono efetivo da linha (`app/escala/services/cadastro_realidade.py:796`).
- **Vigia:** `dias_na_lista()` lê `dono_do_dia` da própria lista e resolve conflitos entre C9/C10 sem depender da ordem (`app/escala/services/cadastro_realidade.py:728`, `:731`, `:748`, `:753`). O vigia chama lista e leitor uma vez e aceita exclusivamente BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:255`, `:259`, `:266`); ESTRUTURA permanece em `fio_morto` (`:314`, `:324`).
- **Texto antigo:** a busca encontrou “Regularizar a saida do dia” apenas em relato histórico, sem dependência de código identificada nessa busca (`app/docs/RELATO.md:151`). O novo texto vem do helper único (`app/escala/services/cadastro_realidade.py:520`).

Executei, sem importar Django e sem escrita, as funções extraídas por AST numa matriz de 16 casos: C9/C10 × aplicável/não aplicável × BATIDA/CADASTRO/ESTRUTURA/ausente. Os resultados confirmaram o mapa acima (`app/escala/services/cadastro_realidade.py:478`, `:511`); integração com banco/renderização real não executada [não verificado].

## 30/10 22:40 · sessao bc2cecd364ad · O261-E-CELULA-20 · verificador · origem=Code

**O delta não garante “papel lido da porta” nem “um dono”: há atribuições locais de `admin`, dois contraexemplos de responsável e possibilidade de `admin+colaborador`.** (`app/chamados/services/vigia_fio.py:481`, `:574`, `:579`)

**1. Papel por rota e quem pode agir**

| Rota | Verificação |
|---|---|
| `porta_fila` | Grava `admin` diretamente. `qs_na_fila` informa presença na fila, sem papel; o leitor existente distingue `colab/cobrar` de `admin_validar/validar`. A tela administrativa permite agir conforme departamento ou gestor geral. (`app/chamados/services/vigia_fio.py:574`; `app/chamados/services/painel.py:186`; `app/chamados/catalogo/natureza.py:41`; `app/chamados/utils.py:52`; `app/chamados/views.py:418`) |
| `porta_pergunta` | Deriva colaborador/admin de `respondida_em`. Há testemunha equivalente na API: `p.estado` e `aguarda_admin`. Pendente: colaborador responde; respondida: autoridade administrativa competente valida, e o colaborador ainda pode corrigir. (`app/chamados/services/vigia_fio.py:579`; `app/api/views.py:2621`; `app/chamados/views.py:1239`, `:1280`, `:483`; `app/templates/chamados/partials/_card_atendimento.html:101`) |
| `de_hoje_pra_frente` | Grava `nenhuma porta` depois de falharem as duas portas anteriores, **antes de consultar os quatro baldes**. Portanto não demonstra ausência de toda ação possível naquele dia; quem pode agir além dessas duas consultas: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:582`, `:588`) |
| `decisao_do_dia_pendente` | O leitor comprova oferta de “Resolver dia”, mas exclui expressamente a permissão do usuário. Tela e POST exigem `regularizar_batida`, concedida à supervisão na semeadura padrão. `admin` vem do mapa local. (`app/colaboradores/services/calendario.py:203`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/ponto/views.py:3305`; `app/colaboradores/mixins.py:355`; `app/chamados/services/vigia_fio.py:481`) |
| `aguarda_decisao_de_ausencia` | O leitor aceita os dois estados de `ABERTAS`: documento devido pelo colaborador e decisão devida pelo DP. Colaborador anexa pela API; usuários com `editar_ausencias` anexam/decidem pelo painel. O mapa escreve `admin` para ambos. (`app/chamados/services/vigia_fio.py:258`; `app/ponto/services/ausencia.py:719`, `:749`; `app/api/views.py:3125`; `app/ponto/views.py:2139`, `:2589`; `app/chamados/services/vigia_fio.py:481`) |
| `escala_nao_descreve_o_dia` | Basta pergunta validada com aceite de folga; esse ramo não consulta lista nem porta de correção. Existem wizard com `editar_escalas` e edição de vínculo com `editar_cadastro`; qual porta está oferecida para cada dia: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:261`; `app/escala/views_wizard.py:63`; `app/colaboradores/views.py:16`) |
| `fio_do_admin` | Lê presença do dia e dono BATIDA/CADASTRO, não perfil administrativo. A lista aponta regularização, template ou vínculo, sujeitos às permissões correspondentes. A regularização é explicitamente descrita como porta da supervisão. (`app/chamados/services/vigia_fio.py:204`; `app/escala/services/cadastro_realidade.py:445`, `:20`; `app/colaboradores/views.py:2948`; `app/escala/views_wizard.py:63`; `app/colaboradores/views.py:16`) |
| `sem_porta` | `nenhuma porta` significa que falharam as portas e leis consultadas pelo censo. Não identifica um responsável por reparar o caso; isso permanece `[nao verificado]`. (`app/chamados/services/vigia_fio.py:572`, `:588`, `:632`) |

Dois contraexemplos demonstráveis pelo código:

- **Fila com pergunta pendente:** chamado aberto ligado à célula + pergunta pendente recebe `porta_fila/admin`, porque esse ramo vence antes de olhar perguntas. O leitor existente daria `colab/cobrar`. (`app/chamados/services/vigia_fio.py:572`; `app/chamados/catalogo/natureza.py:48`, `:58`)
- **Ausência aguardando documento:** recebe `admin`, embora a próxima obrigação seja do colaborador; aprovação nesse estado é recusada pela máquina. (`app/chamados/services/vigia_fio.py:258`, `:481`; `app/ponto/services/ausencia.py:719`, `:751`, `:795`)

A exclusão categórica de supervisor também não se sustenta como descrição de quem pode agir: há portas da supervisão e permissões concedidas a ela. Os usuários efetivamente autorizados em produção são `[nao verificado]`. (`app/escala/services/cadastro_realidade.py:20`; `app/colaboradores/mixins.py:355`, `:322`)

**2. Dias e contadores**

Na execução normal, com universo dentro do `limite`, **nenhum dia some nem duplica**: o universo usa chave `(colab_id, data)`; cada ramo acrescenta uma linha e encerra aquela iteração, ou chega ao `sem_porta`. Dias sem célula também entram, com `celula_id=None`. (`app/chamados/services/vigia_fio.py:518`, `:524`, `:564`, `:569`, `:572`, `:576`, `:582`, `:590`, `:600`, `:632`)

Pelo delta fornecido, **os contadores preservam a classificação anterior**: `any(...)` virou lista com o mesmo predicado, e o limite continua aplicado somente à inclusão na lista. Mudou o contrato da saída: `alcance_dias_sem_porta` virou `alcance_dias`, abrangendo todas as rotas. (`app/chamados/services/vigia_fio.py:513`, `:565`, `:576`, `:628`)

Uma linha pode conter **`admin+colaborador`** quando há perguntas elegíveis pendentes e respondidas no mesmo dia. Isso não duplica o dia, mas tampouco entrega “um dono”. Ocorrência em produção: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:579`)

**3. Testes que aceitam papel errado** [nao verificado]

**Sim.** O teste novo `test_supervisor_nunca_e_escrito...` aceita, por exemplo, trocar o papel de `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia` ou `fio_do_admin` para `colaborador`: ele só proíbe `supervisor` e compara as chaves do mapa. Os outros dois testes novos não verificam essas três rotas. (`app/chamados/tests/test_o261e_f5_alcance.py:281`, `:291`, `:299`)

O teste da fila exige `admin` num chamado **sem pergunta**; não cobre a fila com pergunta pendente, onde diverge do leitor de dono. O teste de decisão pendente usa um booleano mockado, sem conferir permissão ou ação pela tela. (`app/chamados/tests/test_o261e_f5_alcance.py:293`, `:296`; `app/chamados/catalogo/natureza.py:50`)

Resultado da suíte Django e quantidades/ocorrências reais em produção: `[nao verificado]`. [nao verificado]

## 30/10 22:39 · sessao 44e003a85fc7 · O261-V-FASE · verificador · origem=Code

**Não encontrei novo procedente nos caminhos examinados da cópia.** A conferência integral do diff `aa96b93c..9c9e030e` fica **[nao verificado]**: esta cópia não oferece o histórico Git necessário.

1. **Escritores, leitores e corte literal**

   - **Fase:** o censo produz os dias de desvio e injeta os dias desta corrida em `InsumosDoDono`, evitando consultar a lavra anterior enquanto escreve (`app/ponto/management/commands/censo_fase_12x36.py:210`, `:260`). Nos demais usos, `InsumosDoDono.fase()` lê `ultimo_lavrado(self.hoje)`, filtra pelo colaborador e transforma a presença do dia no booleano `fase_deslocada` (`app/ponto/management/commands/e6_oraculo.py:159`, `:169`).
   - **Dono lavrado:** o censo consulta `_ins.dono()` para cada dia, grava o resultado em `dono_do_dia` e persiste pelo serviço `lavrar()` (`app/ponto/management/commands/censo_fase_12x36.py:262`, `:273`; `app/escala/services/fase_12x36.py:90`).
   - **Leitores operacionais do juiz:** e6, pautas discordantes e teto usam `InsumosDoDono.dono()`; o pré-filtro das pautas também passa explicitamente a fase (`app/ponto/management/commands/e6_oraculo.py:309`, `:359`; `app/ponto/management/commands/pautas_discordantes.py:96`, `:109`; `app/ponto/services/teto_na_lista.py:72`, `:86`).
   - **Leitores do dono da lavra:** C10 preserva `dono_do_dia`; `lista()` o exibe; `dias_na_lista()` consolida por colaborador/dia; o vigia aceita somente BATIDA/CADASTRO (`app/escala/services/cadastro_realidade.py:343`, `:650`, `:742`; `app/chamados/services/vigia_fio.py:259`, `:266`).
   - **Demais consumidores da fase:** API, fila, propositor, pauta semanal e vigia da família leem o snapshot; não arbitram o dono do dia (`app/api/views_mensageria.py:1672`; `app/chamados/services/fila_por_causa.py:174`; `app/escala/services/propositor.py:492`; `app/escala/services/fase_12x36.py:124`; `app/ponto/management/commands/vigiar_familia_escala.py:24`).

   **Não identifiquei leitor e6 inconsistente com o corte:** BATIDA por turno aberto/batida real ou contagem ímpar precede CADASTRO pela fase. Permanecem os ramos anteriores de cadastro já ressalvados na triagem (`app/ponto/management/commands/e6_oraculo.py:97`, `:103`, `:105`, `:107`).

2. **Sim, o teste novo morde `lista()`.** [nao verificado]

   Ele chama `lista(hoje=30/10)`, substitui apenas `ultimo_lavrado` e exige que esse leitor receba `30/10`; o relógio está congelado em `05/11` (`app/escala/tests/test_o261_v_fase_na_lista.py:177`, `:208`). A cadeia real repassa `hoje` nos dois elos; retirar qualquer repasse deixa de satisfazer a asserção (`app/escala/services/cadastro_realidade.py:611`, `:340`; `app/escala/tests/test_o261_v_fase_na_lista.py:214`). Execução da suíte Django: **[nao verificado]**.

3. **Nenhum novo procedente identificado nesse recorte.**

   Conferi também: pré-filtro das pautas consulta fase antes de pedir motor; C10 recebe o `hoje` da lista; C9/C10 têm igual autoridade e divergência resulta em `None`, preservando o alarme independentemente da ordem (`app/ponto/management/commands/pautas_discordantes.py:96`; `app/escala/services/cadastro_realidade.py:611`, `:710`, `:721`; `app/chamados/services/vigia_fio.py:266`). Efeitos em produção e resultados históricos além do anexo: **[nao verificado]**.

## 30/10 22:39 · sessao 4f714b67b5f3 · - · - · origem=Ronald (tmux)

Achado principal: `porta_fila → admin` não é leitura do responsável. `qs_na_fila()` filtra `VIVOS/COBRANCA` e silêncio, sem retornar papel (`app/chamados/services/painel.py:186-188`); o censo escreve `admin` diretamente e interrompe antes de examinar perguntas (`app/chamados/services/vigia_fio.py:572-581`).

Contraexemplo verificável por código: chamado `batida_ausente`, aberto, ligado à célula, com pergunta pendente ancorada nele. O censo informa `porta_fila/admin`; o leitor de dono existente informa `colab`, convertido em `cobrar` (`app/chamados/catalogo/natureza.py:41-60`). A própria tela considera `cobrar` trabalho do colaborador e só `validar/decidir` exigem admin (`app/chamados/services/verbos.py:17-24`). Helpers permitem construir exatamente esse estado (`app/chamados/tests/test_o261e_f5_alcance.py:66-82`). Não executei Django/testes: resultado de execução `[nao verificado]`.

Para `porta_pergunta`, a distinção pendente/respondida tem testemunha existente: a API serve `p.estado` e publica `aguarda_admin` quando `RESPONDIDA` (`app/api/views.py:2620-2624`); a propriedade do modelo traduz pergunta viva por `respondida_em` (`app/chamados/models.py:2197-2211`). O censo não consulta essa tradução: repete a decisão por carimbo (`app/chamados/services/vigia_fio.py:579-580`).

`admin` também não identifica a autoridade de validação. O catálogo atribui `batida_ausente` a `colab` (`app/chamados/catalogo/modulos.py:105`), que o guard administrativo mapeia a `supervisao` (`app/chamados/utils.py:27-43`); o guard exige departamento correspondente ou gestor geral (`app/chamados/utils.py:47-72`). A tela aplica esse guard (`app/chamados/views.py:418-421`) e oferece validar (`app/chamados/views.py:483-501`; `app/templates/chamados/partials/_pergunta_card.html:92-97`). O supervisor pode entrar como gestor porque `supervisor_posto` recebe `is_staff=True` e `_is_gestor` aceita staff (`app/core/credenciais.py:132-140`; `app/chamados/views.py:35-38`). A afirmação “supervisor nunca sai de nenhuma porta” não descreve essas autoridades.

Mesmo respondida, a pergunta continua corrigível pelo colaborador: a tela diz “Aguardando a supervisao conferir” e permite responder de novo (`app/templates/chamados/partials/_card_atendimento.html:94-101`, `:154-155`); a porta HTML encaminha `ja_respondida=True` ao core (`app/chamados/views.py:1280-1283`), que grava nova resposta (`app/chamados/services/disputa_emissao.py:1247-1249`). Assim, `admin` pode representar o próximo responsável por validar; não representa sozinho todos que podem agir.

Pluralidade: duas perguntas elegíveis do mesmo dia, uma pendente e outra respondida, produzem literalmente `admin+colaborador` (`app/chamados/services/vigia_fio.py:576-580`). Isso mantém uma linha por dia, porém não garante “um dono”; ocorrência no dado de produção `[nao verificado]`.

Testes novos: [nao verificado]

- O teste fila cria chamado sem pergunta e exige `admin`; não cobre o contraexemplo de fila com pergunta pendente (`app/chamados/tests/test_o261e_f5_alcance.py:291-297`).
- O teste pergunta respondida exige `admin`, embora sua fixture use `batida_ausente`, cuja autoridade administrativa é supervisão; não consulta guard/tela nem atua com usuário (`app/chamados/tests/test_o261e_f5_alcance.py:66-82`, `:281-289`; `app/chamados/utils.py:27-43`).
- O teste “supervisor nunca” examina apenas os valores das constantes dos baldes, sem executar censo nem testar as duas rotas de porta (`app/chamados/tests/test_o261e_f5_alcance.py:299-304`). Poderia passar com papel errado em `porta_fila` ou `porta_pergunta`, pois não observa esses resultados (`app/chamados/services/vigia_fio.py:574`, `:579-580`).

## 30/10 22:39 · sessao 913a8ab46598 · - · - · origem=Ronald (tmux)

Os quatro baldes recebem `'admin'` por mapa local; o retorno de `dono_fora_do_fio` é apenas o nome do balde, sem papel/RBAC (`app/chamados/services/vigia_fio.py:256-265`, `:481-482`, `:588-592`).

| Balde | Leitura efetiva | Porta e quem pode agir |
|---|---|---|
| `decisao_do_dia_pendente` | `resolver_dia_oferecido_na_data` monta o calendário e lê o predicado; a função declara expressamente que **não verifica `pode_veredito`**, pois é permissão de quem vê (`app/colaboradores/services/calendario.py:199-216`). | O calendário exige `ver_cadastro` e oferece Resolver dia a quem tem `regularizar_batida` (`app/colaboradores/views.py:1935-1957`); template verifica `pode_veredito` (`app/templates/colaboradores/partials/_calendario_grade.html:125-126`); POST exige `regularizar_batida` (`app/ponto/views.py:3302-3306`). Supervisão recebe essa permissão na semeadura padrão (`app/colaboradores/mixins.py:355-362`). |
| `aguarda_decisao_de_ausencia` | `ausencia_cobre(...para='cobranca')` e status em `ABERTAS` (`app/chamados/services/vigia_fio.py:258-260`). `ABERTAS` reúne **aguardando_documento** e **aguardando_decisao** (`app/ponto/services/ausencia.py:749`). | Documento é devido pelo colaborador, decisão pelo DP (`app/ponto/services/ausencia.py:719-722`). Colaborador autenticado anexa na própria ausência pela API (`app/api/views.py:3125-3144`, `:3153-3156`); anexar passa o estado para decisão (`app/ponto/services/ausencia.py:857-859`). Quem tem `editar_ausencias` também anexa/decide pelo painel (`app/ponto/views.py:2134-2152`, `:2589-2599`), mas aprovar diretamente `aguardando_documento` é transição recusada (`app/ponto/services/ausencia.py:751-753`, `:792-806`). **Há atribuição indistinta de admin a estado cuja próxima obrigação é do colaborador.** |
| `escala_nao_descreve_o_dia` | Basta alguma pergunta `VALIDADA` com resposta `FOLGA_CONTESTA`; esse ramo **não consulta presença na lista nem uma porta de correção** (`app/chamados/services/vigia_fio.py:261-264`; código da resposta em `app/chamados/services/validacao.py:216-218`). | A função comenta que o dono é CADASTRO (`app/chamados/services/vigia_fio.py:245-246`). Portas de cadastro existentes: wizard exige `editar_escalas` (`app/escala/views_wizard.py:63-65`, `:95-98`) e vínculo exige `editar_cadastro` (`app/colaboradores/views.py:16-20`). Qual delas está realmente oferecida **para cada dia desse balde**: `[nao verificado]`; não é lido no ramo (`app/chamados/services/vigia_fio.py:261-262`). |
| `fio_do_admin` | Lista é carregada e reduzida a `(colab,data)->dono_do_dia`; predicado só exige dono BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:168`, `:193-210`; `app/escala/services/cadastro_realidade.py:655-670`). | Lista mostra links `a.url` (`app/templates/escala/cadastro_x_realidade.html:28`); C9/BATIDA aponta regularização e C9/CADASTRO aponta template/vínculo (`app/escala/services/cadastro_realidade.py:445-456`). Regularização é declarada porta da supervisão (`app/escala/services/cadastro_realidade.py:20-22`) e exige `regularizar_batida` (`app/colaboradores/views.py:2948-2958`); wizard exige `editar_escalas`, vínculo `editar_cadastro` (linhas acima). Portanto BATIDA/CADASTRO não equivalem a um perfil exclusivo admin. |

As permissões reais são dinâmicas via `user.get_all_permissions()`, com passe de superuser; os perfis efetivamente autorizados em produção são `[nao verificado]` (`app/colaboradores/mixins.py:322-331`). A semeadura padrão concede `regularizar_batida`, `editar_escalas` e `editar_ausencias` à supervisão (`:355-362`), `editar_ausencias`/`editar_cadastro` ao DP (`:363-370`) e conjunto amplo a TI (`:371-382`). Logo a frase “supervisor nunca sai de nenhuma porta” não é sustentada como exclusão de capacidade: a própria porta C9/BATIDA é apresentada como regularização da supervisão (`app/escala/services/cadastro_realidade.py:20-22`).

## 30/10 22:38 · sessao 97f9cda9c180 · O261-E-CELULA-20 · verificador · origem=Code

**Parecer: REPROVA o delta para fechamento da F1.** Há uma regressão demonstrável na relavra S2, perda de isolamento no E2 e um caminho de segunda passada que deixa a impressão anterior ao fechamento. (`app/chamados/models.py:261`, `:278–288`, `:1774`; `app/ponto/services/cartorio.py:751–759`, `:1187–1191`, `:1449`)

**1. Achados**

- **Cache velho: o ponto comprovado é depois do `save`, não entre gate e núcleo.** O gate guarda as perguntas em `_f1_ancoradas`; durante `self.save()`, S143 busca outras instâncias e pode mudar `materializada_em`, `via_resolucao`, `validada_em` e a lavra S2. Ao retornar, o núcleo relavra os objetos anteriores, sem recarregar. (`app/chamados/models.py:357`, `:261`, `:278–288`; `app/chamados/signals.py:89–92`; `app/chamados/services/disputa_emissao.py:1536–1583`)

  Contraexemplo pelo código: pergunta de escolha ainda sem materialização, com célula que absolve e sem marcos pendentes. C6 permite fechar; S143 carimba e lavra **SIM**; a relavra final usa `materializada_em=None` do snapshot e grava **NÃO**. O novo `julgado_em` pode ainda ficar posterior ao carimbo, impedindo que a comparação de marcos detecte esse envelhecimento. Reprodução em banco e ocorrência no dado fornecido: **[não verificado]**. (`app/chamados/juizes.py:628–629`, `:1054–1065`; `app/chamados/services/disputa_emissao.py:1562–1583`; `app/chamados/veredito_lavrado.py:188–190`, `:219–231`)

- **`atomic(savepoint=False)` remove a recuperação local do fechamento de disputa chamado pelo E2.** E2 captura exceções de `DisputaSupervisao.fechar`; agora, havendo transação externa, uma exceção que atravesse o bloco sem savepoint pode marcar essa transação para rollback, embora E2 continue. É a classe de falha já documentada pelo próprio mecanismo de auditoria. Falha efetivamente provocada nesta cópia: **[não verificado]**. (`app/chamados/reconciliador.py:632–640`; `app/chamados/models.py:1774–1797`; `app/core/observ.py:124–133`)

  Isso **não** demonstra defeito nos dois receivers protegidos: Defesa 1 e S143 conservam seus próprios `atomic()`. (`app/chamados/signals.py:41–46`, `:88–94`)

- **A segunda passada pode consumir o fechamento e descartar a convergência.** Se E2 matar a pergunta, mas não fechar seu chamado-destino — E2 fecha o pai da disputa, que pode ser outro chamado —, `_porm` fecha o destino na segunda passada e reenfileira a célula. O wrapper descarta essa nova fila em `rejulgar_descartado`. A impressão foi calculada antes do fechamento e é gravada depois; sem emissão nova, a releitura final não entra. Quantidade de ocorrências: **[não verificado]**. (`app/chamados/reconciliador.py:623–638`; `app/chamados/signals.py:32–42`; `app/ponto/services/cartorio.py:1089–1094`, `:1187–1191`, `:751–759`, `:804–817`, `:1449–1450`)

- **O ciclo de `cobranca_indevida` permanece.** O corte altera `cobrado` no ramo `not cods`; `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO` continuam dependendo do chamado vivo. Ambos derivam `cobranca_indevida`, que mantém C6 vivo e fica excluída do E2. (`app/ponto/services/cartorio.py:1141–1165`; `app/ponto/supra_juiz.py:418–445`; `app/escala/models.py:1581`, `:1735–1736`; `app/chamados/juizes.py:1054–1057`; `app/chamados/reconciliador.py:598`)

**2. Quem escreve e quem lê**

| Estado | Escrita | Leitura relevante |
|---|---|---|
| `CelulaDia.veredito`, incluindo `cobrado` | Porta `lavrar_veredito`; cartório solicita o valor e depois carimba. (`app/ponto/portas/celula.py:678–695`; `app/ponto/services/cartorio.py:1164–1165`, `:1434–1450`) | C6 e seleção do E2. (`app/chamados/juizes.py:1017–1032`, `:1054–1065`; `app/chamados/reconciliador.py:594–599`) |
| `materializada_em` / `via_resolucao` neste E2 | Escritor existente `retratar_perguntas_absolvidas_por_celula`; o delta acrescenta seu chamador. (`app/chamados/reconciliador.py:617–621`; `app/ponto/services/cartorio.py:749–750`) | C6 e juiz usado pela lavra S2. (`app/chamados/juizes.py:1044–1052`, `:1076–1083`; `app/chamados/veredito_lavrado.py:219–221`) |
| `chamado_destino` no agrupamento | `agrupar_furo` chama `reapontar_resolvedora`, que consulta E4-c, atribui, relavra e salva. (`app/chamados/services/limbo_folgas.py:228–230`; `app/chamados/services/fio.py:821–831`) | Âncora canônica `chamado_que_lista` e guarda F1. (`app/chamados/services/fio.py:97–117`; `app/chamados/models.py:336–343`) |
| Outros escritores de `chamado_destino` | `PerguntaDisputa.abrir` também escreve no dedup e no nascimento; portanto a exclusividade da porta não cobre literalmente toda escrita existente. (`app/chamados/models.py:2370–2397`) | Guarda de costura no dedup. (`app/chamados/models.py:2367–2369`) |
| `status_local` | Transição persistente em `_gravar_estado`; troca temporária para julgar o pós-estado em `_vivas_entre`, restaurada no `finally`. (`app/chamados/models.py:222–261`, `:333–346`) | Motor dos gates, tripwire, juiz de materialização e receivers. (`app/chamados/models.py:215`, `:631`, `:1469`; `app/chamados/juizes.py:699`; `app/chamados/signals.py:70–81`) |
| `_f1_ancoradas` | Lista escrita pelo plano; limpa no consumo, recusa e dry-run. (`app/chamados/models.py:357`, `:211`, `:398`, `:645`) | Tripwire e relavra S2. (`app/chamados/models.py:210–218`, `:278–288`) |
| `_f1_dedup` | Premissa prepara o payload; decisão inicializa, consome e limpa. (`app/chamados/models.py:697–701`, `:639–642`) | Decisão emite `fechamento_por_dedup`. (`app/chamados/models.py:648–650`) |
| `fechamento_recusado` | `_registrar_recusa` solicita; `evento` persiste `LogAuditoria`. (`app/chamados/models.py:372–383`; `app/core/observ.py:134–138`) | A própria porta lê último payload e estados posteriores para deduplicar. (`app/chamados/models.py:378–381`) |
| `fechamento_por_dedup` | Decisão solicita após a guarda, **antes** de `_gravar_estado`; persistência por `evento`. (`app/chamados/models.py:643–650`, `:612`; `app/core/observ.py:134–138`) | Testes conferem sobrevivente e idempotência. (`app/chamados/tests/test_fechar3_retorno_do_gate.py:109–117`) |

**3. Limites e efeitos que não devem virar acusação sem prova**

- **Não demonstrei nova travessia vivo→terminal com pergunta viva causada pelo delta.** Antes do núcleo, a reata modifica as mesmas instâncias do cache; o receiver de pergunta agenda `on_commit` sob o atomic aberto. Um mutador concorrente nessa janela permanece **[não verificado]**. (`app/chamados/models.py:408–419`, `:608–612`; `app/chamados/services/fio.py:828–831`; `app/chamados/signals.py:191–202`)

- **Há caminho preexistente que conserva terminal com viva:** nascimento de pergunta com destino terminal continua fora da guarda F1; depois, `resolvido→fechado` não passa pela verificação reservada à saída de VIVOS. Não é prova de regressão nova deste delta. (`app/chamados/models.py:2396–2397`, `:392–393`, `:215`, `:1468–1477`; `app/chamados/services/fio.py:748`)

- **E2 dentro do julgamento não faz apenas o carimbo.** Sua consulta abrange o colaborador inteiro; ele pode fechar disputas, reconciliar pais e acrescentar notas. Com `_tl.ativo=True`, o receiver suprime o julgamento provocado por esses fechamentos; a segunda passada recebe apenas `rejulgar`. Dano concreto em outros dias: **[não verificado]**. (`app/chamados/reconciliador.py:584–599`, `:632–638`; `app/chamados/signals.py:41–45`, `:174–175`; `app/ponto/services/cartorio.py:738–752`)

- **A trilha dedup adiada corrige a emissão quando a guarda recusa**, mas ainda não é confirmação posterior ao save: continua dependente da transação concluir corretamente. (`app/chamados/models.py:643–650`, `:608–613`)

- **LIMBO e O199 usam as portas existentes solicitadas:** reata antes da reconciliação no agrupamento; novo pedido a `fechar_o_fio_do_dia` após o aceite da irmã no dia já folga. Resultado executado e impacto em produção: **[não verificado]**. (`app/chamados/services/limbo_folgas.py:228–230`; `app/chamados/services/validacao.py:261–271`)

- **Não há prova adicional de segundo juiz, segundo escritor de materialização, inversão de trava ou mudança de teto temporal.** O filtro novo chama `marcos_acesos`; E2 conserva a guarda de marcos pendentes. Regressão de relógio/ordem de trava: **[não verificado]**. (`app/ponto/services/cartorio.py:1140`, `:1164`, `:1181`; `app/chamados/reconciliador.py:604–614`)

**4. Testes vermelhos e cobertura** [nao verificado]

- **Vermelho relatado no anexo e sustentado pelo mecanismo:** `PortaSegundoAtoTest.test_MORDE_derruba_a_cobranca_viva_do_dia_que_virou_folga`, que exige a saída de VIVOS. Reexecução independente: **[não verificado]**. (`app/colaboradores/tests/test_contract_porta_2ato.py:147–160`; `app/ponto/supra_juiz.py:441–445`)

- **Não confirmo HX/B2.d como vermelhos atuais:** suas fixtures já plantam explicitamente o passivo terminal, sem depender de `encerrar()` fabricá-lo. Resultado atual: **[não verificado]**. (`app/chamados/tests/test_laco_hx_x_bug31.py:37–43`; `app/chamados/tests/test_b2d_resposta_visivel.py:16–22`)

- **Tetos do chokepoint e contagem das órfãs:** resultado atual **[não verificado]**; não há medição fornecida que autorize afirmar novo excesso ou novo total. Os testes do chokepoint exigem fechamento efetivo antes de aceitar a contagem. (`app/ponto/tests/test_selo_chokepoint_escrita.py:157–164`, `:360–368`)

- **O teste novo da lâmpada não prende a convergência:** verifica fechamento e ausência de viva, mas não `rejulgar_descartado`, impressão estável ou segunda chamada sem escrita. O selo AST prende chamada/raise antes da escrita; não verifica a atualidade dos objetos reutilizados na S2. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:375–390`; `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24–47`)

A exigência de idempotência é “mesmo estado, uma trilha, zero duplicação”; reutilizar o snapshot após os receivers e terminar com impressão anterior ao fechamento deixa essa exigência sem cumprimento demonstrado. (`CLAUDE.md:299–302`; `app/chamados/models.py:278–288`; `app/ponto/services/cartorio.py:757–759`, `:1449–1450`)

## 30/10 22:38 · sessao d814bc670e38 · - · - · origem=Ronald (tmux)

Não encontrei leitor operacional do e6 que omita a fase na cópia atual: e6 usa `InsumosDoDono` e `.dono()` (`app/ponto/management/commands/e6_oraculo.py:309`, `:359`); pautas usam a montagem, incluem fase no pré-filtro e só pedem motor para ESTRUTURA (`app/ponto/management/commands/pautas_discordantes.py:82`, `:96`, `:98`, `:100`, `:109`); teto também usa a montagem (`app/ponto/services/teto_na_lista.py:72`, `:86`); censo injeta os dias do julgamento corrente e grava o resultado do juiz (`app/ponto/management/commands/censo_fase_12x36.py:260`, `:262`, `:273`).

A fase vem de `ultimo_lavrado(self.hoje)`, filtrada pelo colaborador e cacheada (`app/ponto/management/commands/e6_oraculo.py:159`). Ela entra nas duas chamadas finais possíveis ao juiz, inclusive quando o e6 fornece o espelho previamente (`app/ponto/management/commands/e6_oraculo.py:169`, `:173`, `:176`). A precedência efetiva mantém os ramos anteriores de cadastro, depois BATIDA por turno aberto/batida real ou paridade ímpar, depois CADASTRO pela fase (`app/ponto/management/commands/e6_oraculo.py:97`, `:103`, `:105`, `:107`).

O leitor indireto preserva o dono lavrado da C10, mostra esse dono na lista e resolve C9/C10 no mesmo nível; divergência produz `None`, independentemente da ordem (`app/escala/services/cadastro_realidade.py:343`, `:650`, `:710`, `:713`, `:738`). O vigia consulta exatamente `dias_na_lista(lista(hoje=hoje,...))` e só retira do alarme donos BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:259`, `:266`).

Não achei residual procedente novo nesses caminhos; o pré-filtro ainda omite os demais insumos de cadastro, situação já registrada na triagem fornecida (`app/ponto/management/commands/pautas_discordantes.py:98`; classificação histórica: [nao verificado]). Não executei suíte; aprovação dinâmica [nao verificado].

## 30/10 22:38 · sessao e312cdb2f279 · - · - · origem=Ronald (tmux)

O teste novo morde `lista()` diretamente: importa o chamador, substitui apenas o leitor `fase_12x36.ultimo_lavrado`, chama `lista(hoje=30/10)` e exige que esse leitor receba `30/10` (`app/escala/tests/test_o261_v_fase_na_lista.py:208-214`). O relógio está congelado em `05/11`, portanto usar o dia corrente não satisfaz a expectativa (`app/escala/tests/test_o261_v_fase_na_lista.py:177`).

A cadeia efetiva é `lista()` → `_fase_deslocada(empresas, hoje)` → `ultimo_lavrado(hoje)` (`app/escala/services/cadastro_realidade.py:611`, `:335-340`). O leitor aplica `data_ref__lte=ate` (`app/escala/services/fase_12x36.py:102-105`). Retirar o repasse em qualquer elo produz chamada com `None` e deixa de satisfazer a asserção (`app/escala/tests/test_o261_v_fase_na_lista.py:214`). Execução RED/GREEN: `[nao verificado]`.

Não encontrei outro residual procedente na resolução dos donos da lista: C9 e C10 participam igualmente como donos do juiz; divergência resulta em `None`, independentemente da ordem (`app/escala/services/cadastro_realidade.py:710-721`). Os testes cobrem as duas ordens e o controle de concordância (`app/escala/tests/test_o261_v_fase_na_lista.py:334-343`). O vigia só considera o dia atendido pela lista quando o resultado é `BATIDA` ou `CADASTRO`; `None` mantém o alarme (`app/chamados/services/vigia_fio.py:263-266`).

## 30/10 22:37 · sessao af8b9dab4cc6 · - · - · origem=Ronald (tmux)

**Núcleo: não demonstrei um caminho novo do delta que atravesse vivo→terminal deixando pergunta viva.** A guarda usa C6 no pós-estado e reata as mesmas instâncias entregues ao tripwire; os fechadores públicos entram em atomic antes do gate. `app/chamados/models.py:215`, `app/chamados/models.py:326`, `app/chamados/models.py:410`, `app/chamados/models.py:608`, `app/chamados/models.py:726`, `app/chamados/models.py:1474`, `app/chamados/models.py:1492`.

1. **Cache envelhece comprovadamente depois do save, na relavra S2.** `_plano_pergunta_viva` materializa perguntas/células e armazena a lista; `_gravar_estado` reutiliza-a tanto antes quanto depois de `self.save()`. O save executa S143, que busca OUTRAS instâncias e modifica `materializada_em`, `via_resolucao` e eventualmente `validada_em`; depois S2 volta às instâncias antigas, sem refresh. `app/chamados/models.py:321`, `app/chamados/models.py:357`, `app/chamados/models.py:261`, `app/chamados/models.py:278`, `app/chamados/signals.py:91`, `app/chamados/services/disputa_emissao.py:1536`, `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1574`.

   Contraexemplo permitido pelo código: pergunta de escolha sem materialização, com destino vivo e célula que absolve sem pendentes. C6 permite fechar pela célula; S143 carimba e lavra SIM. A S2 final relavra o snapshot cujo `materializada_em` continua nulo e grava NÃO, pois o juiz de escolha lê exatamente esse campo. A gravação recebe `julgado_em` posterior ao carimbo, prejudicando a detecção de staleness por marco. `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1064`, `app/chamados/services/disputa_emissao.py:1547`, `app/chamados/services/disputa_emissao.py:1580`, `app/chamados/juizes.py:628`, `app/chamados/veredito_lavrado.py:219`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:188`. Existência desse caso em produção ou execução de teste: **[não verificado]**.

   Alternativa sem depender de escolha com destino: pergunta textual muda, dia absolvido por ausência aprovada, sem turno com saída. S143 escreve `ausencia_dia`, via que o juiz aceita com materialização; a instância anterior não tem esses campos e cai no teste de completude de turno. `app/chamados/juizes.py:1194`, `app/chamados/models.py:2863`, `app/chamados/juizes.py:603`, `app/chamados/juizes.py:733`. Reprodução executada: **[não verificado]**.

2. **Entre gate e entrada no tripwire, não achei mutador sequencial que invalide a lista.** Reata altera as próprias perguntas da lista. O receiver de pergunta apenas agenda `on_commit`, adiado pelo atomic já aberto. Concorrência pode introduzir pergunta ou alterar outra instância, mas ocorrência e impacto nesta cópia são **[não verificado]**. `app/chamados/models.py:410`, `app/chamados/services/fio.py:828`, `app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/models.py:608`.

3. **`atomic(savepoint=False)` no fechamento de disputa perde a fronteira de recuperação do E2.** E2 chama `DisputaSupervisao.fechar` e captura qualquer exceção; a porta agora usa atomic sem savepoint. Havendo transação externa, uma exceção que atravesse esse bloco marca a transação externa para rollback, embora E2 continue após capturá-la. É a mesma classe descrita explicitamente pelo mecanismo existente de auditoria. `app/chamados/reconciliador.py:632`, `app/chamados/reconciliador.py:636`, `app/chamados/reconciliador.py:639`, `app/chamados/models.py:1774`, `app/core/observ.py:124`.

   Não atribuir esse defeito automaticamente ao receiver S143: ele conserva savepoint próprio; Defesa 1 também conserva atomic próprio em volta da reconciliação. `app/chamados/signals.py:41`, `app/chamados/signals.py:91`. O teste existente de falha engolida em S143 verifica precisamente a preservação do fechamento, mas não cobre essa nova chamada de E2 dentro do julgamento. `app/ponto/tests/test_justificativa_encerra_origem.py:233`, `app/ponto/tests/test_justificativa_encerra_origem.py:250`. Vermelho efetivamente executado: **[não verificado]**.

4. **Limite pré-existente: nascimento F2 e terminal→terminal.** `PerguntaDisputa.abrir` permite criar pergunta nova com destino terminal; a própria guarda de costura declara o nascimento fora da F1. Se chamado já resolvido possui pergunta viva, `encerrar()` pode passar a fechado: gate e tripwire só examinam a saída de VIVOS. Isso não demonstra uma nova regressão vivo→terminal do delta. `app/chamados/models.py:2396`, `app/chamados/services/fio.py:748`, `app/chamados/models.py:392`, `app/chamados/models.py:215`, `app/chamados/models.py:1468`, `app/chamados/models.py:1477`.

**Escritores/leitores solicitados**

- `status_local`: declaração/default em `app/chamados/models.py:46`; nascimentos em `app/chamados/models.py:1009` e `app/chamados/models.py:1026`; transições persistentes em `_gravar_estado`, atribuição `:222` e save `:261`; troca temporária e restauração em `:333` e `:346`. Leitores relevantes: motor/gates `:631`, `:720`, `:1469`; C6 por meio do juiz de materialização `app/chamados/juizes.py:699`; receivers `app/chamados/signals.py:70`, `app/chamados/signals.py:81`, `app/chamados/signals.py:169`.
- `_f1_ancoradas`: escritor da lista `app/chamados/models.py:357`; consumidor `:210`–`:212`; limpezas `:211`, `:398`, `:645`; uso na guarda `:218` e na relavra `:278`.
- `_f1_dedup`: inicialização `app/chamados/models.py:639`; escrita pendente na premissa `:697`; consumo/limpeza `:642`; emissão `:650`. Não encontrei outro leitor/escritor no código pesquisado. `app/chamados/models.py:639`, `app/chamados/models.py:642`, `app/chamados/models.py:697`.
- `fechamento_por_dedup`: solicitante único agora `_decisao_do_fechamento`, após o gate de pergunta viva, mas ainda antes da escrita do estado. Escritor físico de LogAuditoria é `evento`. Leitores de teste: `test_fechar3_retorno_do_gate` e `test_fechar3_troca_escala_gate`. `app/chamados/models.py:643`, `app/chamados/models.py:650`, `app/chamados/models.py:612`, `app/core/observ.py:134`, `app/chamados/tests/test_fechar3_retorno_do_gate.py:115`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:157`.
- `fechamento_recusado`: solicitante único `_registrar_recusa`, que lê último payload e mudanças posteriores de estado para deduplicar; gate de pergunta e premissa chamam essa porta. Escritor físico permanece `evento`. `app/chamados/models.py:372`, `app/chamados/models.py:378`, `app/chamados/models.py:380`, `app/chamados/models.py:383`, `app/chamados/models.py:400`, `app/chamados/models.py:708`, `app/core/observ.py:134`. Teste de idempotência da recusa: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`.

**Trilha dedup adiada:** a mudança elimina a linha quando a guarda de pergunta recusa; não transforma a linha em confirmação posterior ao save. Sua consistência continua dependendo do atomic e da ausência de rollback engolido, descrito acima. `app/chamados/models.py:643`, `app/chamados/models.py:646`, `app/chamados/models.py:650`, `app/chamados/models.py:612`.

## 30/10 22:36 · sessao ce5c5ea4f5f2 · - · - · origem=Ronald (tmux)

Achados do cartório, por leitura estática; execução e contagens reais permanecem [não verificado].

- **A segunda passada pode deixar trabalho pendente e impressão velha.** Um chamado adiado entra em `rejulgar`; na segunda passada, quando `_porm` finalmente o fecha, a célula é novamente enfileirada. O wrapper descarta essa fila e incrementa `rejulgar_descartado`. A impressão foi calculada com o chamado vivo antes do fechamento e é gravada depois; sem emissão nova, a releitura final não roda. Caminho: `app/ponto/services/cartorio.py:1075`, `:1089`, `:1181`, `:1187`, `:1190`, `:1434`, `:1449`, `:751`, `:757`, `:804`. Isso ocorre quando E2 mata a pergunta mas o destino continua vivo até `_porm`; E2 fecha o **pai da disputa**, que pode ser outro chamado (`app/chamados/reconciliador.py:623`, `:632`; `app/chamados/signals.py:32`, `:42`). Número de ocorrências [não verificado].

- **O ciclo `cobranca_indevida` permanece, incluindo `COBRANCA_SEM_FURO`.** O delta só altera o ramo `not cods` de `cobrado`; o suprajuiz continua emitindo `COBRANCA_SEM_FURO` quando o dia completo tem chamado vivo, e `COBRANCA_EM_DIA_SEM_TRABALHO` na folga com chamado vivo. Ambos derivam `cobranca_indevida`, que mantém C6 vivo; E2 exclui vereditos acusatórios. Fontes: `app/ponto/services/cartorio.py:1141`, `:1164`; `app/ponto/supra_juiz.py:418`, `:441`; `app/escala/models.py:1581`, `:1735`; `app/chamados/juizes.py:1054`; `app/chamados/reconciliador.py:598`. O teste específico da porta é `app/colaboradores/tests/test_contract_porta_2ato.py:147`, com exigência terminal em `:158`; vermelho segundo o anexo, execução independente [não verificado].

- **E2 dentro do julgamento tem efeitos além do carimbo e alcance além das células julgadas.** Sua consulta abrange perguntas absolvidas de todo o colaborador; fecha disputas, cujo receiver reconcilia o pai e escreve nota. Tudo isso roda com `_tl.ativo=True`; o julgamento provocado pela mudança do pai é suprimido. A segunda passada só recebe a fila selecionada, sem incorporar automaticamente outros dias alterados por E2. Fontes: `app/ponto/services/cartorio.py:738`, `:744`, `:750`, `:751`; `app/chamados/reconciliador.py:584`, `:632`; `app/chamados/signals.py:41`, `:174`. Ocorrência concreta em produção [não verificado].

- **Não há prova suficiente de terminal com pergunta viva pela relavra posterior.** Para esse caminho, a primeira passada precisa absolver, E2 carimbar e a segunda voltar a acusar. Com insumos e janela iguais, isso não decorre apenas da ordem. A janela efetivamente pode mudar: a segunda passada recebe subconjunto e `_julgar_colab_corpo` deriva a janela dos extremos desse subconjunto (`app/ponto/services/cartorio.py:752`, `:835`, `:861`, `:880`), embora a documentação exija lista inteira justamente para evitar pareamento diferente (`:725`). Contraexemplo concreto [não verificado].

Mapa principal:

| Estado | Escritor | Leitores |
|---|---|---|
| `CelulaDia.veredito` | Porta `lavrar_veredito`, `app/ponto/portas/celula.py:647`, atribuição `:692`; cartório chama em `app/ponto/services/cartorio.py:1434` | C6: `app/chamados/juizes.py:1017`, `:1054`; E2: `app/chamados/reconciliador.py:594`; impressão/pulo: `app/ponto/services/cartorio.py:1098` |
| `materializada_em` / `via_resolucao` neste E2 | Porta existente `app/chamados/reconciliador.py:617`, `:619`, persistência `:620` | C6: `app/chamados/juizes.py:1044`, `:1050`, `:1076`; lavra S2 delega ao juiz em `app/chamados/veredito_lavrado.py:220` |

O novo teste de evento confere terminal e ausência de pergunta viva, mas não confere `rejulgar_descartado`, impressão estável nem segunda chamada sem escrita (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:385`). O selo existente mede queries e callbacks exatos; seus resultados nesta cópia são [não verificado], com limites em `app/ponto/tests/test_selo_chokepoint_escrita.py:157`, `:177`, `:212`.

## 30/10 22:36 · sessao 5581f0c9f9f7 · LAUDO-3010 · leitura · origem=Code

**O achado é uma diferença entre “pergunta viva”, “materialização comprovada” e “porta alcançável pelo admin”. As duas funções nomeadas respondem às duas primeiras coisas; nenhuma verifica integralmente a alcançabilidade exigida pela O261 (A).** (`app/folha/fora_da_folha.py:66`; `app/chamados/services/raiox.py:173`; `app/docs/BACKLOG.md:767`)

Vínculo da cópia com o hash informado/main: **[não verificado]**. Estado persistido da p#34375: **[não verificado]**.

**1. Predicados, insumos, consumidores e registro**

**`o_que_segura(colab, mes, ano)`** devolve os dias retentores com `fio`, `destrava` e chamados; a decisão sobre quem deve agir está em **`_fio_do_dia`**, nesta ordem exata:

| Condição | Resultado |
|---|---|
| Existe pergunta viva da célula com `estado == PENDENTE` | `colab` |
| Senão, existe alguma pergunta viva | `admin` |
| Senão, existe chamado da célula com status em `VIVOS` | `chamado` |
| Senão | `nada` |

Sem `celula_id`, `o_que_segura` força `nada`. Portanto, **`admin` exige alguma viva e nenhuma viva pendente**; `colab` significa aguardar o colaborador, não ação imediata de validação pelo admin. (`app/folha/fora_da_folha.py:66`; `app/folha/fora_da_folha.py:67`; `app/folha/fora_da_folha.py:69`; `app/folha/fora_da_folha.py:71`; `app/folha/fora_da_folha.py:204`; `app/folha/fora_da_folha.py:39`)

Os insumos são colaborador, empresa e competência; `classificar_export` informa entrada/motivo; `motivos_retencao_celula(com_dias=True)` fornece os dias; `_alimentar` carrega perguntas pela FK da célula e chamados em `VIVOS`. Não exige que a pergunta esteja ancorada num chamado vivo **da fila**. (`app/folha/fora_da_folha.py:188`; `app/folha/fora_da_folha.py:191`; `app/folha/fora_da_folha.py:93`; `app/folha/fora_da_folha.py:51`; `app/folha/fora_da_folha.py:54`)

A `.viva` delega a **`pergunta_viva`**. Sua precedência atual é:

- Validação ou tranca tornam a pergunta morta; resposta com texto, sem decisão administrativa do dia, mantém viva salvo concordância comprovada com a luz num dia que não acusa. (`app/chamados/models.py:2369`; `app/chamados/juizes.py:1006`; `app/chamados/juizes.py:1010`; `app/chamados/juizes.py:1020`; `app/chamados/juizes.py:1032`)
- Depois vêm encerramento por identidade, marco materializado aceso e veredito da célula: acusador mantém viva; absolvição sem marcos pendentes mata. (`app/chamados/juizes.py:1039`; `app/chamados/juizes.py:1050`; `app/chamados/juizes.py:1054`)
- Sem decisão nesses degraus, consulta a classificação de falta na janela viva; sem carimbo vive; com carimbo, devolve o inverso de `_pergunta_materializada_de_fato`; exceção nesse último degrau resulta em morta. (`app/chamados/juizes.py:1066`; `app/chamados/juizes.py:1076`; `app/chamados/juizes.py:1083`)

A `.estado` distingue pendente/respondida por **`respondida_em`**, sem exigir texto nesse rótulo. Assim, pergunta viva com timestamp de resposta e texto vazio pode produzir `fio=admin`; esse caso está documentado no laudo anterior. (`app/chamados/models.py:2207`; `app/docs/RELATO.md:363`)

**`pergunta_sem_acao_possivel(p, marcos=None)`** tem outro predicado, exatamente:

```text
materializada_em é None                  → False
com carimbo e veredito_lavrado preenchido → veredito_lavrado == VL_SIM
com carimbo e veredito vazio              → bool(_pergunta_materializada_de_fato(p))
exceção no bloco                         → False
```

O parâmetro `marcos` não é consumido pelo corpo; não há checagem de frescor nele. O retorno `False` significa que esse teste **não comprovou materialização sem ação**, não que exista porta administrativa. (`app/chamados/services/raiox.py:151`; `app/chamados/services/raiox.py:173`; `app/chamados/services/raiox.py:182`; `app/chamados/services/raiox.py:186`)

O fallback usa via/carimbo, data/janela, classificação de falta, tipo de resposta, slot esperado, batidas plantadas e não retratadas, destino encerrado em determinados ramos e completude do turno. Portanto, o destino pode entrar como testemunha indireta, mas não existe requisito geral “âncora viva em `qs_na_fila`”. (`app/chamados/juizes.py:603`; `app/chamados/juizes.py:628`; `app/chamados/juizes.py:674`; `app/chamados/juizes.py:699`; `app/chamados/juizes.py:730`)

**Chamadores de produção, distinguindo função pública e núcleo efetivamente usado:**

| Função | Chamadores diretos |
|---|---|
| `o_que_segura` | **0 chamadas estáticas localizadas**; preservada como ferramenta HAIKU de uso humano. (`app/docs/BACKLOG.md:1484`) |
| `_fio_do_dia` | **3 funções / 3 chamadas**: `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. (`app/folha/fora_da_folha.py:154`; `app/folha/fora_da_folha.py:204`; `app/ponto/services/pautas_dos_discordantes.py:75`) |
| `pergunta_sem_acao_possivel` | **1 função / 1 chamada**: `montar_chamados_fio`, pelo alias `_sem_acao`. (`app/chamados/services/fio.py:275`; `app/chamados/services/fio.py:280`) |

O núcleo da folha chega à **tela do retrato** por `fora_da_folha → montar_linhas → lavrar_retrato_folha`; também seleciona dias para pautas dos discordantes. O vigia do fio usa outra redução: `fio_do_dia → fio_vivo → any(pergunta_viva(...))`. (`app/folha/retrato_folha.py:1157`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/views_retrato.py:246`; `app/ponto/services/pautas_dos_discordantes.py:75`; `app/chamados/services/vigia_fio.py:164`; `app/chamados/juizes.py:1111`)

O raio-X compõe `tem_pergunta_viva = any(p.viva and not _sem_acao(p))`. Essa flag é consumida pelo **formulário genérico do colaborador**; o “Validar” do modal administrativo tem condições próprias. A montagem é chamada por atendimento do colab, fio administrativo, modal e render parcial. (`app/chamados/services/fio.py:279`; `app/templates/chamados/partials/_card_atendimento.html:203`; `app/templates/chamados/partials/_card_atendimento.html:205`; `app/templates/chamados/partials/modal_fio.html:210`; `app/templates/chamados/partials/modal_fio.html:272`; `app/chamados/views.py:569`; `app/chamados/views.py:677`; `app/chamados/views.py:889`; `app/chamados/services/fio.py:442`)

O irmão SQL **`creditos_perguntas`** também exclui carimbada com `VL_SIM`, mas **não chama** `pergunta_sem_acao_possivel`; alimenta a partição por verbos. Nela, `validar` e `decidir` são os verbos contabilizados como exigência do admin. (`app/chamados/services/raiox.py:133`; `app/chamados/services/verbos.py:31`; `app/chamados/services/verbos.py:19`; `app/chamados/services/verbos.py:24`)

Pela regra, há ainda um teste explícito de ação administrativa no dia: **`resolver_dia_oferecido`** exige último recurso, dia de trabalho, passado e status em `falta/irregular/aberto/sem_certidao`. O calendário grava esse resultado; template e vigia o leem. (`app/colaboradores/services/calendario.py:196`; `app/colaboradores/services/calendario.py:204`; `app/colaboradores/services/calendario.py:945`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294`)

**Registro:** as duas funções nomeadas não estão individualizadas nos catálogos consultados; estão registrados os componentes `VIVOS`/C1, `pergunta_viva`/C6, `fio_vivo`/C15 e `resolver_dia_oferecido`/TT11. O próprio registro alerta que **chamado vivo não responde se há cobrança/fio**; `JUIZES.md` manda prevalecer o registro vivo. (`app/core/juizes.py:100`; `app/core/juizes.py:233`; `app/core/juizes.py:238`; `app/core/juizes.py:247`; `app/core/juizes.py:684`; `app/docs/JUIZES.md:11`; `app/docs/JUIZES.md:14`)

**2. Onde discordam — e onde apenas deixam de verificar a porta**

Os estados pedidos não bastam para determinar a discordância: é necessário acrescentar **carimbo de materialização e veredito lavrado**. A folha consulta vitalidade; o raio-X consulta esses dois insumos. (`app/folha/fora_da_folha.py:66`; `app/chamados/services/raiox.py:173`)

Inferências abaixo consideram uma única pergunta relevante na célula:

| Combinação | Folha | Raio-X |
|---|---|---|
| Viva, respondida, não validada; materializada e `VL_SIM` | `admin` | **Sem ação = True**: discordância direta. (`app/folha/fora_da_folha.py:69`; `app/chamados/services/raiox.py:183`) |
| Viva, pendente; materializada e `VL_SIM` | `colab` | **Sem ação = True**: divergem sobre continuidade do fio, sem a folha prometer validação imediata. (`app/folha/fora_da_folha.py:67`; `app/chamados/services/raiox.py:183`) |
| Viva, respondida; sem materialização; âncora terminal ou viva fora da fila | `admin` | **Sem ação = False**; ambos deixam passar esse aspecto, embora falte a alcançabilidade da O261. (`app/folha/fora_da_folha.py:69`; `app/chamados/services/raiox.py:173`; `app/docs/BACKLOG.md:767`) |
| Morta/validada, sem materialização, sem outro chamado vivo | `nada` | **Sem ação = False**; inverter esse booleano fabricaria uma possibilidade que a vitalidade não reconhece. (`app/chamados/juizes.py:1006`; `app/folha/fora_da_folha.py:73`; `app/chamados/services/raiox.py:173`) |
| Nenhuma viva; existe chamado `registrado` ou silenciado | `chamado`, com instrução de resolver | A fila não o cobra; `registrado` é vivo, mas não pede ação. (`app/folha/fora_da_folha.py:71`; `app/folha/fora_da_folha.py:41`; `app/chamados/catalogo/motor.py:24`; `app/chamados/services/painel.py:186`) |

**Disputa fechada não elimina automaticamente a porta do admin nesta árvore.** A montagem administrativa inclui perguntas vivas de disputas fechadas; o POST de validação exige vitalidade nesse caso e segue sem reabrir a disputa. (`app/chamados/services/fio.py:213`; `app/chamados/services/fio.py:234`; `app/chamados/services/fio.py:594`; `app/chamados/services/fio.py:603`)

Também não cabe afirmar “âncora terminal = impossível agir em qualquer tela”: o modal administrativo carrega todos os chamados do colaborador, e o POST verifica permissão e correspondência da âncora, sem exigir participação na fila. **Estar acessível manualmente e cumprir o caminho exigido pela O261 são condições distintas.** (`app/chamados/services/fio.py:174`; `app/chamados/services/fio.py:117`; `app/chamados/services/fio.py:557`; `app/chamados/services/fio.py:592`; `app/docs/BACKLOG.md:767`)

Para **p#34375**, não localizei testemunho do estado na árvore **[não verificado]**. Condicionalmente: se viva/respondida e carimbada com `VL_SIM`, reproduz a primeira discordância; se viva sob terminal/fora da fila, reproduz a lacuna de alcançabilidade. O caso documental de **col292, 21/09**, nomeia outras perguntas, **p#34412/p#51322**; não há base para substituir esses identificadores por 34375. (`app/folha/fora_da_folha.py:69`; `app/chamados/services/raiox.py:183`; `app/docs/RELATO.md:359`)

**3. Proximidade da lei e alcance**

**Quanto à forma “consumidor lê o veredito gravado”, o raio-X é mais próximo:** normalmente lê `veredito_lavrado`; porém ainda chama o juiz quando o campo está vazio. A folha delega à vitalidade, cujo caminho pode descer a classificação de falta e julgamento de lastro. (`CLAUDE.md:276`; `app/chamados/services/raiox.py:182`; `app/chamados/services/raiox.py:184`; `app/chamados/juizes.py:1068`; `app/chamados/juizes.py:1083`)

**Quanto à regra completa da O261 — porta viva alcançável — nenhuma das duas basta.** A folha aceita viva sem conferir âncora/fila; o raio-X não verifica sequer vitalidade no próprio corpo. A exigência documental acrescenta chamado vivo em `qs_na_fila` e presença no app. Alcance direto: **`o_que_segura` 0; raio-X 1; núcleo `_fio_do_dia` 3**, conforme os sítios acima. (`app/folha/fora_da_folha.py:66`; `app/chamados/services/raiox.py:173`; `app/docs/BACKLOG.md:767`; `app/docs/BACKLOG.md:1484`; `app/chamados/services/fio.py:280`; `app/folha/fora_da_folha.py:154`; `app/folha/fora_da_folha.py:204`; `app/ponto/services/pautas_dos_discordantes.py:75`)

**4. “T3” da O261 (A), existência e tela**

A identificação literal **“T3 da O261 (A)” não foi localizada nos cinco documentos indicados [não verificado]**. Há três divisões documentais diferentes:

- **O261 original:** FASES 1–3; a terceira é invariante, vigia/contador, caso que morde e passivo. (`app/docs/BACKLOG.md:1259`; `app/docs/TICKETS.md:146`)
- **O261-COMPLEMENTO-[nome]:** itens **(A) alcançabilidade, (B) materializada sem resposta, (C) pai terminal, (D) vigia e link**. (`app/docs/BACKLOG.md:767`)
- **Proposta numerada (1)–(4):** o **(3)** pede que reabrir levante o véu; o documento não o chama de T3. (`app/docs/RELATO.md:370`)

**A alcançabilidade de (A) não está implementada no caminho atual do censo:** ele parte de chamados vivos com célula e aceita `fio_vivo` sobre as perguntas da célula, sem exigir a combinação âncora/fila/app. O contador existente aparece no **placar ESTADO, “Fio morto (O261)”**. (`app/chamados/services/vigia_fio.py:186`; `app/chamados/services/vigia_fio.py:270`; `app/chamados/services/vigia_fio.py:286`; `bin/gerar_estado.py:313`; `bin/fio_estado.py:101`)

**A FASE 3 original tem implementação**, com comando e elo do cron; isso não comprova o complemento posterior de alcançabilidade. (`app/chamados/management/commands/vigiar_fio_morto.py:160`; `app/chamados/management/commands/vigiar_fio_morto.py:195`; `app/config/crons.py:1654`; `app/docs/BACKLOG.md:767`)

**O (3) numerado não aparece no corpo atual de `reabrir`:** após as guardas, ele grava o estado e ressuscita perguntas, sem chamada para levantar o véu. (`app/chamados/models.py:1182`; `app/chamados/models.py:1194`)

Na tela administrativa, existe **“O que fazer / O que destrava” no painel do colaborador**, com texto de `DESTRAVA` e botões genéricos. O Raio-X do retrato abre esse painel; as datas são `<span>`, sem link por dia/pergunta. Portanto, o link específico exigido no complemento (D) não é o que esses templates entregam. (`app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/folha/retrato_folha.py:1068`; `app/templates/folha/_retrato_o_que_fazer.html:46`; `app/templates/folha/_retrato_o_que_fazer.html:61`; `app/templates/folha/_retrato_raiox.html:68`; `app/docs/BACKLOG.md:767`)

## 30/10 22:36 · sessao f69ca6868bc3 · - · - · origem=Ronald (tmux)

- **LIMBO: ordem solicitada presente.** `agrupar_furo` chama `_vivas_ancoradas('resolvido')`, reaponta pela porta existente e só depois pede `reconciliar()`; a porta consulta E4-c e a guarda de costura, atribui `chamado_destino`, relavra S2 e salva. O teste exige pergunta viva ancorada no pai vivo e segunda execução sem novo agrupamento. `app/chamados/services/limbo_folgas.py:228-230`; `app/chamados/services/fio.py:821-831`; `app/chamados/tests/test_t34_furo_sob_limbo.py:43-61`.

- **Quem escreve/lê `chamado_destino`:** a porta `reapontar_resolvedora` escreve em `fio.py:828-831`; `PerguntaDisputa.abrir` também escreve no dedup e no nascimento em `models.py:2370-2397`. As atribuições de `models.py:342` e `fio.py:758-762` servem à simulação em memória. A autoridade da âncora retorna destino ou container em `fio.py:97-117`; a montagem do fio carrega os destinos em `fio.py:199-228`. Todos os caminhos citados estão em `app/chamados/`.

- **O199: reaproveita o fechador existente.** No ramo “dia já folga”, carimba a validação, fecha a pergunta e chama `fechar_o_fio_do_dia`; esse fechador percorre as perguntas e pede reconciliação dos chamados, anotando apenas quando fecham. A validação mata a pergunta no primeiro degrau de C6; irmã respondida ainda não validada pode continuar viva antes da leitura do carimbo. `app/chamados/services/validacao.py:261-271`; `app/ponto/services/veredito_celula.py:174-187`; `app/chamados/juizes.py:1006-1007`; `app/chamados/juizes.py:1020-1032`. O teste das quatro irmãs cobra uma folga, chamado resolvido e uma nota. `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167-192`.

- **Vermelho reportado e mecanismo confirmado por leitura:** `PortaSegundoAtoTest.test_MORDE_derruba_a_cobranca_viva_do_dia_que_virou_folga` exige que o chamado saia de VIVOS. `app/colaboradores/tests/test_contract_porta_2ato.py:147-160`. O protesto `COBRANCA_EM_DIA_SEM_TRABALHO` depende do chamado vivo; vira `cobranca_indevida`, que continua em `VEREDITOS_ACUSAM`; C6 mantém a pergunta viva e E2 exclui essa célula. O cartório só pede rejulgamento desse ramo quando alguma reconciliação fecha. `app/ponto/supra_juiz.py:441-445`; `app/escala/models.py:1581`; `app/escala/models.py:1735`; `app/chamados/juizes.py:1054-1057`; `app/chamados/reconciliador.py:598`; `app/ponto/services/cartorio.py:1213-1225`. Execução local do teste: **[não verificado]**.

- **Não repetir como vermelhos comprovados os testes HX/B2.d.** A cópia já planta explicitamente o passivo terminal por `update`, em vez de depender de `encerrar()` produzir esse estado. `app/chamados/tests/test_laco_hx_x_bug31.py:37-43`; `app/chamados/tests/test_b2d_resposta_visivel.py:16-22`. Resultado atual dessas suítes, das órfãs e dos tetos de queries: **[não verificado]**.

- **Limite do agrupamento, preexistente no trecho fornecido:** o contexto é salvo antes da reconciliação; se esta não fechar, nova chamada com o mesmo `agrupado_em` retorna antes de tentar novamente. `app/chamados/services/limbo_folgas.py:217-231`. Não encontrei cenário normal novo de recusa da reata com os pais vivos selecionados pelos chamadores; ocorrência em produção: **[não verificado]**. Os chamadores selecionam pai vivo em `app/chamados/services/limbo_folgas.py:243-255` e `app/ponto/management/commands/detectar_ausencias.py:755-756`.

## 30/10 22:34 · sessao f86cff5c627c · - · - · origem=Ronald (tmux)

Não encontrei **“T3 da O261 (A)”** nos cinco documentos indicados; a correspondência exata fica **[não verificado]**. Há divisões diferentes que não devem ser confundidas:

- **O261 original:** usa **FASE 1, FASE 2, FASE 3**. A FASE 3 pede invariante, caso que morde, allowlist zero, hoje fora, contador no placar e passivo pela porta. Não contém divisão (A)..(D). — `app/docs/BACKLOG.md:1259`; `app/docs/TICKETS.md:146`.
- **O261-COMPLEMENTO-[nome]:** contém **(A)..(D)**. (A) é **alcancabilidade real**: pergunta viva sob chamado vivo em `qs_na_fila`, aparecendo no app; pergunta viva sob chamado terminal conta como fio morto. (B) materializada sem resposta; (C) nascimento com pai terminal; (D) vigia e link do retrato levando à pergunta pelo chamado vivo. — `app/docs/BACKLOG.md:767`; `app/docs/RELATO.md:355`; `app/docs/RELATO.md:359`.
- **Proposta de quatro curas:** (1) vigia pelo universo `o_que_segura`; (2) resposta vazia; **(3) reabertura levanta o véu**; (4) link por dia. Chamar o terceiro de “T3” seria inferência, pois o documento o chama somente “(3)”. — `app/docs/RELATO.md:370`.

**Existência atual:**

- A **FASE 3 original tem implementação**: `censo` lê chamados vivos e células que acusam; elimina dias com fio vivo, hoje e decisão do dia pendente. O comando executa, imprime e carimba; o cron o agenda. — `app/chamados/services/vigia_fio.py:168`, `app/chamados/services/vigia_fio.py:186`, `app/chamados/services/vigia_fio.py:229`, `app/chamados/services/vigia_fio.py:286`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/management/commands/vigiar_fio_morto.py:195`, `app/config/crons.py:1654`.
- Seu lugar visível é o **placar ESTADO**, seção **“Fio morto (O261)”**. — `bin/gerar_estado.py:313`; `bin/fio_estado.py:101`.
- O **(A) de alcancabilidade não aparece aplicado nesse censo**: ele agrupa perguntas pela FK da célula e aceita `fio_do_dia(ps)`; esse trecho não verifica `qs_na_fila`, âncora viva nem presença no app. — `app/chamados/services/vigia_fio.py:270`, `app/chamados/services/vigia_fio.py:286`. O próprio relatório registra perguntas vivas visíveis no app, sob chamado terminal ausente da fila admin. — `app/docs/RELATO.md:359`.
- O link atual do **Raio-X do retrato** abre o **painel do colaborador**; as datas do bloco “O que fazer” são apenas `<span>`, sem link por dia/pergunta. — `app/templates/folha/_retrato_raiox.html:68`; `app/templates/folha/_retrato_o_que_fazer.html:43`.
- O bloco **“O que fazer / O que destrava”** existe no drawer/painel do colaborador, lê a frase genérica `DESTRAVA`, e não interpola um chamado específico nessa frase. — `app/folha/retrato_folha.py:1049`, `app/folha/retrato_folha.py:1057`, `app/folha/retrato_folha.py:1068`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:28`, `app/templates/folha/_retrato_o_que_fazer.html:33`.

## 30/10 22:33 · sessao fc7f73f9089d · - · - · origem=Ronald (tmux)

Achados de `raiox.pergunta_sem_acao_possivel`, somente leitura:

- **Predicado atual:** se `materializada_em is None`, devolve `False`; com carimbo, se `veredito_lavrado` estiver preenchido, devolve exclusivamente `veredito_lavrado == VL_SIM`; sem veredito, executa `_pergunta_materializada_de_fato(p)`; qualquer exceção devolve `False`. `marcos` é recebido, mas não usado. Não verifica staleness, vitalidade, `validada_em`, disputa aberta, status da âncora ou fila. `app/chamados/services/raiox.py:151`, `app/chamados/services/raiox.py:173`, `app/chamados/services/raiox.py:182`, `app/chamados/services/raiox.py:186`.
- **Insumos do fallback:** via de resolução/carimbo, data e janela viva/classificação de falta; tipo escolha; para hora, slot esperado e batidas plantadas/vivas, destino encerrado e retratação auditada quando não há slot; para texto, turno completo ou retratação auditada. `app/chamados/juizes.py:603`, `app/chamados/juizes.py:628`, `app/chamados/juizes.py:674`, `app/chamados/juizes.py:699`, `app/chamados/juizes.py:730`, `app/chamados/juizes.py:734`.
- **Um chamador direto de produção / um call site:** `montar_chamados_fio`, alias `_sem_acao`, compõe `ch.tem_pergunta_viva = any(p.viva and not _sem_acao(p, _marcos_s2))`. `app/chamados/services/fio.py:275`, `app/chamados/services/fio.py:279`.
- **Quatro chamadores de `montar_chamados_fio`:** `meu_atendimento`, `fio_colaborador`, closure `_resp_modal_full` do modal, `contexto_parcial_fio`. `app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`, `app/chamados/services/fio.py:442`.
- **O uso efetivo da flag é COLAB:** único consumo de template encontrado é o formulário genérico de hora do `_card_atendimento`, condicionado a chamado aberto/em análise, furo e ausência de `tem_pergunta_viva`. O questionário usa `p.viva` diretamente. `app/templates/chamados/partials/_card_atendimento.html:203`, `app/templates/chamados/partials/_card_atendimento.html:205`, `app/templates/chamados/partials/_card_atendimento.html:77`.
- **ADMIN não lê essa flag para o Validar:** modal exige disputa + permissão; com resposta, `pendencia_admin` ou `validada_em` mostram estado e retiram o Validar; caso contrário mostra Validar. Disputa fechada só retira Rejeitar/Corrigir. `app/templates/chamados/partials/modal_fio.html:120`, `app/templates/chamados/partials/modal_fio.html:210`, `app/templates/chamados/partials/modal_fio.html:215`, `app/templates/chamados/partials/modal_fio.html:221`, `app/templates/chamados/partials/modal_fio.html:242`, `app/templates/chamados/partials/modal_fio.html:272`, `app/templates/chamados/partials/modal_fio.html:282`.
- **Âncora terminal/fora da fila ainda pode ter porta manual no modal:** listagem administrativa traz todos os chamados do colaborador; a âncora é destino ou pai sem ler status. Backend exige RBAC, correspondência da âncora e, em disputa fechada, pergunta viva; não exige chamado na fila. Portanto não afirmar “terminal = sem porta em toda tela”. `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:557`, `app/chamados/services/fio.py:560`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:599`.
- **Validar já validada não age:** porta responde `ja_validada`; sem resposta, `sem_resposta`. `app/chamados/services/validacao.py:356`, `app/chamados/services/validacao.py:359`.
- **Fila não equivale a vivo:** `qs_na_fila` exige `COBRANCA` e exclui silêncio futuro; `registrado` é vivo, mas não cobrança; terminais são resolvido, fechado, cancelado, superado. `app/chamados/services/painel.py:186`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:31`.
- **Não confundir o irmão SQL:** `creditos_perguntas` exclui carimbada + `VL_SIM`, sem chamar `pergunta_sem_acao_possivel`; tem dois chamadores diretos, `fio` e `particionar_verbos`. `app/chamados/services/raiox.py:133`, `app/chamados/services/fio.py:386`, `app/chamados/services/verbos.py:31`.
- **Registro:** buscas não encontraram os nomes `pergunta_sem_acao_possivel`, `o_que_segura` ou `_pergunta_materializada_de_fato` no registro/mapa. O registro próximo é C6 → `pergunta_viva`, C4 → permissão contextual, C15 → `fio_vivo`. `app/core/juizes.py:236`, `app/core/juizes.py:238`, `app/core/juizes.py:247`; mapa aponta FIO/DISPUTA para `_disputa_do_chamado`, e permissão contextual para `pode_agir_chamado`. `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:46`. Ausência é resultado de busca [não verificado por referência positiva].
- **p#34375:** nenhuma ocorrência localizada na árvore; estado persistido do caso [não verificado].

## 30/10 22:33 · sessao 803483fec8e6 · - · - · origem=Ronald (tmux)

`o_que_segura` não devolve booleano “há ação admin”; devolve classificação `fio` e instrução `destrava` para cada dia retentor. O predicado está em `_fio_do_dia`, e a ordem exata é: se alguma pergunta **viva** da célula está `PENDENTE`, `colab`; senão, se existe alguma viva, `admin`; senão, se existe chamado com `status_local∈VIVOS`, `chamado`; senão, `nada`. Sem `celula_id`, o chamador força `nada`. Referências: `app/folha/fora_da_folha.py:59`, `:66`, `:67`, `:69`, `:71`, `:73`, `:204`, `:209`.

Os textos são: `admin` → “validar (ou recusar) a resposta ... na mesa de disputa”; `colab` → esperar resposta e depois validar; `chamado` → “resolver o chamado N”; `nada` → “SEM FIO ... este dia precisa de alguém para olhar”. Referências: `app/folha/fora_da_folha.py:38`–`:42`; para IDs reais do chamado e texto substituído, `:198`–`:210`.

Insumos e fontes:

- Recebe colaborador/mês/ano, obtém empresa e janela, lê `classificar_export` para informar entra/motivo e `_dias_que_retem` para universo dos dias. Referências: `app/folha/fora_da_folha.py:179`, `:188`–`:194`.
- `_dias_que_retem` delega a `motivos_retencao_celula(..., com_dias=True)`. Referência: `app/folha/fora_da_folha.py:90`–`:94`.
- Alimentação carrega **todas** as perguntas por célula e chamados com status `VIVOS`, sem filtro de disputa aberta/fechada, âncora viva/terminal, fila, destino ou validação na query. Referência: `app/folha/fora_da_folha.py:46`–`:56`.
- A pergunta é filtrada por `.viva`, que delega integralmente a `pergunta_viva`; `.estado` traduz validação/materialização/vitalidade/resposta em rótulo. Referências: `app/chamados/models.py:2358`–`:2369`; `:2197`–`:2211`.
- `validada_em != None` torna a pergunta morta no juiz; portanto pergunta validada não pode produzir `admin`/`colab` nesse caminho. Referência: `app/chamados/juizes.py:1005`–`:1007`.
- O retentor lê célula soberana: isento retorna vazio; ajusta admissão/demissão; sem célula em dia passado com vínculo retém; veredito acusador retém salvo todos protestos não cobráveis ou isenção de pausa; sem veredito, trabalha definido e dia passado retém. Referências: `app/folha/export.py:1174`, `:1180`–`:1196`, `:1206`–`:1238`.

**Contagem de chamadores estáticos de produção:** `o_que_segura`: **0**; seu juiz efetivo `_fio_do_dia`: **3 funções / 3 chamadas** — `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. Referências dos três: `app/folha/fora_da_folha.py:154`, `:204`; `app/ponto/services/pautas_dos_discordantes.py:75`. A ausência de chamadores de `o_que_segura` resulta da busca global, não tem linha de prova positiva; marcar `[não verificado]` se exigir referência documental estrita.

Cadeias de consumo:

1. `_fio_do_dia` → `fora_da_folha` (`app/folha/fora_da_folha.py:154`) → `montar_linhas` (`app/folha/retrato_folha.py:1157`) → comando `lavrar_retrato_folha` (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`) → tela que lê retratos gravados (`app/folha/views_retrato.py:246`–`:260`), rota `exportar/retrato/` (`app/folha/urls.py:23`). Drawer lê `DESTRAVA` literalmente via `o_que_fazer`, `app/folha/retrato_folha.py:1057`, `:1068`.
2. `_fio_do_dia` → `dias_sem_fio`, que escolhe células `discordante` e só aceita fio `nada` (`app/ponto/services/pautas_dos_discordantes.py:67`–`:78`) → `Command.apurar` (`app/ponto/management/commands/pautas_discordantes.py:56`, `:68`).
3. `vigia_fio` **não chama** `_fio_do_dia`: seu `fio_do_dia` delega a `fio_vivo` (`app/chamados/services/vigia_fio.py:155`–`:165`); essa redução é `any(pergunta_viva(p) ...)` (`app/chamados/juizes.py:1102`–`:1111`). A documentação reconhece a mesma autoridade `.viva` do outro leitor (`app/chamados/services/vigia_fio.py:129`–`:137`).

**Discrepância estrutural importante para a matriz:** `VIVOS=('aberto','em_analise','registrado')`, mas o catálogo declara que **`registrado` não pede ação**, não entra na fila/placar; `COBRANCA` contém só aberto/em_análise. Referência: `app/chamados/catalogo/motor.py:24`–`:29`. Assim, sem pergunta viva e com único chamado `registrado`, `_fio_do_dia` classifica `chamado` e oferece “resolver chamado”, embora o catálogo diga ausência de ação devida. Referências: `app/folha/fora_da_folha.py:71`–`:72`, `:41`.

A UI não pode ser confundida com porta viva: junta tanto `chamado` como `ninguem` no dono `admin`; isso é agrupamento explícito, não prova de uma ação endereçável. Referência: `app/folha/retrato_folha.py:189`–`:192`. A regra `dono_da_linha` ainda desvia `ninguem` para `hoje` quando todas as datas são hoje/futuras (`:474`–`:481`).

Nenhum dos nomes `o_que_segura`, `_fio_do_dia` ou `pergunta_sem_acao_possivel` está registrado diretamente nos dois catálogos pesquisados `[não verificado]`. O registro contém suas autoridades componentes: `_C1→VIVOS`, `_C6→pergunta_viva`, `_C15→fio_vivo`. Referências: `app/core/juizes.py:233`, `:238`, `:247`. O texto do registro distingue expressamente **chamado vivo** de **fio vivo**, para evitar autorizar `VIVOS` como resposta a “alguém cobra?”. Referência: `app/core/juizes.py:100`–`:106`. `app/docs/JUIZES.md:51`–`:54` ainda traz o lookup antigo “vivo/cobra = VIVOS/COBRANCA”; o próprio documento manda o registro vivo prevalecer (`:11`–`:15`).

Outra função realmente relevante pela **regra**, independente do nome: `resolver_dia_oferecido(dia, hoje)` exige `pode_ultimo_recurso && eh_trabalho && data<hoje && status∈('falta','irregular','aberto','sem_certidao')`; permissão de quem vê fica no template. Referências: `app/colaboradores/services/calendario.py:196`, `:199`–`:205`. `resolver_dia_oferecido_na_data` monta o calendário pelo mesmo caminho da tela e lê esse campo (`:208`–`:216`). Está registrada `_TT11` em `app/core/juizes.py:684`.

O vigia distingue esse clique real de mero status: `resolver_dia_oferecido_na_data` → `decisao_do_dia_pendente`, com comentário explícito de que `em_analise` sozinho não é ação nomeada (`app/chamados/services/vigia_fio.py:294`–`:300`); separa ausência aberta (`:301`–`:306`), FOLGA_CONTESTA validada (`:307`–`:313`) e fio do admin por presença **e dono BATIDA/CADASTRO** na lista (`:314`–`:318`, `:266`).

Lei exata: “CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga”; cron vigia só alarma, nunca juiz. Referências: `CLAUDE.md:275`–`:276`.

_795 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
