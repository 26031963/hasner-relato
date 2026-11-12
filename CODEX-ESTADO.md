# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 15:58** · publicado em **12/11/2026 15:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 15:58 · sessao 89593e35bd52 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura entregue para certificação: **29 testes locais verdes, 14 mutantes mortos por asserção e Ruff limpo nos cinco Python tocados**. HEAD continua na base, sem commit. ([testes](docs/lastro_c19_execucao.txt:1040), [mutantes](docs/lastro_c19_execucao.txt:1086), [Ruff](docs/lastro_c19_execucao.txt:1045), [HEAD](docs/lastro_c19_execucao.txt:1053))

| Item | RED que mordeu antes da cura |
|---|---|
| 1 | `test_reconciliacao_mesmo_minuto_remove_so_instante_alvo`: célula preenchida e órfã remanescente conferidas. [RED](docs/lastro_c19_execucao.txt:96) |
| 2 | `test_dryrun_nao_orfa_no_mesmo_minuto_nao_vira_intervalo`. Também há cobertura com matcher real. [RED](docs/lastro_c19_execucao.txt:159), [matcher real](ponto/tests/test_lastro_c19_orfas_instantes.py:121) |
| 3 | `test_contador_sem_chamador_foi_apagado`: contador removido com lápide de uma linha. [RED](docs/lastro_c19_execucao.txt:193), [lápide](ponto/services/cartorio.py:721) |
| 4 | `test_leitor_filtra_par_hora_instante_junto`: filtra o par; ata antiga conserva instantes vazios. [RED](docs/lastro_c19_execucao.txt:213), [cura](escala/services/leitor_celula.py:187) |
| 5 | **Procede.** `test_sem_marcos_publica_orfas_da_ata_e_pdf_declara_ignorancia`: adaptador conserva horas sem marcos; PDF mantém “não sei” para ata antiga. [RED](docs/lastro_c19_execucao.txt:268), [cura](escala/utils.py:1105) |

Comandos para repetir os testes e os cinco mutantes C19: [nao verificado]

```bash
c19() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B \
    -m ponto.tests.test_lastro_c19_orfas_instantes "$@" -v
}

c19 ReconciliacaoC19Test
c19 DryRunC19Test
c19 ContadorC19Test
c19 LeitorC19Test.test_leitor_filtra_par_hora_instante_junto
c19 LeitorC19Test.test_sem_marcos_publica_orfas_da_ata_e_pdf_declara_ignorancia

c19 --mutante reconciliacao_minuto ReconciliacaoC19Test
c19 --mutante dryrun_minuto DryRunC19Test
c19 --mutante contador_volta ContadorC19Test
c19 --mutante leitor_desalinhado LeitorC19Test
c19 --mutante sem_marcos_descarta LeitorC19Test
```

Os comandos executados antes de cada cura, os nove mutantes C18 restantes e o comando exato do Ruff estão no [registro de entrega](docs/lastro_c19_execucao.txt:1065).

Arquivos::funções alterados:

- `escala/utils.py::montar_grade_prevista_periodo_por_turno` — comparação por instante no [dry-run](escala/utils.py:1699) e na [reconciliação](escala/utils.py:2067).
- `escala/utils.py::montar_realizado_grade` — [órfãs independem dos marcos](escala/utils.py:1097).
- `escala/services/leitor_celula.py::_orfas_da_ata` e `::grade_da_celula` — [filtragem pareada](escala/services/leitor_celula.py:180), [transporte](escala/services/leitor_celula.py:424).
- `ponto/services/cartorio.py::contar_celulas_orfas_sem_instantes` — [apagada](ponto/services/cartorio.py:721).
- Testes C18 ajustados e C19 acrescentados: [lista de funções e remoções](docs/lastro_c19_execucao.txt:1095).

| Campos | Quem escreve | Quem lê |
|---|---|---|
| Grade `orfas` / `orfas_instantes` | Matcher, builders e reconciliação: [mapa completo](docs/lastro_c19_execucao.txt:1107) | [Cartório](ponto/services/cartorio.py:737), [adaptador](escala/utils.py:1105), [espelho](ponto/services/espelho.py:860) |
| Ata `orfas` / `orfas_instantes` | [ata_do_dia](ponto/services/cartorio.py:739); porta persiste | [leitor](escala/services/leitor_celula.py:187) |
| Grade lida | [grade_da_celula](escala/services/leitor_celula.py:427) | [adaptador/espelho](ponto/services/espelho.py:704) |
| `orfas_grade` / `orfas_instantes_grade` | [montar_dias](ponto/services/espelho.py:859) | [PDF](relatorios/pdf_espelho.py:176), [tela](templates/ponto/espelho.html:240) |

Certificação Code/Opus, dados de produção, relavratura, DIFF monetário e bytes do PDF: **[nao verificado]**.

## 12/11 15:44 · sessao 8296f491b2c5 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVADO para fechar o aval (A): o cartório passou a lavrar os instantes, mas sobraram identidades de órfã por hora, há remoção semanticamente errada na reconciliação e o contador não tem chamador de produção.** (`app/ponto/services/cartorio.py:769`; `app/escala/utils.py:1721`; `app/escala/utils.py:2097`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`)

1. **O delta realiza (A) e só ele?**

Realiza a cadeia central: grade conserva ISO → cartório lavra → leitor transporta → espelho publica → PDF compara timestamps completos. Nesse caminho, o PDF exige offset e conserva segundos, sem atribuir à órfã a data da linha. (`app/escala/utils.py:628`; `app/ponto/services/cartorio.py:769`; `app/escala/services/leitor_celula.py:422`; `app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:182`; `app/relatorios/pdf_espelho.py:500`)

**Entretanto, sobraram estas comparações/chaves:**

| Sítio | Resíduo |
|---|---|
| PDF | Constrói `_espurias_datadas` por `DD/MM HH:MM` e elimina anomalias por substring dessa chave. É decisão, além de formatação. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`) |
| Dry-run de intervalo | Chama `_match_marcos` sem coletor, converte órfãs em conjunto de HH:MM e seleciona batidas pela pertença nesse conjunto. (`app/escala/utils.py:1696`; `app/escala/utils.py:1697`; `app/escala/utils.py:1721`) |
| Reconciliação | Reconhece a borda por HH:MM, relaciona órfãs ao turno por horas e remove a primeira ocorrência daquela hora. (`app/escala/utils.py:2062`; `app/escala/utils.py:2085`; `app/escala/utils.py:2089`; `app/escala/utils.py:2097`) |

Portanto, a retirada do casamento central de c17 aconteceu, mas **não houve eliminação de toda identidade por `(data, HH:MM)` ou HH:MM nos sítios perguntados**. Os resíduos acima continuam executáveis. (`app/relatorios/pdf_espelho.py:702`; `app/escala/utils.py:1721`; `app/escala/utils.py:2097`)

A relavratura da competência 11, reversão ANTES, prova de frota DEPOIS, DIFF monetário zero e contador real zero **[nao verificado]**: o dado fornecido não contém esses resultados.

2. **ESMERIL-SEMÂNTICA: escritores, leitores e acompanhamento**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `dia['orfas_instantes']` | `_match_marcos` preenche coletores nas duas saídas; builders publicam e acrescentam sobras. (`app/escala/utils.py:628`; `app/escala/utils.py:671`; `app/escala/utils.py:799`; `app/escala/utils.py:811`; `app/escala/utils.py:1863`; `app/escala/utils.py:1888`) | Ordenação, reconciliação e `ata_do_dia`. (`app/escala/utils.py:815`; `app/escala/utils.py:1892`; `app/escala/utils.py:2099`; `app/ponto/services/cartorio.py:769`) |
| `ata['orfas_instantes']` | Produtor de conteúdo: `ata_do_dia`. A porta copia/persiste a ata recebida. (`app/ponto/services/cartorio.py:769`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1033`) | Contador e `grade_da_celula`. (`app/ponto/services/cartorio.py:745`; `app/escala/services/leitor_celula.py:422`) |
| `dia['orfas_instantes']` transportado | Leitor copia da ata; degradação pelo builder esvazia deliberadamente. (`app/escala/services/leitor_celula.py:422`; `app/ponto/services/espelho.py:503`) | Espelho publica como `orfas_instantes_grade`. (`app/ponto/services/espelho.py:860`) |
| `orfas_instantes_grade` | `montar_dias`. (`app/ponto/services/espelho.py:860`) | `_orfas_publicadas`, que produz instantes para classificação das batidas e entradas espúrias. (`app/relatorios/pdf_espelho.py:177`; `app/relatorios/pdf_espelho.py:195`; `app/relatorios/pdf_espelho.py:500`; `app/relatorios/pdf_espelho.py:590`) |
| `orfas_sem_instantes` | Coletor do PDF, quando o helper devolve `None`. (`app/relatorios/pdf_espelho.py:587`) | Guarda da marca `b.orfa` e observação textual. (`app/relatorios/pdf_espelho.py:590`; `app/relatorios/pdf_espelho.py:899`) |

O contrato preserva **instante absoluto com offset e segundos**: os timestamps entram por `timezone.localtime`, são serializados por `isoformat` e o PDF rejeita valores sem offset. A linha não determina a data do instante: há órfãs atribuídas ao turno e sobras destinadas pelo dia civil. (`app/escala/utils.py:692`; `app/escala/utils.py:1367`; `app/escala/utils.py:628`; `app/relatorios/pdf_espelho.py:186`; `app/escala/utils.py:807`; `app/escala/utils.py:1872`)

A ordenação é **por HH:MM, com pares preservados**, não por cronologia absoluta. Append, promoção de célula para órfã, clip, folga, ausência/justificativa e legacy acompanham o campo nos caminhos examinados. (`app/escala/utils.py:815`; `app/escala/utils.py:1770`; `app/escala/utils.py:1771`; `app/escala/utils.py:978`; `app/escala/utils.py:983`; `app/escala/utils.py:1173`; `app/escala/utils.py:1252`; `app/escala/utils.py:1296`; `app/escala/utils.py:1302`; `app/escala/utils.py:1062`)

**A reconciliação acompanha a posição, mas pode acompanhar a batida errada.** Contraexemplo executado em memória no laço real: órfãs `03:00:10` e `03:00:20`, borda alvo `03:00:20`. O `index('03:00')` remove `:10`; a célula recebe `:20`, que também permanece órfã. Comprimentos continuam iguais, portanto o contador não acusa. Batidas de tipos diferentes podem sobreviver ao dedupe nesse mesmo minuto. (`app/escala/utils.py:2097`; `app/escala/utils.py:2099`; `app/escala/utils.py:2101`; `app/ponto/services/cartorio.py:746`; `app/escala/utils.py:1378`)

Há ainda dois desacoplamentos:

- O leitor filtra horas falsas em `_orfas_da_ata`, mas copia instantes sem aplicar o mesmo filtro; atas malformadas podem perder o pareamento. Ocorrência em produção **[nao verificado]**. (`app/escala/services/leitor_celula.py:187`; `app/escala/services/leitor_celula.py:422`)
- A degradação mantém horas e apaga instantes de propósito: nesse campo, `[]` também representa ausência de lastro lavrado. (`app/ponto/services/espelho.py:502`; `app/ponto/services/espelho.py:503`)

3. **Segundo escritor ou lavra sem `ata_do_dia`**

Não encontrei segundo **produtor do conteúdo** `ata['orfas_instantes']`: ele nasce em `ata_do_dia`; os dois caminhos de lavra com ata chegam desse produtor à porta. A porta copia o conteúdo, inclusive quando prevalece o veredito administrativo. (`app/ponto/services/cartorio.py:769`; `app/ponto/services/cartorio.py:1280`; `app/ponto/services/cartorio.py:1321`; `app/ponto/services/cartorio.py:1880`; `app/ponto/services/cartorio.py:1882`; `app/ponto/portas/celula.py:1006`)

**Existe gravação de ata sem passar por ele:** `carimbar_precedencia` pode partir de ata vazia, adicionar `precedencia` e salvar. Isso é outro dono por chave, não outro produtor de instantes. Também existe ata parcial construída em cópia de memória pelo builder, sem instantes; aquele trecho não persiste a cópia. (`app/ponto/portas/celula.py:1083`; `app/ponto/portas/celula.py:1087`; `app/ponto/portas/celula.py:104`; `app/escala/utils.py:1943`; `app/escala/utils.py:1952`)

4. **Contador: chamadores e autoridades**

**L-111: sítio com zero chamador de produção.** No censo, os chamadores encontrados são testes; o comando de produção chama `julgar_colab`, sem incorporar esse contador. Assim, sua existência não constitui vigia operacional. (`app/ponto/services/cartorio.py:721`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:323`; `app/ponto/management/commands/processar_cartorio.py:71`; `app/docs/LEIS.md:137`)

As autoridades usadas são as corretas: empresa/janela delimitam as células; `janela_competencia` delega a `janela_fechamento`; K9 decide por colaborador, compondo exportação vigente e holerite de lote publicado. K9 está registrado. (`app/ponto/services/cartorio.py:728`; `app/ponto/services/cartorio.py:731`; `app/ponto/services/cartorio.py:736`; `app/ponto/services/cartorio.py:739`; `app/folha/export.py:51`; `app/folha/export.py:618`; `app/folha/export.py:624`; `app/core/juizes.py:337`)

Mas ele mede somente **células com `n_orfas>0` e lista ausente/de tamanho diferente**. `['invalido']` com `n_orfas=1` passa, embora o PDF rejeite; ordem, identidade e offset não são certificados pelo zero desse contador. (`app/ponto/services/cartorio.py:744`; `app/ponto/services/cartorio.py:746`; `app/relatorios/pdf_espelho.py:182`)

5. **Classes que já reprovaram**

- **Juiz/derivação paralela:** o PDF ainda decide supressão de anomalias pela chave textual reduzida; a reconciliação identifica órfã por minuto. São os resíduos concretos encontrados. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/escala/utils.py:2063`)
- **Segundo escritor:** não demonstrado para o conteúdo novo; há persistência de outra chave da ata pela porta, conforme item 3. (`app/ponto/services/cartorio.py:769`; `app/ponto/portas/celula.py:1084`)
- **Relógio:** os helpers novos usam dados recebidos; o PDF captura `agora` nas entradas públicas. Não identifiquei nova leitura fora da borda nesses trechos. (`app/relatorios/pdf_espelho.py:174`; `app/relatorios/pdf_espelho.py:1028`; `app/relatorios/pdf_espelho.py:1075`)
- **Travas:** não há aquisição nova de trava nos trechos acrescentados; não encontrei inversão atribuível a c18. Concorrência em execução **[nao verificado]**. (`app/ponto/services/cartorio.py:721`; `app/ponto/services/cartorio.py:769`; `app/relatorios/pdf_espelho.py:174`)
- **Campo com significados incompatíveis:** preservação de segundos no transporte não combina com a identificação por minuto usada para remover a órfã. (`app/escala/utils.py:628`; `app/escala/utils.py:2097`)

**Os REDs comportamentais mordem o retorno do B central.** Repondo em memória os dois assignments c17 do anexo, os cinco testes de PDF produziram oito falhas, contando subtests: noturno, ata antiga, segundos/offset e valores inválidos. Porém, o mutante B não está incorporado na lista, e o selo estrutural aceita explicitamente `(data, HH:MM)` e `DD/MM HH:MM`. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:124`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:138`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:145`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:153`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:373`; `app/relatorios/tests/test_pdf_identidade_datada.py:301`; `app/relatorios/tests/test_pdf_identidade_datada.py:302`)

O teste de reconciliação verifica só a remoção posicional: exclui o preenchimento da célula e não fornece o instante alvo. Não morde o contraexemplo do item 2. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:344`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:345`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:349`)

Execução nesta leitura, sem log criado: **15 testes C18 verdes; os dez mutantes incorporados falharam; sete testes de identidade datada verdes**. Esses ensaios não certificam banco nem bytes do PDF. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:1`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:373`; `app/relatorios/tests/test_pdf_identidade_datada.py:229`)

6. **Competência exportada**

**Existe caminho para mudar o gravado de exportada:** `processar_cartorio --apply --forcar` seleciona células passadas sem recorte por competência fechada; a lavra acrescenta a nova chave e uma ata diferente é persistida. O alcance do comando já existia, mas o conteúdo acrescentado por c18 passa por ele. (`app/ponto/management/commands/processar_cartorio.py:32`; `app/ponto/management/commands/processar_cartorio.py:34`; `app/ponto/management/commands/processar_cartorio.py:54`; `app/ponto/management/commands/processar_cartorio.py:71`; `app/ponto/services/cartorio.py:769`; `app/ponto/services/cartorio.py:1882`; `app/ponto/portas/celula.py:1033`)

Sem `forcar`, impressão igual continua pulando a célula; adicionar o campo não prova relavratura automática das atas antigas. Execução sobre exportadas, mudança efetiva de TXT/dinheiro ou DIFF zero **[nao verificado]**. (`app/ponto/services/cartorio.py:1438`; `app/ponto/services/cartorio.py:1442`)

O PDF diz “não sei”, sem casar por hora **na classificação central**, quando recebe horas órfãs e faltam instantes válidos. Mas isso não é universal: com marcos vazios, o adaptador descarta também as horas; o helper interpreta horas vazias como conjunto conhecido vazio e não acende o aviso. Ocorrência em produção **[nao verificado]**. (`app/escala/utils.py:1103`; `app/ponto/services/espelho.py:704`; `app/ponto/services/espelho.py:859`; `app/relatorios/pdf_espelho.py:178`; `app/relatorios/pdf_espelho.py:587`; `app/relatorios/pdf_espelho.py:899`)

7. **Leis contrariadas**

**L-111 é contrariada pelo contador sem chamador de produção.** A regra exige retirar o sítio sem chamador; a função permanece sustentada pelos testes. (`app/docs/LEIS.md:137`; `app/ponto/services/cartorio.py:721`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`)

**LEI-AKITA 2/L-038 seguem contrariadas no filtro próprio de anomalias do PDF:** ele deriva uma identidade reduzida para decidir o que suprimir. A exigência de RED também fica incompleta para a identidade da reconciliação, embora os REDs centrais tenham mordida. (`CLAUDE.md:22`; `app/docs/LEIS.md:86`; `app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `CLAUDE.md:30`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:345`)

O cumprimento integral do aval — sobretudo preservar exportadas e provar o zero monetário após relavratura da 11 — permanece **[nao verificado]**.

## 12/11 15:42 · sessao 0732f0ee3ea7 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **L-111: sítio com zero chamador de produção.** A função só é definida em `app/ponto/services/cartorio.py:721`; os chamadores encontrados estão nos testes (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`, `:323`). O contador não é incluído nos contadores do cartório (`app/ponto/services/cartorio.py:1983`) nem chamado pelo command (`app/ponto/management/commands/processar_cartorio.py:71`). A contagem real na competência aberta permanece **[nao verificado]**.
- As autoridades do contador são corretas: janela via `janela_competencia` (`app/ponto/services/cartorio.py:727`), que delega para `janela_fechamento` (`app/folha/export.py:47`); fechamento por pessoa via K9 (`app/ponto/services/cartorio.py:739`), que compõe `competencia_entregue` e holerite de lote publicado (`app/folha/export.py:617`). K6/K9 estão registrados em `app/core/juizes.py:327` e `:337`.
- **O contador só certifica formato de lista/tamanho contra `n_orfas`.** Não valida ISO, offset, segundos ou correspondência posicional (`app/ponto/services/cartorio.py:743`). Uma lista `['invalido']` com `n_orfas=1` conta zero, embora o PDF a rejeite (`app/relatorios/pdf_espelho.py:182`).

Escritores e caminhos da ata:

- Único produtor explícito do conteúdo `ata['orfas_instantes']`: `ata_do_dia`, `app/ponto/services/cartorio.py:769`.
- Lavra normal: `ata_do_dia` → `_ata` → `lavrar_veredito`, `app/ponto/services/cartorio.py:1880`, `:1882`. Ramo sem adesão: `:1280`, `:1321`.
- A porta copia e compara a ata (`app/ponto/portas/celula.py:995`), escreve mesmo quando o veredito humano prevalece (`:1006`) ou no ramo comum (`:1033`). Não deriva instantes.
- `celula_como_o_julgamento_deixa` só copia em memória (`app/ponto/portas/celula.py:942`, `:945`). O builder também fabrica uma ata parcial em cópia de memória, só com lâmpadas e horas, sem instantes (`app/escala/utils.py:1943`, `:1944`, `:1952`).
- **Existe gravação de ata sem passar por `ata_do_dia`:** `carimbar_precedencia` parte de `dict(cel.ata or {})`, adiciona a chave e salva (`app/ponto/portas/celula.py:1083`, `:1087`). É outro dono declarado por chave (`:104`), não segundo produtor de instantes.
- Admin (`app/ponto/services/veredito_celula.py:351`) e complemento de status (`app/ponto/services/cartorio.py:964`) chamam a lavra sem ata; nesse caso a porta não substitui a ata (`app/ponto/portas/celula.py:991`). Quando há ata, só `precedencia` é preservada da anterior (`:997`, `:104`).
- Não encontrei `_gravar_sem_fato` na cópia **[nao verificado quanto a outras versões]**.

Competência exportada:

- **Não afirmar impossibilidade de mudar o gravado.** O command aceita `--apply --forcar` (`app/ponto/management/commands/processar_cartorio.py:32`, `:34`), seleciona todas as células passadas (`:54`, `:66`) e encaminha `forcar` (`:71`). A lavra resultante acrescenta o campo novo (`app/ponto/services/cartorio.py:1880`) e a porta persiste uma ata diferente (`app/ponto/portas/celula.py:1000`, `:1033`). Não há guarda K9 nesse caminho; o uso K9 no cartório está no contador (`app/ponto/services/cartorio.py:727`).
- Esse alcance do command já existe; o efeito acrescentado por C18 é a nova chave na ata lavrada (`app/ponto/services/cartorio.py:769`). Execução sobre exportadas, TXT ou dinheiro em produção: **[nao verificado]**.
- O patch não altera a versão declarada (`app/ponto/services/cartorio.py:132`). Células com impressão igual continuam puladas sem `forcar` (`:1438`, `:1442`; ramo sem adesão `:1289`, `:1293`), portanto o campo novo não prova relavratura automática de atas antigas.
- Para o PDF, o fallback descarta deliberadamente os instantes reconstruídos pelo builder (`app/ponto/services/espelho.py:502`), inclusive o caminho de ata agregada (`:547`); a publicação da tela apenas copia o campo (`:860`).

## 12/11 15:42 · sessao 35878f3eb069 · - · - · origem=Ronald (tmux)

Achados prioritários:

- **Sobrou identidade de órfã por HH:MM no dry-run de intervalo:** `_match_marcos` é chamado sem coletor de instantes; `_orf2` vira `set` de horas e seleciona batidas com `strftime('%H:%M') in _orfset`. Portanto, o c18 não eliminou todas essas comparações de `escala/utils.py`. (`app/escala/utils.py:1696`, `app/escala/utils.py:1697`, `app/escala/utils.py:1721`)
- **Sobrou identidade por HH:MM na reconciliação:** testa presença da borda pela hora; relaciona órfãs e células ao turno por conjuntos/comparações de horas; remove a primeira ocorrência dessa hora. (`app/escala/utils.py:2062`, `app/escala/utils.py:2063`, `app/escala/utils.py:2085`, `app/escala/utils.py:2089`, `app/escala/utils.py:2091`, `app/escala/utils.py:2097`)
- **Bug semântico reproduzido em memória executando o laço AST real:** duas órfãs `03:00:10` e `03:00:20`, alvo da reconciliação `03:00:20`: `index('03:00')` remove a órfã de `:10`; a célula passa a conter `:20`, que também permanece em `orfas_instantes`. As listas continuam com tamanhos e posições pareados, mas apontam para a batida errada. O mecanismo é `index` por HH:MM seguido de `pop` posicional e preenchimento pelo instante alvo. (`app/escala/utils.py:2097`, `app/escala/utils.py:2099`, `app/escala/utils.py:2100`, `app/escala/utils.py:2101`)
- O dedupe não impede necessariamente esse caso: só elimina proximidade quando o tipo é igual; batidas E/S distintas podem sobreviver no mesmo minuto. (`app/escala/utils.py:1376`, `app/escala/utils.py:1378`)
- **O RED da reconciliação não testa essa identidade:** recorta somente desde `_ix_orfa`, exclui o preenchimento da célula e não fornece o instante alvo; valida a remoção da primeira ocorrência. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:344`, `app/ponto/tests/test_lastro_c18_orfas_instantes.py:345`, `app/ponto/tests/test_lastro_c18_orfas_instantes.py:349`, `app/ponto/tests/test_lastro_c18_orfas_instantes.py:351`)

Censo e significado:

| Operação | Arquivo:linha |
|---|---|
| Origem dos timestamps em fuso local | `app/escala/utils.py:692`, `app/escala/utils.py:1367` |
| Célula conserva ISO completo | `app/escala/utils.py:328` |
| `_match_marcos` escreve coletor de instantes, nas duas saídas | `app/escala/utils.py:628`, `app/escala/utils.py:671` |
| Builders inicializam coletores | `app/escala/utils.py:785`, `app/escala/utils.py:1747` |
| Builders publicam horas e instantes | `app/escala/utils.py:799`, `app/escala/utils.py:1862`, `app/escala/utils.py:1863` |
| Célula promovida a órfã leva instante | `app/escala/utils.py:1770`, `app/escala/utils.py:1771`, `app/escala/utils.py:1782`, `app/escala/utils.py:1783` |
| Sobras são destinadas pelo dia civil | `app/escala/utils.py:807`, `app/escala/utils.py:1872` |
| Sobras acrescentam as duas listas | `app/escala/utils.py:810`, `app/escala/utils.py:811`, `app/escala/utils.py:1884`, `app/escala/utils.py:1885`, `app/escala/utils.py:1887`, `app/escala/utils.py:1888` |
| Sort conserva pares, ordenando por HH:MM | `app/escala/utils.py:815`, `app/escala/utils.py:816`, `app/escala/utils.py:817`, `app/escala/utils.py:1892`, `app/escala/utils.py:1893`, `app/escala/utils.py:1894` |
| Clip limpa ambas | `app/escala/utils.py:977`, `app/escala/utils.py:978`, `app/escala/utils.py:982`, `app/escala/utils.py:983` |
| Folga limpa ambas | `app/escala/utils.py:1172`, `app/escala/utils.py:1173` |
| Ausência/justificativa limpam ambas | `app/escala/utils.py:1251`, `app/escala/utils.py:1252`, `app/escala/utils.py:1295`, `app/escala/utils.py:1296`, `app/escala/utils.py:1301`, `app/escala/utils.py:1302` |
| Legacy conserva o campo pelo espalhamento de `sd` | `app/escala/utils.py:1062` |
| Leitor inicializa lista vazia | `app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:391` |
| Leitor lê ata e escreve dia | `app/escala/services/leitor_celula.py:421`, `app/escala/services/leitor_celula.py:422` |

Os instantes preservam segundos/offset pelo `isoformat`; a ordem das listas é pareada, mas não necessariamente cronológica absoluta: o sort usa a hora exibida. A linha pode conter órfã do turno e sobra destinada pelo dia civil, portanto sua data não substitui a data do instante. (`app/escala/utils.py:628`, `app/escala/utils.py:671`, `app/escala/utils.py:807`, `app/escala/utils.py:815`, `app/escala/utils.py:1872`)

Duas ressalvas adicionais:

- `_orfas_da_ata` filtra valores falsos das horas, enquanto `orfas_instantes` é copiado independentemente. Uma ata malformada pode perder o pareamento; existência desse dado em produção: [nao verificado]. (`app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:422`)
- Há reconstrução de **ata temporária em memória**, contendo `lampadas` e `orfas`, sem instantes, para alimentar o juiz de turnos; não é gravação persistente nesse trecho. (`app/escala/utils.py:1943`, `app/escala/utils.py:1944`, `app/escala/utils.py:1949`, `app/escala/utils.py:1952`, `app/escala/utils.py:1955`)

## 12/11 15:42 · sessao b2616b5b9103 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **Sobrou casamento por data + HH:MM no PDF.** `_espurias_datadas` é um conjunto `strftime('%d/%m %H:%M')` das batidas marcadas órfãs, e `_anom` elimina anomalias por substring dessa chave. Perde ano e segundos; não é apenas apresentação. `app/relatorios/pdf_espelho.py:698-702`. O selo aceita explicitamente essa classe de chave: `app/relatorios/tests/test_pdf_identidade_datada.py:301-302`.

2. **O caminho central C18 deixou de derivar a data da linha.** `_orfas_publicadas` interpreta os ISO, exige offset e comprimento igual às horas; o coletor casa `b.timestamp` com esses instantes e a marca viaja para o desenho. `app/relatorios/pdf_espelho.py:174-188`, `:494-500`, `:587-590`, `:944-947`. O espelho transporta o campo em `app/ponto/services/espelho.py:860` e apaga instantes do builder sem lavra em `:502-503`.

3. **“Ata antiga diz não sei” é condicional às horas publicadas.** O helper retorna conjunto vazio imediatamente quando `orfas_grade` está vazia, sem conferir ausência dos instantes. `app/relatorios/pdf_espelho.py:178-180`. Há caminho de código que esvazia horas: adaptador com `marcos=[]` devolve `[], []`, `app/escala/utils.py:1103-1104`; espelho publica esse resultado em `app/ponto/services/espelho.py:704-707`, `:859`. Sem escala elegível nem instala o adaptador, `:487-491`. Nesse caminho o flag fica falso, `app/relatorios/pdf_espelho.py:587`, e não entra o aviso, `:899-900`. Ocorrência em produção: **[nao verificado]**. Os testes da antiga/degradação fornecem horas explicitamente: `app/ponto/tests/test_lastro_c18_orfas_instantes.py:118-143`, `:353-368`.

4. **Os REDs comportamentais mordem o retorno do B central.** Executei os cinco `PdfC18Test` com substituição AST em memória dos dois assignments pelos trechos c17 do anexo: oito falhas contando subtests, zero erros. Mordem o noturno (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:129`), ata antiga (`:142`), segundos/offset (`:151`) e inválidos (`:161`). O caso D/D+1 (`:131-136`) passa B porque B já diferencia a data. O mutante B não está na lista incorporada, `:373`, e o selo estrutural permite data + HH:MM, `app/relatorios/tests/test_pdf_identidade_datada.py:301`.

5. **Ensaios atuais executados sem escrita:** C18: 15 testes OK; identidade datada: sete testes OK. Escopo declarado sem banco/rede/Django/PDF bytes: `app/ponto/tests/test_lastro_c18_orfas_instantes.py:1-4`; limitações do censo AST: `app/relatorios/tests/test_pdf_identidade_datada.py:78-84`. A suite verde não elimina o resíduo de `pdf_espelho.py:698-702`.

6. **Relógio do PDF permanece na borda pública:** captura e repassa `agora` em `app/relatorios/pdf_espelho.py:1028-1030` e `:1075`; helper novo usa apenas dados recebidos, `:174-188`. Não identifiquei nova leitura de relógio nos trechos C18 desses dois arquivos.

## 12/11 15:38 · sessao 876dbbcd439a · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

C18 implementada; censo, REDs, comandos e limites estão no [relatório](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt:1).

- **86 testes sem banco passaram; 14 mutantes morderam.** Contador: `0`; retirando o instante do escritor: `1`. [Evidências](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt:5) [nao verificado]
- Suite Django e Ruff ficaram bloqueados por dependências ausentes; entrega sem certificação. [Limites](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt:8) [nao verificado]
- Sem commit ou relavra, conforme restrições; frota e DIFF financeiro **[nao verificado]**. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt) [nao verificado]

## 12/11 15:37 · sessao 83ad142c7d01 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · sonda · origem=Code

Reproduzo a saída raspada fornecida. Nova execução, código e predicados exatos de `sonda_numeros.py`: **[nao verificado]**.

```text
sonda_leitura: sonda_numeros.py [919e00d77781d52f] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

hoje 2026-11-12
futuras_total 8721
futuras_por_origem [('gerada',)] {'gerada': 8721, 'editada': 0}
D1_futura_com_julgamento 0 colabs 0
D1_regenerada_apos_julgada 0
D3_futura_gerada_sem_geradora 0 colabs 0 trabalha_true 0
D7_vinculo_inativo_com_fim_futuro 2 colabs 2
  D7 ec#489 col574 ini=2024-01-01 fim=2027-07-06 celulas_futuras_dela=236
  D7 ec#955 col203 ini=2026-07-21 fim=2026-12-31 celulas_futuras_dela=0
total_erros 0
```

**`total_erros = 0`**, conforme o dado fornecido; conferência independente: **[nao verificado]**. Os zeros reportados para D1/D3 não demonstram impossibilidade desses estados nem medem lacunas por pessoa/data em D7. **[nao verificado quanto à abrangência da sonda]**

**D1 — caminho possível no código**

- A entrada unitária `julgar_celula` recusa `data > hoje`, retornando `None`. (`app/ponto/services/cartorio.py:2016`; `app/ponto/services/cartorio.py:2025`)
- A entrada de lote `julgar_colab` recebe `cels` do chamador e repassa a lista ao corpo sem filtrar futuras. (`app/ponto/services/cartorio.py:940`; `app/ponto/services/cartorio.py:996`)
- Um caminho concreto é o ramo `adesao is None`: percorre as células recebidas; com `apply_=True`, `completar_status=False` e vencido o pulo de idempotência — por exemplo, `forcar=True` — chama `lavrar_veredito(..., save=False, ata=_ata_nb)` e depois `carimbar(..., lavrados=_cv)`. (`app/ponto/services/cartorio.py:1211`; `app/ponto/services/cartorio.py:1261`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1268`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1314`)
- As duas operações da porta não validam `cel.data`: a lavra atribui ata/veredito quando aplicável; o carimbo atribui `julgada_em=agora` e persiste também os campos lavrados. Ata preenchida nesse ramo depende de `_ata_nb` não nula e diferente da atual. A chamada direta dessas operações também permite essa sequência. (`app/ponto/portas/celula.py:991`; `app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1039`; `app/ponto/portas/celula.py:1065`; `app/ponto/portas/celula.py:1070`)

Isso estabelece possibilidade estática. Um chamador de produção ter entregue uma futura ao lote, ou executado diretamente essa sequência: **[nao verificado]**.

A regeneração pode conservar julgamento previamente preenchido: seu save altera desenho e marcas de regeneração, sem ata/veredito/julgada_em; o rejulgamento pula futuras. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:276`; `app/ponto/portas/celula.py:431`)

**D7 — ausência em dia coberto**

O dado informa 236 futuras “dela” para `ec#489/col574` e zero para `ec#955/col203`. Isso **não comprova célula futura AUSENTE para nenhum dos dois**: não foram fornecidas datas nem o predicado desse contador. **[nao verificado]**

A identidade da célula é **colaborador/data**, não vínculo/data. Portanto, zero atribuídas à EC pode coexistir com células da pessoa cuja geradora seja outra EC ou nula; 236 também não certifica cobertura completa das datas. (`app/escala/models.py:1692`; `app/escala/models.py:1833`) Quais dessas situações existem para os IDs listados: **[nao verificado]**.

A possibilidade estrutural de D7 existe: o gerador seleciona apenas `ativa=True`, embora a autoridade admita vínculo inativo com fim declarado e cobertura inclusiva entre início e fim. (`app/ponto/portas/celula.py:829`; `app/ponto/portas/celula.py:849`; `app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:214`) Para afirmar ausência real, falta verificar existência de célula por pessoa/data em cada dia futuro coberto; essa medição não aparece na saída. **[nao verificado]**

Há divergências materiais entre matriz e cópia: nesta árvore, o gerador reescreve futuras existentes quando necessário, e o ramo fora de vigência salva somente `trabalha/dna`, conservando a FK. Assim, as descrições de conservação pelo cron e de FK anulada em D3 não correspondem a esses trechos. (`app/ponto/portas/celula.py:870`; `app/ponto/portas/celula.py:876`; `app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:245`)

## 12/11 15:36 · sessao af02dd476f08 · - · - · origem=Ronald (tmux)

D1 — caminho estático mínimo:

- A entrada unitária `julgar_celula` obtém `hoje = timezone.localdate()` e retorna `None` para `data > hoje`; portanto essa entrada não julga uma célula futura. (`app/ponto/services/cartorio.py:2016`; `app/ponto/services/cartorio.py:2025`)
- A entrada de lote `julgar_colab(c, cels, hoje=..., agora=..., apply_=...)` recebe a lista do chamador e a repassa ao corpo sem filtrar futuras. (`app/ponto/services/cartorio.py:940`; `app/ponto/services/cartorio.py:983`; `app/ponto/services/cartorio.py:996`)
- No corpo, havendo **`adesao is None`**, percorre todas as células recebidas. Com **`apply_=True`**, **`completar_status=False`** e sem o pulo por impressão idêntica (ou com `forcar=True`), chama `cel.lavrar_veredito(..., save=False, ata=_ata_nb)` e depois `carimbar(..., lavrados=_cv)`. Esse é um caminho conjunto que pode alcançar uma futura indevidamente incluída pelo chamador. (`app/ponto/services/cartorio.py:1211`; `app/ponto/services/cartorio.py:1261`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1268`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1314`)
- `lavrar_veredito` não verifica a data: quando há alteração, atribui `ata`, `veredito`, `veredito_via`, `veredito_em`; `save=False` deixa os campos para a operação seguinte salvar. Ata preenchida depende de `_ata_nb` não nula e diferente da atual. (`app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:991`; `app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1039`)
- `carimbar` também não verifica a data: atribui `julgada_em=agora` e salva junto os campos lavrados; o save é efetivado por `salvar_e_emitir`. Assim, chamada direta dessas operações também permite preencher uma futura, respeitadas suas outras guardas e condições de alteração. (`app/ponto/portas/celula.py:1052`; `app/ponto/portas/celula.py:1061`; `app/ponto/portas/celula.py:1065`; `app/ponto/portas/celula.py:1070`; `app/ponto/portas/celula.py:905`)

Isso demonstra possibilidade estática; chamador de produção fornecendo uma futura, execução desse caminho e existência de futura carimbada em produção: **[nao verificado]**.

A regeneração reescreve trabalho/FK/DNA e marcas de regeneração, sem incluir ata/veredito/julgada_em no save; o rejulgamento pula futuras. Portanto pode **conservar** um julgamento previamente preenchido sem o produzir por esse ato. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:276`; `app/ponto/portas/celula.py:431`)

A matriz anexa diverge da cópia no ramo fora de vigência: aqui ele só grava `trabalha` e `dna`, conservando a geradora; não grava FK nula. (`app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:245`)

## 12/11 15:36 · sessao a5e0c3951f1e · - · - · origem=Ronald (tmux)

D7 não comprova célula ausente para nenhum dos dois vínculos. O dado informa apenas `ec#489 col574 celulas_futuras_dela=236` e `ec#955 col203 celulas_futuras_dela=0`; o predicado exato desse contador não foi verificado, pois o código de `sonda_numeros.py` não está disponível. [nao verificado]

A célula pertence à identidade **colaborador/data**, com unicidade definida em `app/escala/models.py:1833`. Portanto, zero células atribuídas à EC não demonstra zero células da pessoa nas mesmas datas: elas podem existir com outra geradora ou geradora nula; quais existem para esses IDs permanece [nao verificado].

Na cópia, o gerador seleciona somente ECs `ativa=True` (`app/ponto/portas/celula.py:829`), depois exige cobertura (`app/ponto/portas/celula.py:849`), consulta células por pessoa/data e cria ausentes (`app/ponto/portas/celula.py:840`, `app/ponto/portas/celula.py:852`, `app/ponto/portas/celula.py:853`). A autoridade admite EC inativa com fim declarado e considera início/fim inclusivos (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`). Assim, a exclusão dessas ECs pelo gerador é demonstrável; ocorrência concreta de ausência por dia não foi medida no stdout fornecido. [nao verificado quanto à ocorrência]

O comando usa por padrão a união das competências correntes das empresas ativas (`app/ponto/management/commands/gerar_celulas.py:290`, `app/ponto/management/commands/gerar_celulas.py:291`) e chama essa porta (`app/ponto/management/commands/gerar_celulas.py:310`). O stdout não fornece datas ausentes, células de outras geradoras, janela efetivamente examinada para D7 ou escolha de autoridade por dia. [nao verificado]

**Divergência material do anexo:** `cadastro_futuro.py` e `vigia_cadastro_futuro.py` não apareceram na cópia pesquisada; atribuir-lhes comportamento nesta árvore fica [nao verificado]. Além disso, o gerador desta cópia reescreve células existentes de hoje/futuro quando desenho/geradora mudam (`app/ponto/portas/celula.py:870`, `app/ponto/portas/celula.py:872`, `app/ponto/portas/celula.py:876`), diferentemente da conservação descrita na matriz.

## 12/11 15:32 · sessao d123cf6bebfd · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · leitura · origem=Code

A cópia contém curas na origem, adaptações locais e transições ainda abertas. A candidata **(b)** está implementada; isso não basta para fechar os estados “futura carimbada”, “reativada por `update`” e “passada atingida por exclusão da geradora”. (`app/ponto/services/vigia_cadastro_futuro.py:31`; `app/colaboradores/services/vinculo.py:716`; `app/ponto/portas/celula.py:453`; `app/colaboradores/services/vinculo.py:1007`)

A comparação histórica exata `0a747193e..a5a92544a` e o resultado da suíte em c4b ficam **[nao verificado]**: a cópia disponível não trouxe histórico Git nem esse diff. Abaixo separo comportamento verificável nesta árvore de atribuição histórica e dado de produção. **[nao verificado]**

**1. Matriz de transições**

“Gerada/editada” é `origem`; “regenerada” acrescenta marcas de reescrita; “carimbada” acrescenta julgamento. Portanto, as cinco colunas pedidas não são estados exclusivos: uma célula regenerada pode continuar gerada e carimbada. A proteção de `origem='editada'` prevalece sobre as demais características. (`app/escala/models.py:1690`; `app/escala/models.py:1718`; `app/escala/models.py:1745`; `app/ponto/portas/celula.py:285`)

Nas matrizes, cada casa tem **ação atual / exigência / conclusão**. Os símbolos abaixo incorporam o comportamento e as referências indicadas; `=` significa concordância naquele recorte, `D` aponta divergência ou transição incompleta, e `?` significa que falta estabelecer a aplicação da lei. As exigências específicas dos avais transcritos são tomadas do dado fornecido, sem confirmação dos respectivos arquivos locais. **[nao verificado]**

| Símbolo | Expansão verificável usada nas casas |
|---|---|
| **I** | Conserva a célula: suplemento seleciona somente futuras existentes `origem='gerada'`; não cria ausente nem seleciona editada. (`app/ponto/services/cadastro_futuro.py:18`; `app/ponto/services/cadastro_futuro.py:22`) |
| **R** | Em dia coberto, compara `trabalha`, geradora e DNA; se mudou, reescreve esses campos e grava `dna_anterior`, `regenerada_em`, `regeneracoes`; repetição igual não escreve. (`app/ponto/portas/celula.py:277`; `app/ponto/portas/celula.py:285`; `app/ponto/portas/celula.py:293`) |
| **K** | Executa **R**, refaz inclusive `dna.lampadas`, mas conserva ata, veredito, impressão e carimbo anteriores; julgamento de futura é pulado. (`app/ponto/management/commands/gerar_celulas.py:94`; `app/ponto/portas/celula.py:298`; `app/ponto/portas/celula.py:453`) |
| **Z** | Fora da vigência, sendo gerada pela EC encerrada, sem lavra e sem outro vínculo cobrindo: põe `trabalha=False`, geradora nula, DNA sem marcos e marcas de regeneração. (`app/ponto/portas/celula.py:244`; `app/ponto/portas/celula.py:249`) |
| **N** | Regeneração direta cria a célula ausente em dia coberto. (`app/ponto/portas/celula.py:280`) |
| **B** | EC inativa sem fim: regeneração recusa e conserva células; vigia usa `vinculo_do_dia` e separa o caso em C6. (`app/ponto/portas/celula.py:146`; `app/ponto/services/vigia_cadastro_futuro.py:31`; `app/ponto/services/vigia_cadastro_futuro.py:40`) |
| **G** | Gerador cria ausente; conserva futura existente e editada; no passado pode preencher DNA nulo. (`app/ponto/portas/celula.py:875`; `app/ponto/portas/celula.py:881`; `app/ponto/portas/celula.py:883`; `app/ponto/portas/celula.py:892`) |

O suplemento usa **J = união das competências correntes das empresas ativas**, limitada inferiormente por amanhã e por `desde`; chama a regeneração para cada data existente, com `estender_horizonte=False`. Isso é a janela do gerador, não uma janela calculada individualmente por empresa dentro do suplemento. (`app/ponto/management/commands/gerar_celulas.py:281`; `app/ponto/services/cadastro_futuro.py:16`; `app/ponto/services/cadastro_futuro.py:30`)

| Evento e entrada efetiva | Futura não existe | Futura gerada | Futura editada | Futura carimbada, origem gerada | Futura regenerada |
|---|---|---|---|---|---|
| **Salvar estrutura: município do posto**. Evento somente se mudou município; percorre vínculos do posto. (`app/colaboradores/services/estrutura.py:53`; `app/colaboradores/services/vinculo.py:931`) | I / I / = | R / R / =, ressalva **D2** | I / I / = | K / coerência / **D1** | R / R / =, ressalva **D2** |
| **Salvar demais campos de estrutura**: essas operações apenas salvam; não chamam o suplemento. (`app/colaboradores/services/estrutura.py:18`; `app/colaboradores/services/estrutura.py:36`; `app/colaboradores/services/estrutura.py:75`; `app/colaboradores/services/estrutura.py:82`) | I / depende do efeito / ? | I / R se muda previsto / ? | I / I / = | I / depende do efeito / ? | I / R se muda previsto / ? |
| **Salvar template pela porta comum**: futuro percorre todos os vínculos do tipo, inclusive encerrados. (`app/escala/services/cadastro_tipo.py:368`) | I / I / = | R / R / =, **D2** | I / I / = | K / coerência / **D1** | R / R / =, **D2** |
| **Salvar template pelo admin**: salva e propaga somente futuras no mesmo atomic. (`app/escala/admin.py:11`; `app/escala/services/cadastro_tipo.py:380`) | I / I / = | R / R / =, **D2** | I / I / = | K / coerência / **D1** | R / R / =, **D2** |
| **Folga lançada/alterada/foto relavrada**: passado pela regeneração; futuro pelo suplemento. (`app/escala/folgas.py:247`; `app/escala/folgas.py:290`; `app/escala/folgas.py:298`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Salvar calendário / trocar calendário do vínculo**: sincroniza foto futura e depois células existentes. (`app/escala/folgas.py:597`; `app/escala/folgas.py:619`; `app/escala/folgas.py:638`; `app/colaboradores/services/vinculo.py:455`) | I / I / = | R / R / = | I na célula / I / = | K / coerência / **D1** | R / R / = |
| **Vínculo novo pelo executor**: cria EC e suplementa desde início. (`app/colaboradores/services/vinculo.py:281`; `app/colaboradores/services/vinculo.py:330`) | I / I / = | R pelo novo / R / = | I / I / = | K pelo novo / coerência / **D1** | R pelo novo / R / = |
| **Encerrar com sucessor no executor**: fecha anterior em D−1 e depois aplica novo desde D. (`app/colaboradores/services/vinculo.py:205`; `app/colaboradores/services/vinculo.py:333`) | I / I / = | Z transitório ou conserva; termina R / R / = | I / I / = | termina K / coerência / **D1** | termina R / R / = |
| **Encerrar sem sucessor por `fechar_vigencia`**: começa após menor fim antigo/novo. (`app/colaboradores/services/vinculo.py:640`; `app/colaboradores/services/vinculo.py:648`) | I / I / = | Z / sem previsão inválida / **D3** | I / I / = | Z, ata conservada / coerência / **D1+D3** | Z / sem previsão inválida / **D3** |
| **Encerrar por `encerrar_vinculo`**: `save` dispara signal de vigência. (`app/colaboradores/services/vinculo.py:821`; `app/escala/signals.py:80`) | I / I / = | Z sem outro cobrindo; conserva havendo outro / evento deve fechar efeito / **D4** | I / I / = | mesma ação + ata antiga / coerência / **D1+D4** | mesma regra / evento deve fechar efeito / **D4** |
| **Reativar pelo executor**: mesma EC recebe ativa, fim nulo e suplemento. (`app/colaboradores/services/vinculo.py:253`; `app/colaboradores/services/vinculo.py:270`; `app/colaboradores/services/vinculo.py:333`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Reabrir vigência impossível**: `update`, sem suplemento. (`app/colaboradores/services/vinculo.py:715`; `app/colaboradores/services/vinculo.py:730`) | I / I / = | I / R / **D5** | I / I / = | I / atualizar efeito / **D5** | I / R / **D5** |
| **Trocar tipo/configuração pelo executor ou salvar template**: segue as respectivas linhas acima; âncora alterada para valor não nulo tem signal próprio. (`app/colaboradores/services/vinculo.py:268`; `app/escala/signals.py:111`; `app/escala/signals.py:131`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Trabalha em feriado**: flag mudou → suplemento; valor igual retorna. (`app/colaboradores/services/vinculo.py:869`; `app/colaboradores/services/vinculo.py:888`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Feriado salvo/movido**: datas antiga/nova; somente geradas; EC vem do juiz alimentado pela geradora. (`app/core/signals.py:14`; `app/core/signals.py:27`; `app/ponto/services/cadastro_futuro.py:53`; `app/ponto/services/cadastro_futuro.py:59`) | I / I / = | R se juiz responde; B no caso C6 / mesma autoridade / = | I / I / = | K se juiz responde / coerência / **D1** | R ou B / mesma autoridade / = |
| **Feriado apagado**: mesma seleção, na data apagada. (`app/core/signals.py:30`; `app/ponto/services/cadastro_futuro.py:49`) | I / I / = | R ou B / mesma autoridade / = | I / I / = | K / coerência / **D1** | R ou B / mesma autoridade / = |
| **DnaContrato**: cenário composto de encerramento, sucessor e edição de template; não é porta de cadastro autônoma. (`app/escala/tests/test_celula_dia.py:146`; `app/escala/tests/test_celula_dia.py:152`; `app/escala/tests/test_celula_dia.py:165`; `app/escala/tests/test_celula_dia.py:186`) | segue portas acima | segue portas acima | segue portas acima | cenário não prova esta coluna / exigência pendente / **D1** | segue portas acima |
| **`regenerar_celulas_vinculo` direta**: intervalo explícito; extensão ligada por padrão. (`app/ponto/portas/celula.py:110`; `app/ponto/portas/celula.py:187`) | N / suplemento deveria I / **D6 se usada como suplemento** | R ou Z ou B / conforme ato / ressalvas acima | I / I / = | K / coerência / **D1** | R ou Z ou B / conforme ato / ressalvas acima |
| **Cron `gerar_celulas`**: mede antes, trata hoje separadamente, gera ausentes e conserva futuras existentes. (`app/ponto/management/commands/gerar_celulas.py:337`; `app/ponto/management/commands/gerar_celulas.py:340`) | G só EC ativa / gerar ausentes vigentes / **D7** | I + vigia / I + vigia / = | I / I / = | I + vigia de DNA / vigiar coerência / **D1** | I + vigia / I + vigia / = |

As ressalvas da matriz são estas:

- **D1 — desenho novo com julgamento antigo.** `dna.lampadas` é previsão; `ata.lampadas` é o fato lavrado. A regeneração futura muda o primeiro e conserva o segundo. O leitor usa as lâmpadas da ata, e há consumidor que casa marco do DNA com luz da ata. O vigia compara `trabalha`/DNA, sem conferir essa coerência. (`app/ponto/portas/celula.py:298`; `app/ponto/portas/celula.py:453`; `app/escala/services/leitor_celula.py:114`; `app/chamados/juizes.py:1337`; `app/ponto/services/vigia_cadastro_futuro.py:48`)
- **D2 — evento de atributo pode substituir o dono do dia.** O suplemento seleciona todas as futuras geradas da pessoa, sem restringir geradora; cada EC que cobre a data pode substituir o carimbo. Na propagação do template, vínculos são percorridos por início decrescente; havendo sobreposição do mesmo template, o mais antigo pode escrever por último, embora `vinculo_do_dia` preservasse o carimbo recebido. É um contraexemplo estático; ocorrência em produção **[nao verificado]**. (`app/ponto/services/cadastro_futuro.py:22`; `app/ponto/services/cadastro_futuro.py:26`; `app/escala/services/cadastro_tipo.py:373`; `app/escala/models.py:1003`; `app/ponto/portas/celula.py:294`; `app/escala/alimentacao.py:279`)
- **D3 — fim sem sucessor ganhou representação, mas zero não comprova sua semântica.** A FK fica nula; `dna.vinculo_id` continua sendo a EC encerrada; o vigia põe o caso em `sem_geradora`, fora dos dois contadores esperado-zero. O teste exige essa representação, mas o aval transcrito não a especifica literalmente. (`app/ponto/portas/celula.py:250`; `app/ponto/management/commands/gerar_celulas.py:97`; `app/ponto/services/vigia_cadastro_futuro.py:34`; `app/ponto/tests/test_cadastro_futuro_c4.py:60`) **lei: representação e leitura da célula futura existente cujo vínculo terminou sem sucessor. [nao verificado]**
- **D4 — encerrar não equivale sempre a aplicar sucessor.** Havendo outro vínculo cobrindo, a porta conserva a célula fora da vigência da geradora antiga; só o evento que efetivamente aplica o sucessor troca o carimbo. Encerramento isolado não completa essa transição. (`app/ponto/portas/celula.py:247`; `app/colaboradores/services/vinculo.py:333`)
- **D5 — reativação por `update` fica fora do evento.** `reabrir_vigencia_impossivel` atualiza ativa/fim, registra trilha e retorna; não chama regeneração nem emite `save`. Introdução dessa omissão pela c4b **[nao verificado]**. (`app/colaboradores/services/vinculo.py:715`; `app/colaboradores/services/vinculo.py:722`; `app/colaboradores/services/vinculo.py:730`)
- **D6 — regeneração explícita e suplemento têm contratos distintos.** A primeira cria ausentes e estende horizonte; o segundo só visita existentes. ESCALA-AUTO ainda usa diretamente a primeira, inclusive com horizonte padrão. O alcance anterior autorizado desse ato não pode ser revogado por inferência desta obra. (`app/ponto/portas/celula.py:187`; `app/ponto/portas/celula.py:280`; `app/escala/services/escala_auto_executor.py:99`; `app/escala/services/escala_auto_executor.py:104`)
- **D7 — ausente com EC inativa ainda vigente.** O cron enumera apenas `ativa=True`; a autoridade admite inativa com fim declarado cobrindo a data. Assim, uma lacuna anterior a um fim futuro pode ficar fora do suplemento e do gerador. É consequência das seleções; existência em produção **[nao verificado]**. (`app/ponto/services/cadastro_futuro.py:22`; `app/ponto/portas/celula.py:851`; `app/escala/alimentacao.py:214`)

Para o **passado**, estas são as expansões por estado:

| Tratamento | Não existe | Gerada | Editada | Carimbada | Regenerada |
|---|---|---|---|---|---|
| **P0 — fora do alcance do evento** | conserva ausência | conserva | conserva | conserva | conserva |
| **PR — chamada formal de regeneração** | cria se coberta | R; fora da vigência pode apagar trabalho/marcos mantendo geradora | conserva | desenho pode mudar; julgamento depende de `recalculo` e proteção humana | R, com nova marca somente se mudou |
| **PG — gerador** | **cria: PROIBIDO** | DNA nulo pode ser preenchido: **PROIBIDO** | conserva | mesma regra de DNA nulo: **PROIBIDO** se escrever | conserva DNA existente |
| **PX — exclusão de EC referenciada** | sem efeito | FK pode virar nula: **PROIBIDO** | restauração recusa | FK pode virar nula: **PROIBIDO** | FK pode virar nula: **PROIBIDO** |

**PR** vem dos ramos de criação, reescrita e fora de vigência; o julgamento só ocorre com `recalculo`, excluindo vereditos humanos. **PG** escreve fora de `regenerar_celulas_vinculo`. **PX** decorre do `delete` e do `SET_NULL`, não de regeneração. (`app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:280`; `app/ponto/portas/celula.py:285`; `app/ponto/portas/celula.py:444`; `app/ponto/portas/celula.py:875`; `app/ponto/portas/celula.py:886`; `app/colaboradores/services/vinculo.py:1005`; `app/colaboradores/services/vinculo.py:1007`; `app/escala/models.py:1693`)

| Evento | Passada: atual / aval / divergência |
|---|---|
| Estrutura/município | **P0 / P0 / =**; município trata hoje separadamente. (`app/colaboradores/services/vinculo.py:933`) |
| Template comum | **PR**, ativos, desde competência anterior/início da EC até hoje / preservar alcance anterior / anterioridade exata **[nao verificado]**. (`app/escala/services/cadastro_tipo.py:337`; `app/escala/services/cadastro_tipo.py:351`) |
| Template no admin | **P0 / P0 / =**. (`app/escala/services/cadastro_tipo.py:380`) |
| Folga/foto | **PR / alcance anterior pela mesma porta / = quanto ao escritor**. (`app/escala/folgas.py:290`; `app/escala/folgas.py:293`) |
| Calendário salvo/trocado | **P0 / P0 / =** no suplemento; foto reconciliada somente após hoje. (`app/escala/folgas.py:632`; `app/escala/folgas.py:638`) |
| Vínculo novo/reativado pelo executor | **PR**, início..hoje / preservar alcance anterior / anterioridade exata **[nao verificado]**. (`app/colaboradores/services/vinculo.py:319`) |
| Fechar por `fechar_vigencia`, com/sem sucessor | **P0 / P0 / =**. (`app/colaboradores/services/vinculo.py:640`) |
| Encerrar por `save` | **PR**, menor início..hoje / mesma porta / = quanto ao escritor. (`app/escala/signals.py:60`; `app/escala/signals.py:73`) |
| Reabrir vigência impossível | **P0 / não ampliar passado / =**; a lacuna é futura, D5. (`app/colaboradores/services/vinculo.py:715`) |
| Âncora para valor não nulo | **PR**, competência corrente..hoje / mesma porta / = quanto ao escritor. (`app/escala/signals.py:119`; `app/escala/signals.py:125`) |
| Trabalha em feriado | **PR**, competência anterior..hoje / mesma porta / = quanto ao escritor. (`app/colaboradores/services/vinculo.py:878`; `app/colaboradores/services/vinculo.py:883`) |
| Feriado salvo/apagado | **P0 / P0 / =** no suplemento. (`app/ponto/services/cadastro_futuro.py:49`) |
| DnaContrato | Verifica congelamento do histórico encerrado após salvar template / conservar passado / cenário presente; execução **[nao verificado]**. (`app/escala/tests/test_celula_dia.py:166`; `app/escala/tests/test_celula_dia.py:197`) |
| Regeneração explícita | **PR / exceção formal / = quanto ao escritor**, respeitadas lavra e edição humana. (`app/ponto/portas/celula.py:114`; `app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:285`) |
| Cron | **PG / nenhuma escrita passada fora da regeneração / PROIBIDO** pela regra desta leitura. (`app/ponto/portas/celula.py:875`; `app/ponto/portas/celula.py:886`) |
| Restaurar snapshot/desfazer ajuste | **PX / conservar passada / PROIBIDO quando houver referência passada**; ocorrência real **[nao verificado]**. (`app/colaboradores/services/vinculo.py:1007`; `app/colaboradores/services/vinculo.py:1077`; `app/colaboradores/services/vinculo.py:1079`; `app/escala/models.py:1693`) |

**2. As 11 falhas da c4**

A numeração segue o anexo. O anexo demonstra os resultados em c4; aprovação posterior de cada teste em c4b permanece **[nao verificado]**. [nao verificado]

| # | Falha / classe revelada / casa da matriz | Tratamento presente em c4b |
|---|---|---|
| 1 | `Cancelar_na_caixa...`: Feriado salvo × futura; data do evento sem representação canônica. O traceback fornecido morre no preparo, antes do clique. **[nao verificado]** | **Adaptação local por `isinstance`**: converte string ISO antes de comparar datas. Resolve essa incompatibilidade no serviço; não normaliza a origem `Feriado`. (`app/ponto/services/cadastro_futuro.py:48`; `app/core/models.py:12`) |
| 2 | `a_tela_de_feriado_novo...`: mesma classe e mesma casa; falha no preparo fornecido. **[nao verificado]** | Mesmo contorno; há teste específico enviando string ao evento. (`app/ponto/services/cadastro_futuro.py:48`; `app/ponto/tests/test_cadastro_futuro_c4.py:146`) |
| 3 | `Excluir_confirmado...`: mesma classe; não constitui prova do clique após preparar o cenário. **[nao verificado]** | Mesma conversão. Exclusão também passa `instance.data` ao serviço. (`app/ponto/services/cadastro_futuro.py:48`; `app/core/signals.py:30`) |
| 4 | `Salvar_do_editar...`: Feriado salvo/movido × futura nas datas antiga/nova. | Mesma conversão seletiva; captura da data antiga continua no `pre_save`. (`app/ponto/services/cadastro_futuro.py:48`; `app/core/signals.py:14`; `app/core/signals.py:27`) |
| 5 | `dna_congelado_sobrevive...`: encerramento → sucessor → template × futura gerada/regenerada; preservação histórica. | **Mudança da testemunha do teste.** Agora exige retirada do carimbo encerrado, aplica explicitamente o sucessor e fabrica divergência por `QuerySet.update` do template; depois verifica cura por `salvar_tipo`. Não é `skip`/allowlist, mas também não é uma cura de produção dessa falha isoladamente. (`app/escala/tests/test_celula_dia.py:159`; `app/escala/tests/test_celula_dia.py:165`; `app/escala/tests/test_celula_dia.py:170`; `app/escala/tests/test_celula_dia.py:177`; `app/escala/tests/test_celula_dia.py:186`) |
| 6 | `template_derivado_trilha_e_passado_intocado`: fechamento com sucessor × futura anterior ao início do efeito, ainda coberta. | **Cura na origem do alcance**: começa em `min(fim_antigo,fim_novo)+1`. O teste original conserva a comparação da foto ant

[truncado em 20000 de 37456 caracteres]

## 12/11 15:30 · sessao e5f3193debbc · O168 · verificador · origem=Code

**Não considero a O168 encerrada:** os quatro fechamentos diretos receberam a guarda, mas há caminhos indiretos que chegam ao mesmo escritor sem consultá-la; além disso, “44 preservados” conta avaliações recusadas, não demonstra 44 chamados distintos salvos (`app/ponto/services/cartorio.py:1040`, `:1568`, `:1581`, `:1602`, `:963`; `app/chamados/signals.py:43`; `app/chamados/reconciliador.py:524`).

1. **Sítios diretos do cartório — os quatro passam pelo helper.**

   | Sítio | Passa por `_fecham_cobrancas_sem_furo`? | Evidência |
   |---|---|---|
   | Adiados pela lâmpada, depois do E2 | Sim | `app/ponto/services/cartorio.py:1035-1040` |
   | RECONCILIA-MARCO: marcos cobrados acenderam; inclui quem não protesta em dia sem trabalho | Sim | `app/ponto/services/cartorio.py:1562-1575` |
   | MARCO-INEXISTENTE: marco cobrado deixou de existir na escala | Sim | `app/ponto/services/cartorio.py:1576-1593` |
   | B5.3b: `COBRANCA_SEM_FURO` ou `COBRANCA_EM_DIA_SEM_TRABALHO` | Sim | `app/ponto/services/cartorio.py:1594-1606` |

   O helper pergunta a `pode_executar_csf` antes de chamar a porta; só devolve como fechados aqueles cuja transição retornou verdadeiro (`app/ponto/services/cartorio.py:955-966`).

2. **Há fechadores indiretos e externos sem essa guarda.**

   O desvio mais próximo está **antes do helper dos adiados**: cartório → E2 → `DisputaSupervisao.fechar()` → signal S88 → `ch.reconciliar()`. O E2 fecha a disputa quando C7 permite; o save dispara S88, que pede fechamento do chamado-pai sem consultar `pode_executar_csf` (`app/ponto/services/cartorio.py:1035-1040`; `app/chamados/reconciliador.py:609-615`; `app/chamados/models.py:1933-1937`; `app/chamados/signals.py:19-43`).

   A porta central não corrige essa ausência: decide pela matriz, pela premissa e pela pergunta **viva**, sem testar qualquer pergunta anteriormente respondida. Uma resposta já consumida pode, portanto, ficar fora da proteção existente da porta (`app/chamados/models.py:672-694`; `app/ponto/supra_juiz.py:543-549`). Reprodução dinâmica e ocorrência desse desvio em produção: **[não verificado]**.

   | Outro caminho | Helper / guarda de conversa | Evidência |
   |---|---|---|
   | Receiver da batida, posterior ao cartório → premissa morta → `_retratar` | Não / não | `app/ponto/signals.py:206-215`; `app/chamados/reconciliador.py:1433-1442`, `:1454-1465`, `:519-524` |
   | Consumidor de `EventoCelula` → absolvição pela célula | Não / não | `app/chamados/reconciliador.py:1369`, `:997-1010` |
   | Consumidor de `EventoCelula` → premissa morta | Não / não | `app/chamados/reconciliador.py:1373`, `:1465` |
   | Consumidor → lastro: todos os marcos cobrados acenderam | Não / não | `app/chamados/reconciliador.py:1383-1384`; `app/ponto/services/lastro.py:388-403` |
   | `detectar_ausencias`: batida satisfez os marcos cobrados | Não / não | `app/ponto/management/commands/detectar_ausencias.py:404-413`, `:845-849` |
   | Entrada sincronizada/API: marcos cobrados acesos | Não / não | `app/ponto/services/entrada_sincronizada.py:29-32` |
   | Ausência aprovada, premissa morta e lâmpada sem acusação | Não / não | `app/chamados/services/silencio_ausencia.py:29-42`, `:94-106` |
   | Fecho por afastamento ou cobertura | Não / não | `app/ponto/management/commands/detectar_ausencias.py:821-849` |
   | Designação/declaração de cobertura | Não / não | `app/ponto/services/cobertura.py:115-123`, `:311-319` |

   A retenção de `_retratar` não equivale à guarda CSF: protege irmã respondida **sem consumo** e justificativa alheia pendente, enquanto CSF protege qualquer `respondida_em` não nulo nas duas relações (`app/chamados/reconciliador.py:438-455`; `app/ponto/supra_juiz.py:543-549`).

   Materialização também fecha destino e clones sem CSF, mas ocorre no contexto de processamento de resposta; classificá-la integralmente como fechamento automático proibido exige distinguir o ato humano que a autorizou — **[não verificado]** (`app/chamados/services/materializacao.py:740-787`).

3. **Quem escreve e quem lê os campos e relações.**

   | Campo/relação | Quem escreve | Quem lê neste circuito |
   |---|---|---|
   | `ChamadoColaborador.status_local` | No fechamento: helper → `transicionar_com_pauta` → `reconciliar` → `_gravar_estado`; este atribui e salva. Nascimento também escreve pela criação | Seleção de vivos, matriz/gates, cartório e testes | [nao verificado]
   | Evidência | `app/ponto/services/cartorio.py:965`; `app/chamados/services/fecho_runtime.py:157-169`; `app/chamados/models.py:651-655`, `:228-229`, `:296`, `:1096` | `app/ponto/management/commands/supra_juiz.py:212`; `app/chamados/models.py:674`, `:713-716`; `app/ponto/services/cartorio.py:1672`; `app/ponto/tests/test_o168_guarda_csf_no_evento.py:104-106` |
   | `out['csf_com_conversa_preservado']` | Exclusivamente o incremento do helper encontrado neste circuito | Os quatro testes protegidos; retorno de `out` ao chamador | [nao verificado]
   | Evidência | `app/ponto/services/cartorio.py:963` | `app/ponto/tests/test_o168_guarda_csf_no_evento.py:121`, `:135`, `:147`, `:161`; `app/ponto/services/cartorio.py:2068-2071` |
   | `disputa_origem__perguntas` | Não é coluna nova: são relações reversas de `DisputaSupervisao.chamado` e `PerguntaDisputa.disputa`. Emissão cria esses vínculos | Prefetch do helper; juiz CSF percorre ambas |
   | Evidência | `app/chamados/models.py:1750-1755`, `:2417-2420`; `app/chamados/services/disputa_emissao.py:587-592`, `:837-845` | `app/ponto/services/cartorio.py:959`; `app/ponto/supra_juiz.py:543-545` |
   | `perguntas_que_resolvem` | Relação reversa de `PerguntaDisputa.chamado_destino`; nascimento/reuso e costura podem definir ou trocar o destino | Carregadores do cartório/supra e juiz CSF |
   | Evidência | `app/chamados/models.py:3386-3392`, `:2983-3018`; `app/chamados/services/fio.py:926-929` | `app/ponto/services/cartorio.py:127-128`; `app/ponto/management/commands/supra_juiz.py:190-192`; `app/ponto/supra_juiz.py:547-549` |

   **O insumo decisivo de CSF é somente `respondida_em`.** O juiz não consulta texto, `validada_em`, `materializada_em`, `via_resolucao`, booleano `conversa` ou histórico de decisões (`app/ponto/supra_juiz.py:539-550`).

   Escrevem esse carimbo: resposta humana, resposta automática, reparo pela trilha e carimbo com resposta. Reabertura para correção pode zerá-lo, preservando a resposta anterior no histórico, que CSF não lê (`app/chamados/services/disputa_emissao.py:1467-1469`, `:1652-1655`, `:1674-1676`, `:1593-1598`, `:1630-1632`; `app/chamados/models.py:3131-3153`; `app/ponto/supra_juiz.py:543-549`). Efeito dessa reabertura nos casos fornecidos: **[não verificado]**.

   As atribuições temporárias de `status_local` durante a simulação do pós-estado são restauradas; não constituem outro escritor persistente (`app/chamados/models.py:355-371`). Censo completo de todos os leitores de tela/relatório: **[não verificado]**.

4. **Os contadores do supra e do cartório não têm a mesma unidade operacional; 44 não equivalem aos seis fechamentos.**

   No cartório, cada passagem recusada pelo juiz incrementa o contador, sem conjunto de PKs vistos e antes de descobrir se a porta fecharia. Quem foi preservado continua em `vivos`, podendo ser contado novamente em outro sítio ou passada (`app/ponto/services/cartorio.py:961-965`, `:1575`, `:1593`, `:1602`, `:1050`).

   No supra, `CSF_COM_CONVERSA_PRESERVADO` cresce por chamado visitado no ramo da lavra `cobranca_indevida` com código reconhecido. A seleção é diferente da união das avaliações dos quatro sítios do cartório (`app/ponto/management/commands/supra_juiz.py:249-262`).

   O dado informa seis fechamentos com conversa no main — cinco na empresa 2 e um na 3 —, zero na cópia e 44 incrementos de preservação — 34 + 8 + 2. Não fornece a decomposição dos 44 por chamado e avaliação (`diff_sombra.txt:1-2`, anexo fornecido). Logo, quantos chamados distintos esses incrementos representam e quantos realmente seriam fechados sem a guarda: **[não verificado]**.

   O rótulo **“preservados/impacto” não descreve fielmente essa soma de recusas**. Usá-la como número de chamados distintos para o limite percentual mistura unidades, contrariando “rótulo diz o que a conta faz; contador == universo” (`app/ponto/services/cartorio.py:963`; `diff_sombra.txt:9-14`; `CLAUDE.md:35-36`).

   Há ainda avaliações potencialmente omitidas: a releitura pós-emissão usa `_conv`, mas não incorpora seu contador CSF ao `out` principal. O resumo de `processar_cartorio` também não imprime esse campo (`app/ponto/services/cartorio.py:1114-1126`; `app/ponto/management/commands/processar_cartorio.py:66-70`).

5. **Prefetch: até dois SELECTs adicionais por invocação fria; N+1 permanece no supra.**

   Para cada um dos quatro sítios, uma lista não vazia e sem cache pode exigir **um SELECT de disputas + um SELECT de perguntas**. Lista vazia retorna antes; sem disputas não há segundo nível; objetos já carregados podem reaproveitar cache. Trata-se de previsão estrutural, não medição SQL (`app/ponto/services/cartorio.py:956-959`; `app/chamados/models.py:1750-1755`, `:2417-2420`).

   Esse limite é por **invocação/lista**, não duas consultas para o sítio durante toda a rodada: há laços por célula e segunda passada. Sobreposição das listas pode reutilizar os mesmos objetos carregados (`app/ponto/services/cartorio.py:1037-1050`, `:1382`, `:1568-1602`). A relação direta já vem prefetchada no carregador do cartório (`app/ponto/services/cartorio.py:127-128`).

   No supra, o carregador prefetcha somente `perguntas_que_resolvem`; CSF também percorre origem e perguntas por disputa. Permanece, portanto, consulta por chamado para origem e por disputa para perguntas, até eventual retorno antecipado (`app/ponto/management/commands/supra_juiz.py:190-192`; `app/ponto/supra_juiz.py:543-549`). Contagens SQL efetivamente executadas e cumprimento dos tetos: **[não verificado]**.

6. **A lei citada não autoriza uma exclusão geral de FIO-DUP.**

   O literal reproduzido diz: “o cartorio nao fecha cobranca sem furo de chamado que tem conversa”; não contém exceção para FIO-DUP. A exclusão aparece na justificativa escrita pela própria obra (`app/ponto/services/cartorio.py:941-947`).

   Deduplicar enquanto outro chamado continua cobrando um furo existente pode estar fora da premissa “sem furo”. Entretanto, o gate aceita `premissa_regularizada` **antes** de examinar deduplicação: o nome FIO-DUP não garante que todo fechamento dali preserve um furo ainda existente (`app/chamados/models.py:739-750`).

   FIO-DUP e FIO-DUP-C8 não passam pelo helper. Sua isenção lê **texto das perguntas dirigidas**, enquanto CSF lê **carimbo nas perguntas dirigidas e de origem**. São proteções diferentes; conversa apenas na origem não é coberta por essa isenção (`app/ponto/services/cartorio.py:1666-1680`, `:1690-1695`; `app/ponto/supra_juiz.py:543-549`). Ocorrência dessa combinação no dado: **[não verificado]**.

7. **Os testes têm controles úteis, mas não comprovam toda a lei nem a cascata.** [nao verificado]

   Há pares com/sem conversa para os quatro sítios, pela porta real, com recarga do estado final. Isso sustenta a expectativa estática de que retirar individualmente cada guarda faça seu caso falhar; execução dos quatro mutantes: **[não verificado]** (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:100-161`).

   A lacuna concreta está no adiado: a fixture cria perguntas sem `celula` e sem `data_evento`; E2 exige `celula` não nula. Assim, o teste não percorre a cascata real E2 → fechamento da disputa → S88. A pergunta sem data também pode ser desconsiderada pelo gate por competência indecidível (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:80-88`; `app/chamados/reconciliador.py:628-638`; `app/chamados/models.py:364-365`).

   Todos os cenários plantam dia inteiramente batido, e a mesma pergunta integra origem e destino. Faltam provas separadas para dia sem trabalho, marco satisfeito com outro furo restante, conversa apenas na origem/destino e fechador posterior ao cartório. A idempotência não exige estabilidade do contador CSF (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:62-66`, `:80-88`, `:109-175`).

8. **Classes históricas: resultado desta leitura.**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | O helper reutiliza CSF; não copia sua decisão. FIO mantém predicado de proteção diferente, conforme achado 6 (`app/ponto/services/cartorio.py:955-962`, `:1666-1670`). |
   | Segundo escritor fora da porta | O trecho novo conserva wrapper e escritor existentes. Os desvios encontrados também passam pela porta: falta-lhes a guarda, não a porta (`app/ponto/services/cartorio.py:965`; `app/chamados/signals.py:43`; `app/chamados/models.py:655`). |
   | Relógio fora da borda | O helper novo não lê relógio; as duas ocorrências deslocadas são conversões de timestamps explícitos (`app/ponto/services/cartorio.py:953-967`, `:1234`, `:1387`). |
   | Derivação paralela | Não aparece nova derivação de conversa no helper; a divergência entre texto/FIO e carimbo/CSF permanece (`app/ponto/services/cartorio.py:962`, `:1670`; `app/ponto/supra_juiz.py:545-549`). |
   | RED sem mordida | Há pares comportamentais, mas a cascata E2/S88 escapa à fixture; mutação executada por sítio **[não verificado]** (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:80-88`, `:109-161`; `app/chamados/reconciliador.py:632`). |
   | Ordem de trava | A extração mantém o caminho central de aquisição de travas. A leitura/prefetch da guarda ocorre antes de entrar nessa porta; ausência de corrida concorrente **[não verificado]** (`app/ponto/services/cartorio.py:959-965`; `app/chamados/models.py:651-652`, `:703-713`). |
   | Pouso antes do pré-requisito | A cópia ainda configura supra com `--executar`; o anexo não mede `CSF_RETRATADO`. Remoção posterior da flag, pouso e pré-requisito zero **[não verificado]** (`app/config/crons.py:599`; `diff_sombra.txt:1-2`). |
   | Campo com dois significados | O contador tem unidades diferentes entre chamadores. “Conversa” em CSF significa carimbo de resposta, não o booleano homônimo (`app/ponto/services/cartorio.py:963`; `app/ponto/management/commands/supra_juiz.py:262`; `app/ponto/supra_juiz.py:543-549`; `app/chamados/models.py:2777-2803`). |

9. **55→52 e catraca temporal: ajustes coerentes com a extração, sem afrouxamento demonstrado nesses dois selos.**

   Os quatro pontos físicos de transição viraram uma chamada física no helper; S33 passa a identificá-la, enquanto FIO-DUP/FIO-DUP-C8 continuam cadastrados. O selo segue procurando escritores não cadastrados e exige exceções vazias. Portanto, a redução é consequência da extração escolhida; não demonstra redução de cobertura das quatro rotas de negócio (`diff.txt`, trecho de `e4b_c12_sitios.json`; `app/chamados/tests/e4b_c12_sitios.json:259-280`; `app/chamados/tests/test_e4b_c12_selo.py:22-36`; `app/ponto/services/cartorio.py:965`).

   A catraca apenas registra o deslocamento das mesmas duas expressões de `localdate(timestamp)`. A igualdade integral das ocorrências continua exigida; não houve aumento de tolerância nesse diff (`app/core/tests/test_medida_turno_do_par.py:319-321`, `:546-550`; `app/ponto/services/cartorio.py:1234`, `:1387`).

10. **Limites da verificação.** RED/GREEN executados, mutantes individuais, custo SQL medido, reprodução dos desvios, decomposição dos 44, conservação depois dos receivers/outbox, `CSF_RETRATADO=0`, implantação e identidade Git independente do hash/pai informado: **[não verificado]**. O anexo demonstra somente os resultados declarados das duas rodadas de `julgar_celula`; seu próprio cabeçalho delimita esse ato em rollback (`diff_sombra.txt:1-2`, `:16-20`).

## 12/11 15:27 · sessao 5516d069f335 · - · - · origem=Ronald (tmux)

1. **O selo O168 não prova a cascata indireta E2→S88.** A fixture cria perguntas sem `celula` e sem `data_evento` (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:80`–88; campos nulos em `app/chamados/models.py:2415`, `:2424`). O E2 exige `celula__isnull=False` (`app/chamados/reconciliador.py:628`–638), logo não carimba nem fecha aquelas disputas. Já o caminho real fecha disputa em `app/chamados/reconciliador.py:609`–615 e o S88 chama `reconciliar` sem `pode_executar_csf` (`app/chamados/signals.py:29`–43).

2. **O teste do “adiado” pode fechar no controle sem exercitar esse pré-requisito.** A pergunta viva sem data aciona o adiamento (`app/ponto/services/cartorio.py:1418`, `:1562`–1565), mas o gate desconsidera competência indecidível (`app/chamados/models.py:364`–365). Assim, o controle fechar não demonstra que o E2 tenha resolvido pergunta datada real. O texto promete “lavrar→E2→reconciliar” (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:14`), mas a fixture não cobre essa sequência.

3. **Mordida dos quatro sítios: plausível por análise estática; execução dos mutantes [não verificado].** Há pares com e sem conversa para B53b (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:109`–121), marco aceso (`:126`–135), adiado (`:138`–147) e inexistente (`:152`–161), com estado final recarregado (`:104`–106). Os ramos correspondentes estão em `app/ponto/services/cartorio.py:1602`, `:1568`, `:1040`, `:1581`. Porém o arquivo não implementa mutação individual: somente afirma que cada retirada fica vermelha (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:16`–17). Não rodei testes.

4. **A cobertura é estreita.** Todos os casos plantam dia inteiramente batido (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:62`–66); não há caso específico de dia sem trabalho nem marco satisfeito com outro furo restante. A mesma pergunta pertence à origem e ao destino (`:80`–88), logo não há prova separada das duas relações lidas pelo juiz (`app/ponto/supra_juiz.py:543`–549). Idempotência compara estado, veredito e quantidade de perguntas, mas não exige contador estável (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:166`–175).

5. **55→52 é consolidação de chamadas físicas, sem afrouxamento demonstrado do selo global.** S33 identifica o único fechamento no novo helper (`app/chamados/tests/e4b_c12_sitios.json:259`–264; `app/ponto/services/cartorio.py:965`), enquanto FIO-DUP continua nos S37/S38 (`app/chamados/tests/e4b_c12_sitios.json:267`–280). O selo varre toda a produção e reprova também escritores não cadastrados (`app/chamados/tests/test_e4b_c12_selo.py:22`–32), mantendo exceções vazias (`:34`–36). O manifesto é resolvido por função e texto, não pelo número antigo da linha (`app/chamados/tests/e4b_c12_verificar.py:84`–115). É consequência da extração escolhida; a extração não era a única implementação possível da cura.

6. **A catraca temporal só registra deslocamento.** As duas expressões continuam conversões de timestamps explícitos (`app/ponto/services/cartorio.py:1234`, `:1387`), registradas agora em `app/core/tests/test_medida_turno_do_par.py:319`–321. A igualdade integral das ocorrências segue exigida (`:546`–550); não há relaxamento do teto no diff fornecido.

7. **Classes estruturais no trecho novo:** o helper importa e chama o juiz existente (`app/ponto/services/cartorio.py:955`, `:962`), conserva a porta real (`:954`, `:965`) e não introduz relógio ou `select_for_update` (`:953`–967). O fechamento continua adquirindo a ordem central de trava (`app/chamados/models.py:651`–652, `:703`–713; ordem implementada em `:2667`, `:2676`, `:2689`, `:2693`). Ausência de corrida entre prefetch/guarda e aquisição dessas travas: **[não verificado]**. Pouso e execução do supra após pré-requisito: **[não verificado]**; os dados fornecidos não demonstram `CSF_RETRATADO=0`.

## 12/11 15:27 · sessao 6dbf9ebf2159 · - · - · origem=Ronald (tmux)

1. **O contador do cartório conta recusas por avaliação, não chamados distintos.** O incremento ocorre em cada passagem pelo helper, sem conjunto de PKs vistos: `app/ponto/services/cartorio.py:959-964`. Chamado preservado continua em `vivos`, pois só quem fechou é removido: `app/ponto/services/cartorio.py:1575`, `app/ponto/services/cartorio.py:1593`. Assim, pode ser contado em RECONCILIA-MARCO e novamente em B5.3b: `app/ponto/services/cartorio.py:1568`, `app/ponto/services/cartorio.py:1602`; também pode reaparecer na segunda passada: `app/ponto/services/cartorio.py:1050`. Os 44 informados não demonstram 44 chamados distintos salvos; decomposição efetiva por PK/sítio [não verificado].

2. **No supra, seleção e unidade operacional diferem.** O contador nasce apenas quando a lavra é `cobranca_indevida` e contém código reconhecido, percorrendo uma vez os chamados de `vivos`: `app/ponto/management/commands/supra_juiz.py:249-262`. É contador por chamado visitado nesse ramo da varredura; o cartório conta cada recusa em qualquer dos quatro sítios. Portanto, o mesmo rótulo não garante a mesma conta: `app/ponto/services/cartorio.py:946`, `app/ponto/management/commands/supra_juiz.py:262`.

3. **Há avaliação potencialmente omitida do total devolvido.** A releitura pós-emissão usa outro dicionário `_conv`; copia códigos e alguns contadores, mas não `csf_com_conversa_preservado`: `app/ponto/services/cartorio.py:1114-1126`.

4. **Escritores/leitores do contador.** Escritor de cartório: helper em `app/ponto/services/cartorio.py:963`; leitores nos quatro testes protegidos: `app/ponto/tests/test_o168_guarda_csf_no_evento.py:121`, `:135`, `:147`, `:161`. `contadores()` não declara essa chave: `app/ponto/services/cartorio.py:1983`. O comando cria `out` por lote e o passa a cada colaborador, mas não imprime esse contador no resumo: `app/ponto/management/commands/processar_cartorio.py:43`, `:65-70`. No supra, escritor: `app/ponto/management/commands/supra_juiz.py:262`; leitor para impressão: `app/ponto/management/commands/supra_juiz.py:297-300`.

5. **O juiz lê somente relações e `respondida_em`.** Percorre `ch.disputa_origem.all()`, cada `d.perguntas.all()` e `ch.perguntas_que_resolvem.all()`; qualquer carimbo não nulo impede execução: `app/ponto/supra_juiz.py:539-550`. Não consulta texto, `conversa`, validação, materialização, via ou histórico `DecisaoPergunta`: `app/ponto/supra_juiz.py:543-549`. Uma pergunta de fato nasce com `conversa=False`; pergunta de conversa nasce pela outra porta com `True`: `app/chamados/models.py:2777-2803`. Logo “conversa” no rótulo da guarda não significa o campo booleano `conversa`.

6. **Origem e perguntas são relações reversas, não campos novos escritos pelo diff.** `disputa_origem` decorre de `DisputaSupervisao.chamado`: `app/chamados/models.py:1750-1755`; `perguntas`, de `PerguntaDisputa.disputa`: `app/chamados/models.py:2417-2420`; destino direto, de `PerguntaDisputa.chamado_destino`: `app/chamados/models.py:3386-3392`. Emissão escreve o vínculo da disputa em `app/chamados/services/disputa_emissao.py:587-592`, e pede nascimento da pergunta com disputa/destino em `app/chamados/services/disputa_emissao.py:837-845`; a inserção efetiva está em `app/chamados/models.py:3015-3018`. Reuso pode trocar destino em `app/chamados/models.py:2983-3007`; reapontamento explícito escreve por `app/chamados/services/fio.py:926-929`.

7. **Quem escreve o insumo da guarda.** Resposta humana: `app/chamados/services/disputa_emissao.py:1467-1469`; resposta automática: `app/chamados/services/disputa_emissao.py:1652-1655`; reparo pela trilha: `app/chamados/services/disputa_emissao.py:1674-1676`; carimbo com resposta: `app/chamados/models.py:3131-3153`. Reabrir para correção zera resposta/carimbo atual, preservando foto no histórico: `app/chamados/services/disputa_emissao.py:1593-1598`, `:1630-1632`. Ressuscitação também pode zerar `respondida_em`: `app/chamados/models.py:1567`. O juiz não lê a foto histórica: `app/ponto/supra_juiz.py:543-549`; ocorrência/efeito em produção [não verificado].

8. **`status_local` mantém a porta existente.** O helper chama `transicionar_com_pauta(..., ato=x.reconciliar)`: `app/ponto/services/cartorio.py:965`; o wrapper executa esse ato e devolve o resultado: `app/chamados/services/fecho_runtime.py:157-169`. `reconciliar` obtém decisão e chama `_gravar_estado`: `app/chamados/models.py:651-656`. A atribuição persistida é `self.status_local = novo`, seguida do `save`: `app/chamados/models.py:228-229`, `:296`. Nascimento escreve estado por `abrir`/`criar`, com exemplo do segundo em `app/chamados/models.py:1096`; atribuições em `:356` e `:371` são simulação temporária em memória. Leitores relevantes: seleção de vivos do supra `app/ponto/management/commands/supra_juiz.py:212`; motor/gates `app/chamados/models.py:674`, `:713-716`; a auditoria não enumerou todos os consumidores de tela/relatório [não verificado].

9. **Prefetch: custo previsto de até dois SELECTs adicionais por chamada fria não vazia do helper.** O caminho tem duas relações reversas (`disputas`, depois `perguntas`): `app/chamados/models.py:1750-1755`, `:2417-2420`; o helper prefetcha ambas de uma vez: `app/ponto/services/cartorio.py:959`. Lista vazia retorna antes: `app/ponto/services/cartorio.py:956-958`; sem disputas, não há segundo nível para carregar; cache já preenchido pode evitar consultas. Destino direto já vem prefetchado pelo carregador: `app/ponto/services/cartorio.py:127-128`. Quantidade SQL executada, cache em cada sítio e limites de teste [não verificado]; isto é leitura estrutural, não medição.

10. **N+1 permanece no supra.** Seu carregador prefetcha somente `perguntas_que_resolvem`: `app/ponto/management/commands/supra_juiz.py:190-192`; o juiz consulta também origem e perguntas por disputa: `app/ponto/supra_juiz.py:543-544`. Portanto, nesse caminho há consultas por chamado para origem e por disputa para perguntas, até eventual retorno antecipado: `app/ponto/supra_juiz.py:545-546`. Nos quatro sítios novos, o helper evita esse padrão para as relações que ele carrega: `app/ponto/services/cartorio.py:959`.

## 12/11 15:27 · sessao 18e177eeea5c · - · - · origem=Ronald (tmux)

Transições comuns da célula futura:

| Estado | Comportamento atual |
|---|---|
| Não existe | Suplemento não cria: seleciona somente células existentes, `origem='gerada'`, de amanhã ao fim da janela; `app/ponto/services/cadastro_futuro.py:18-31`. A chamada direta de regeneração cria ausentes em dias cobertos; `app/ponto/portas/celula.py:280-284`. |
| Gerada | Se trabalha, geradora ou DNA divergir, reescreve e registra `dna_anterior`, `regenerada_em`, `regeneracoes`; `app/ponto/portas/celula.py:285-299`. |
| Editada | Suplemento exclui na seleção; regeneração também só altera origem gerada; `app/ponto/services/cadastro_futuro.py:23`; `app/ponto/portas/celula.py:285`. |
| Carimbada com lâmpada | Não existe guarda por julgamento/carimbo: pode mudar DNA e `dna.lampadas`, conservando ATA/veredito/impressão/julgada_em; `app/ponto/portas/celula.py:285-299`; montagem das lâmpadas em `app/ponto/management/commands/gerar_celulas.py:79-104`. Rejulgamento pula futuras; `app/ponto/portas/celula.py:448-454`. |
| Regenerada | Continua `origem='gerada'`, não é estado exclusivo; recebe o mesmo tratamento e novo rastro se mudar; `app/escala/models.py:1690-1691`; `app/ponto/portas/celula.py:285-299`. |

Fora da vigência, a regeneração limpa a futura gerada cuja geradora é o vínculo encerrado, sem outro vínculo cobrindo e sem lavra: `trabalha=False`, geradora nula, DNA sem marcos e rastro de regeneração; `app/ponto/portas/celula.py:244-259`. Havendo sucessor cobrindo, conserva até o evento que regenere pelo sucessor; `app/ponto/portas/celula.py:247`; `app/colaboradores/services/vinculo.py:330-335`.

Inativa sem fim é recusada pela porta; `app/ponto/portas/celula.py:146-155`. O vigia consulta `vinculo_do_dia` e separa esse caso em C6; `app/ponto/services/vigia_cadastro_futuro.py:31-45`. A lista C6 inclui vínculos inativos sem fim, sem exigir referência de célula; `app/escala/services/cadastro_realidade.py:320-328`.

| Evento | Futuro existente | Passado |
|---|---|---|
| Salvar estrutura: município do posto | `atualizar_posto` e `definir_municipio_posto` disparam evento atomicamente; `app/colaboradores/services/estrutura.py:65-96`. Evento percorre vínculos do posto e suplementa futuras; `app/colaboradores/services/vinculo.py:931-941`. | Só hoje, se vínculo cobre hoje; `app/colaboradores/services/vinculo.py:933-937`. |
| Salvar estrutura: outros campos | Empresa/praca/posto ativo/opera_em_feriado apenas salvam, sem suplemento explícito; `app/colaboradores/services/estrutura.py:18-22`; `:36-40`; `:75-85`. Necessidade para cada campo depende de seu efeito; anterioridade da omissão [nao verificado]. | Sem escrita direta em célula nessas funções; mesmas referências. |
| Vínculo novo / reativar pelo executor | Executor atômico, cria ou reativa e suplementa desde início; `app/colaboradores/services/vinculo.py:169`; `:253-291`; `:330-335`. | Regenera início..hoje, sem extensão; `app/colaboradores/services/vinculo.py:319-325`. |
| Encerrar por `fechar_vigencia` | Suplementa apenas após `min(fim anterior,fim novo)+1`, evitando tocar dias cujo cadastro não mudou; `app/colaboradores/services/vinculo.py:640-650`. | Nenhum suplemento passado; `app/colaboradores/services/vinculo.py:640-641`. |
| Encerrar por `encerrar_vinculo` | Save dispara signal e suplemento; `app/colaboradores/services/vinculo.py:821-824`; `app/escala/signals.py:80-83`. | Signal regenera desde menor início até hoje; `app/escala/signals.py:60-75`. |
| Reativar por `reabrir_vigencia_impossivel` | `update` de ativa/fim, sem signal ou suplemento; `app/colaboradores/services/vinculo.py:715-732`. Omissão verificada; introdução pela c4b [nao verificado]. | Também não regenera; mesma referência. |
| Alterar âncora | Signal suplementa futuras quando muda para valor não nulo; `app/escala/signals.py:111-134`. | Desde início da competência corrente até hoje; `app/escala/signals.py:119-127`. |
| Trocar template via `salvar_tipo` | Todos os vínculos do tipo, inclusive encerrados; `app/escala/services/cadastro_tipo.py:368-376`. | Só vínculos ativos, janela anterior até hoje; `app/escala/services/cadastro_tipo.py:337-353`. |
| Template pelo admin | Só suplemento futuro; `app/escala/services/cadastro_tipo.py:380-387`. | Preserva; mesma referência. |
| Trocar calendário do vínculo | Sincroniza foto futura e regenera existentes; `app/colaboradores/services/vinculo.py:455-460`; `app/escala/folgas.py:616-640`. | Esse suplemento preserva; `app/escala/folgas.py:632`. |
| Trabalha em feriado | Atualiza flag e suplementa futuras; `app/colaboradores/services/vinculo.py:869-892`. | Janela anterior..hoje pela regeneração; `app/colaboradores/services/vinculo.py:878-887`. |
| ESCALA-AUTO | Continua chamando regeneração direta, que cria ausentes e estende horizonte; `app/escala/services/escala_auto_executor.py:104-105`; `:143-144`; `app/ponto/portas/celula.py:187-192`; `:280-284`. | Aplicação exige início futuro; `app/escala/services/escala_auto_executor.py:99-100`. |
| Cron `gerar_celulas` | Mede antes, regenera hoje e gera ausentes; conserva futuras existentes; `app/ponto/management/commands/gerar_celulas.py:337-344`; `app/ponto/portas/celula.py:892-897`. | **PROIBIDO** pela regra estrita do pedido: cria ausente passada e enriquece DNA nulo fora de `regenerar_celulas_vinculo`; `app/ponto/portas/celula.py:875-888`. |

Outros **PROIBIDO** condicionais: restauração de snapshot apaga vínculos e deixa `SET_NULL` alterar células passadas geradas fora da porta; recusa apenas referências editadas. `app/colaboradores/services/vinculo.py:1003-1007`; FK em `app/escala/models.py:1692-1694`. Desfazer ajuste remove vínculo de modo equivalente; `app/colaboradores/services/vinculo.py:1077-1080`. Ocorrência real de referências passadas nesses caminhos [nao verificado].

Lâmpadas: a lei exige evento carimbar/julgar e consumidor ler a lâmpada; `CLAUDE.md:170-173`. O leitor apresenta a hora prevista da ATA; `app/escala/services/leitor_celula.py:114-123`. Outro consumidor cruza marco do DNA com luz da ATA; `app/chamados/juizes.py:1337-1345`. Portanto o estado futuro já carimbado precisa fechar a transição entre DNA novo e ATA antiga. O cartório também recusa data futura; `app/ponto/services/cartorio.py:2025`. Se a lei existente não define invalidação sem julgamento antecipado: **lei: qual estado da ATA/lâmpada futura conserva coerência após o cadastro mudar**.

Janela: suplemento usa união das competências das empresas ativas, enquanto a autoridade individual respeita o corte da empresa; `app/ponto/services/cadastro_futuro.py:16-18`; `app/ponto/management/commands/gerar_celulas.py:281-291`; `app/ponto/janelas.py:60-63`. Existência em produção de cortes diferentes [nao verificado].

Não localizei identificadores `redefinir_config` e `trocar_tipo`; caminho exato [nao verificado].

## 12/11 15:26 · sessao 3e23a5af05bc · - · - · origem=Ronald (tmux)

1. Os quatro sítios diretos foram encaminhados ao helper: adiados `app/ponto/services/cartorio.py:1040`; RECONCILIA-MARCO `:1568`; MARCO-INEXISTENTE `:1581`; B5.3b (`COBRANCA_EM_DIA_SEM_TRABALHO`/`COBRANCA_SEM_FURO`) `:1594-1602`. O helper consulta o juiz antes da porta (`:962-965`).

2. **Há bypass indireto dentro do próprio ato do cartório.** Antes do helper dos adiados, `julgar_colab` chama E2 (`app/ponto/services/cartorio.py:1035-1040`). E2 fecha disputas aprovadas pelo C7 (`app/chamados/reconciliador.py:609-615`). `DisputaSupervisao.fechar` salva o fechamento (`app/chamados/models.py:1933-1937`), disparando `disputa_fechada_fecha_chamado_pai`, que chama `transicionar_com_pauta(..., ato=ch.reconciliar)` sem consultar `pode_executar_csf` (`app/chamados/signals.py:19-43`). A porta consulta premissa e pergunta viva, não a guarda de qualquer conversa (`app/chamados/models.py:672-694`). Portanto a guarda local dos quatro sítios não constitui guarda de toda a cascata. Reprodução e ocorrência nesse caminho em produção: [não verificado].

3. **Há fechadores posteriores à porta do cartório sem a guarda.** O receiver posterior da batida chama `reconciliar_chamados_colab` (`app/ponto/signals.py:206-215`), que executa E2 e `retratar_premissa_morta_colab` (`app/chamados/reconciliador.py:1433-1442`). Este percorre chamados vivos e chama `_retratar` quando a premissa morre (`:1454-1465`). `_retratar` chama a porta sem `pode_executar_csf` (`:519-524`); sua retenção cobre irmã respondida **sem consumo**, não toda pergunta que já foi respondida (`:438-455`).

4. O consumidor de `EventoCelula` chama três fechadores adicionais: absolvidos pela célula (`app/chamados/reconciliador.py:1369`), premissa morta (`:1373`) e lastro (`:1383-1384`). Os absolvidos chamam `_retratar` (`:997-1010`), conforme a célula lavrada sem acusação e sem marco pendente (`:1019-1032`). O lastro percorre `batida_ausente` na fila e chama a porta quando todos os marcos cobrados acenderam, sem a guarda de conversa (`app/ponto/services/lastro.py:388-403`). Assim o DIFF do ato isolado `julgar_celula` não prova conservação no consumidor posterior; execução real desse consumidor nos 44 casos: [não verificado].

5. Outros fechadores do mesmo tipo, todos sem helper/juiz de conversa:

   - `detectar_ausencias`: exige marcos acesos antes de chamar `_fechar_chamado` (`app/ponto/management/commands/detectar_ausencias.py:404-413`); o método chama diretamente a porta (`:845-849`).
   - Sync/API: busca a cobrança viva do dia, exige `marcos_acesos_do_dia` e chama a porta (`app/ponto/services/entrada_sincronizada.py:29-32`).
   - Ausência/cobertura: chamadas diretas da porta (`app/ponto/management/commands/detectar_ausencias.py:821-849`).
   - Materialização: `_reconciliar_destino_e_clones` fecha destino e clones após verificar somente irmãs sem consumo (`app/chamados/services/materializacao.py:740-777`). Aqui há ato de materialização/decisão humana, portanto equipará-lo integralmente ao fechamento automático proibido exige análise contextual [não verificado].

6. **FIO-DUP e FIO-DUP-C8 continuam fora.** O primeiro exige que todas as perguntas dirigidas sejam duplicatas e fecha via porta (`app/ponto/services/cartorio.py:1674-1683`); o segundo pode fechar outros vivos do dia (`:1690-1695`). A isenção desses blocos verifica texto em `perguntas_que_resolvem`, não `respondida_em` nem perguntas da disputa de origem (`:1666-1670`). Já o juiz legal verifica `respondida_em` nas duas relações (`app/ponto/supra_juiz.py:543-549`). Logo as guardas têm extensão diferente.

7. A exclusão FIO-DUP está declarada pelo comentário da obra (`app/ponto/services/cartorio.py:946-947`), mas a lei literal reproduzida não contém essa exceção (`:941-942`). Além disso o gate aceita primeiro `premissa_regularizada` e só depois tenta deduplicação (`app/chamados/models.py:739-750`); chamar o sítio de FIO-DUP não garante, por si, que todo fechamento dali ocorreu enquanto o furo persistia. A hipótese de pergunta respondida na origem e somente perguntas dirigidas sem texto passa pela isenção estreita do FIO e seria barrada pelo juiz de conversa (`cartorio.py:1674-1680`; `supra_juiz.py:543-549`). Ocorrência dessa combinação no dado raspado: [não verificado].

8. No supra, o comando consulta o mesmo juiz antes de retratar (`app/ponto/management/commands/supra_juiz.py:257-263`), e `CSF_RETRATADO` só cresce quando `_retratar` retorna verdadeiro (`:284-290`). A porta central `reconciliar` segue sem consulta ao juiz de conversa (`app/chamados/models.py:642-694`), permitindo que os demais chamadores acima fechem o que o supra preservaria.

Não executei testes, não consultei banco/logs e não escrevi arquivos. Prova dinâmica dos bypasses e universo completo de todas as variantes de fechamento no repositório: [não verificado]. [nao verificado]

## 12/11 15:26 · sessao dfd10d4e98f4 · - · - · origem=Ronald (tmux)

Censo e semântica das portas de estrutura, somente por leitura:

| Evento | Alcance e comportamento no código |
|---|---|
| Atualizar estrutura/Posto | `atualizar_posto` e `definir_municipio_posto` são atômicos e disparam evento somente quando muda `municipio_id`; demais campos de Posto não disparam regeneração por essa porta. `app/colaboradores/services/estrutura.py:53`, `:65`, `:88`. |
| Município do posto | Para cada vínculo do posto, regenera HOJE se o vínculo o cobre; futuro usa o suplemento para todos os vínculos do posto; passado anterior a hoje fica fora. `app/colaboradores/services/vinculo.py:931`, `:933`, `:938`. |
| Salvar template pela porta comum | Passado: ativos do template, desde o máximo entre início do vínculo e competência anterior, até hoje, pela porta de regeneração. Futuro: todos os vínculos do template, inclusive encerrados, pelo suplemento. `app/escala/services/cadastro_tipo.py:337`, `:351`, `:373`. |
| Salvar template no Django admin | Salva template e propaga somente futuras, dentro do mesmo atomic. `app/escala/services/cadastro_tipo.py:380`. |
| Salvar calendário de folga | Grava definição e sincroniza vínculos vinculados ao calendário no mesmo atomic. `app/escala/folgas.py:578`, `:597`. |
| Sincronizar calendário | Seleciona células futuras existentes, inclusive editadas para determinar meses; restringe à cobertura do vínculo; reconcilia fotos futuras mensais sem sobrescrever folgas manuais; relavra somente células `origem='gerada'` pelo suplemento. `app/escala/folgas.py:619`, `:622`, `:625`, `:638`; preservação manual em `:187`, `:194`. |
| Lançar/alterar/remover foto de folga | O caminho comum relavra passado até hoje pela porta e acrescenta futuras existentes; eventual rejulgamento de mês segue a mesma porta. `app/escala/folgas.py:283`, `:290`, `:293`, `:298`. |
| Trocar calendário do vínculo | Salva o FK e sincroniza futuras quando o calendário mudou, no mesmo atomic. `app/colaboradores/services/vinculo.py:454`. |
| Feriado salvo | `pre_save` captura data antiga pela autoridade de consultas; `post_save` passa datas antiga/nova ao suplemento; `raw=True` não propaga. `app/core/signals.py:14`, `:22`; `app/core/feriados.py:86`. |
| Feriado apagado | `post_delete` propaga a data apagada. `app/core/signals.py:30`. |
| Suplemento por feriado | Normaliza texto ISO usando `isinstance`; limita a datas estritamente futuras na janela corrente; só células geradas; pergunta `vinculo_do_dia` alimentado com a geradora; se a autoridade cala, não escolhe sucessor. `app/ponto/services/cadastro_futuro.py:48`, `:49`, `:53`, `:59`, `:62`. |

O suplemento comum não cria célula ausente: trava e seleciona somente futuras `origem='gerada'` existentes na janela do gerador, depois chama `regenerar_celulas_vinculo` um dia por chamada com horizonte desligado. Editada fica fora; regenerada continua elegível porque sua origem permanece `gerada`. `app/ponto/services/cadastro_futuro.py:16`, `:22`, `:27`, `:30`; `app/escala/models.py:1709`.

**Problema de desenho demonstrável estaticamente:** template e município chamam o suplemento com cada vínculo, mas o suplemento seleciona todas as células geradas da pessoa sem restringir à geradora. A porta troca o carimbo sempre que aquele vínculo cobre a data, sem perguntar `vinculo_do_dia`. Vínculos sobrepostos do mesmo template são percorridos por início decrescente; portanto o mais antigo pode sobrescrever por último o carimbo do mais novo. Incidência em produção: [nao verificado]. `app/escala/services/cadastro_tipo.py:373`; `app/escala/models.py:1003`; `app/ponto/services/cadastro_futuro.py:22`, `:26`; `app/ponto/portas/celula.py:277`, `:285`, `:294`; autoridade preserva carimbo em `app/escala/alimentacao.py:279`.

**Vigia e candidata (b):** lê a autoridade existente; geradora carimbada inativa sem fim passa para `na_lista_do_admin_c6`, sem alterar a juíza ou a célula. A lista C6 usa o predicado existente e já enumera todos os vínculos inativos sem fim. O N exigido pelo aval antes da construção não está no dado fornecido: [nao verificado]. `app/ponto/services/vigia_cadastro_futuro.py:31`, `:40`; `app/escala/services/cadastro_realidade.py:320`; `app/escala/alimentacao.py:199`.

**Encerramento sem sucessor:** o novo ramo limpa `escala_geradora`, põe `trabalha=False`, recompõe DNA e grava os três campos de denúncia; `montar_dna` mantém `vinculo_id` do vínculo encerrado dentro do blob. O vigia conta esse estado em `sem_geradora`, fora dos dois contadores esperado-zero. Isso é comportamento comprovado, mas sua conformidade semântica não decorre literalmente do aval fornecido: lei: representação de célula futura existente cujo vínculo terminou sem sucessor, distinguindo proveniência e autoridade. `app/ponto/portas/celula.py:244`, `:249`, `:252`; `app/ponto/management/commands/gerar_celulas.py:95`; `app/ponto/services/vigia_cadastro_futuro.py:34`.

**Campos e semântica:**

- `data_inicio`/`data_fim` são inclusivos na cobertura e no ramo carimbado de `vinculo_do_dia`; não confundir `data_fim` com primeiro dia do sucessor. `app/escala/alimentacao.py:214`, `:282`.
- Vínculo sem vigência declarada = inativo sem fim; a cobertura o recusa, mas a juíza carimbada o devolve se não há violação explícita de início/fim, conforme candidata (b). `app/escala/alimentacao.py:189`, `:214`, `:279`.
- Janela do suplemento é a união das competências correntes das empresas ativas; o piso efetivo é amanhã ou `desde`, o que for maior. `app/ponto/management/commands/gerar_celulas.py:288`; `app/ponto/services/cadastro_futuro.py:18`.
- `dna.marcos` guarda HH:MM; `dna.lampadas[*].instante` guarda data e hora local naive. Nos turnos que cruzam meia-noite, horas anteriores à entrada ficam em D+1; a célula continua identificada pelo dia D do turno. `app/ponto/management/commands/gerar_celulas.py:36`, `:55`, `:67`, `:81`.
- `trabalha`, `escala_geradora`, `dna`, `dna_anterior`, `regenerada_em`, `regeneracoes`: escritor da alteração é a porta; leitores incluem autoridade de vínculo, métodos do vínculo e vigia. `app/ponto/portas/celula.py:294`; `app/escala/alimentacao.py:269`; `app/escala/models.py:1094`, `:1124`, `:1189`, `:1235`; `app/ponto/services/vigia_cadastro_futuro.py:28`, `:48`.
- `dna.lampadas` não é o carimbo de julgamento. Regeneração futura altera DNA e preserva `julgada_em`, `impressao`, `ata`, `veredito` e `status_dia`; o rabo da porta pula julgamento de datas futuras. Estado futura já carimbada permanece com julgamento anterior; regra de frescura aplicável a esse estado: [nao verificado]. `app/ponto/portas/celula.py:298`, `:448`, `:453`; campos de julgamento em `app/escala/models.py:1738`.
- O leitor de marcos ainda cai no template quando DNA sem marcos; leitores de pausas e extra declarada distinguem célula existente sem declaração e respondem vazio/zero. São contratos distintos existentes. `app/escala/models.py:1124`, `:1133`, `:1202`, `:1234`.

Não encontrei artefato local do diff específico ou dos hashes da obra pelo censo de nomes; atribuir cada mudança ao intervalo histórico exato permanece [nao verificado]. O RELATO documenta etapas anteriores, mas não substitui o diff: `app/docs/RELATO.md:39`, `:50`, `:60`.

## 12/11 15:25 · sessao 7f4b1508da7d · E3-10-CURA-NO-ESCRITOR · sonda · origem=Code

**PARA antes de qualquer apply:** zerar `respondida_em` deslocaria **142 perguntas do histórico para pendentes**; **col331/#22394 permaneceria no histórico**, porque sua disputa está fechada. A partição exige simultaneamente ausência de carimbo e disputa aberta. [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71) [nao verificado: estado de produção fornecido]

Não executei a sonda de produção: não há acesso a banco/logs, e `sonda_app.py` não foi localizado na cópia. Conferi a partição em memória usando a função extraída do código e os IDs fornecidos, sem criar, editar ou apagar arquivos. [nao verificado: execução local] [nao verificado]

**(1) Saída integral da sonda recebida**, reproduzida abaixo; não representa uma nova execução. [nao verificado]

```text
sonda_leitura: sonda_app.py [019c12f6f89be675] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T15:22:30.115249-03:00
universo: perguntas 143 colabs 17 erros 0
col60 alvo=1 pendentes=0 historico=1 fora_do_app=0 disputa_aberta=1 | app_total pend=1 hist=21 badge={'count': 1, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31436] pks_fora=[]
col191 alvo=17 pendentes=0 historico=17 fora_do_app=0 disputa_aberta=17 | app_total pend=43 hist=17 badge={'count': 43, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29167, 29168, 29169, 31724, 31725, 31726, 31727, 31728, 31729, 31730, 31731, 32909, 32910, 33593, 33594, 34182, 34183] pks_fora=[]
col204 alvo=4 pendentes=0 historico=4 fora_do_app=0 disputa_aberta=4 | app_total pend=14 hist=14 badge={'count': 14, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31584, 31585, 31586, 31587] pks_fora=[]
col224 alvo=9 pendentes=0 historico=9 fora_do_app=0 disputa_aberta=9 | app_total pend=44 hist=9 badge={'count': 44, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32291, 32292, 32293, 32294, 32295, 32296, 33925, 33926, 33927] pks_fora=[]
col310 alvo=6 pendentes=0 historico=6 fora_do_app=0 disputa_aberta=6 | app_total pend=18 hist=6 badge={'count': 18, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32243, 32244, 32245, 32246, 34141, 34142] pks_fora=[]
col331 alvo=1 pendentes=0 historico=1 fora_do_app=0 disputa_aberta=0 | app_total pend=30 hist=5 badge={'count': 30, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[22394] pks_fora=[]
col349 alvo=10 pendentes=0 historico=10 fora_do_app=0 disputa_aberta=10 | app_total pend=54 hist=12 badge={'count': 54, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29502, 29503, 32131, 32132, 32133, 32134, 32135, 32136, 33749, 33750] pks_fora=[]
col437 alvo=16 pendentes=0 historico=16 fora_do_app=0 disputa_aberta=16 | app_total pend=20 hist=18 badge={'count': 20, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29565, 32224, 32225, 32226, 32227, 32228, 32229, 32230, 32231, 32232, 32233, 32234, 32235, 32236, 32237, 32238] pks_fora=[]
col446 alvo=16 pendentes=0 historico=16 fora_do_app=0 disputa_aberta=16 | app_total pend=8 hist=33 badge={'count': 8, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29872, 32589, 32590, 32591, 32592, 32593, 32594, 32595, 32596, 32597, 32598, 32599, 32600, 32601, 33146, 33147] pks_fora=[]
col592 alvo=11 pendentes=0 historico=11 fora_do_app=0 disputa_aberta=11 | app_total pend=49 hist=13 badge={'count': 49, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[28357, 31708, 31709, 31710, 31711, 31712, 31713, 31714, 31715, 31716, 31717] pks_fora=[]
col607 alvo=3 pendentes=0 historico=3 fora_do_app=0 disputa_aberta=3 | app_total pend=270 hist=3 badge={'count': 270, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[29114, 31334, 31335] pks_fora=[]
col782 alvo=12 pendentes=0 historico=12 fora_do_app=0 disputa_aberta=12 | app_total pend=36 hist=19 badge={'count': 36, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32279, 32280, 32281, 32282, 32283, 32284, 32285, 32286, 32287, 32288, 32289, 32290] pks_fora=[]
col789 alvo=6 pendentes=0 historico=6 fora_do_app=0 disputa_aberta=6 | app_total pend=22 hist=31 badge={'count': 22, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31839, 31840, 31841, 31842, 33549, 33550] pks_fora=[]
col869 alvo=13 pendentes=0 historico=13 fora_do_app=0 disputa_aberta=13 | app_total pend=32 hist=13 badge={'count': 32, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[28443, 29270, 31863, 31864, 31865, 31866, 31867, 31868, 31869, 31870, 31871, 31872, 31873] pks_fora=[]
col893 alvo=12 pendentes=0 historico=12 fora_do_app=0 disputa_aberta=12 | app_total pend=136 hist=14 badge={'count': 136, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[32459, 32460, 32461, 32462, 32463, 32464, 32465, 32466, 33577, 33578, 34171, 34172] pks_fora=[]
col900 alvo=2 pendentes=0 historico=2 fora_do_app=0 disputa_aberta=2 | app_total pend=85 hist=2 badge={'count': 85, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[38251, 38252] pks_fora=[]
col903 alvo=4 pendentes=0 historico=4 fora_do_app=0 disputa_aberta=4 | app_total pend=14 hist=5 badge={'count': 14, 'cor': '#f59e0b', 'rotulo': 'Responder DP'}
   pks_historico=[31472, 31473, 31476, 31477] pks_fora=[]
TOTAL {'pend': 0, 'hist': 143, 'fora': 0, 'disputa_aberta': 142}
total_erros 0
```

**(2) Tabela por colaborador.** As quatro contagens centrais referem-se ao alvo das 143; as duas últimas reproduzem o total do app informado pela sonda. Todas correspondem ao instante fornecido, **12/11/2026 às 15:22:30 −03:00**. [nao verificado]

| Colab | Alvo | Alvo pendentes | Alvo histórico | Alvo fora do app | Alvo disputa aberta | App total pendentes | App total histórico | Fonte |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| col60 | 1 | 0 | 1 | 0 | 1 | 1 | 21 | [nao verificado] |
| col191 | 17 | 0 | 17 | 0 | 17 | 43 | 17 | [nao verificado] |
| col204 | 4 | 0 | 4 | 0 | 4 | 14 | 14 | [nao verificado] |
| col224 | 9 | 0 | 9 | 0 | 9 | 44 | 9 | [nao verificado] |
| col310 | 6 | 0 | 6 | 0 | 6 | 18 | 6 | [nao verificado] |
| col331 | 1 | 0 | 1 | 0 | 0 | 30 | 5 | [nao verificado] |
| col349 | 10 | 0 | 10 | 0 | 10 | 54 | 12 | [nao verificado] |
| col437 | 16 | 0 | 16 | 0 | 16 | 20 | 18 | [nao verificado] |
| col446 | 16 | 0 | 16 | 0 | 16 | 8 | 33 | [nao verificado] |
| col592 | 11 | 0 | 11 | 0 | 11 | 49 | 13 | [nao verificado] |
| col607 | 3 | 0 | 3 | 0 | 3 | 270 | 3 | [nao verificado] |
| col782 | 12 | 0 | 12 | 0 | 12 | 36 | 19 | [nao verificado] |
| col789 | 6 | 0 | 6 | 0 | 6 | 22 | 31 | [nao verificado] |
| col869 | 13 | 0 | 13 | 0 | 13 | 32 | 13 | [nao verificado] |
| col893 | 12 | 0 | 12 | 0 | 12 | 136 | 14 | [nao verificado] |
| col900 | 2 | 0 | 2 | 0 | 2 | 85 | 2 | [nao verificado] |
| col903 | 4 | 0 | 4 | 0 | 4 | 14 | 5 | [nao verificado] |
| **TOTAL alvo** | **143** | **0** | **143** | **0** | **142** | — | — | [nao verificado] |

**(3) `total_erros = 0`**, conforme a saída recebida. Não há erro reportado que acione a condição “medida NÃO conclusiva”; a execução de produção não foi verificada independentemente. [nao verificado]

**(4) Efeito de zerar somente `respondida_em`, mantendo os demais campos e fatos do instante:**

- `respondida_sem_texto` exige carimbo presente e texto vazio após `strip()`. Portanto, estar nesse predicado não significa estar em pendentes: a consulta do app testa o carimbo diretamente. [app/chamados/juizes.py:1510](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1510) [app/chamados/juizes.py:1515](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1515) [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71)
- `perguntas_do_app` seleciona pelo colaborador, por `q_vivas()` e por `pergunta_viva`. Como o texto desses alvos já é vazio, o ramo de vitalidade por resposta com texto já era falso; zerar o carimbo não altera esse resultado, preservados os demais fatos. [app/chamados/services/pergunta_no_app.py:45](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:45) [app/chamados/models.py:3349](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3349) [app/chamados/models.py:3360](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3360) [app/chamados/juizes.py:1407](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1407)
- A regra efetiva é `p.respondida_em is None and p.disputa.fechada_em is None`: verdadeira → pendentes; falsa → histórico. [app/chamados/services/pergunta_no_app.py:69](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:69)
- `projecao_do_app` usa essa consulta e calcula a contagem e o badge a partir das pendentes; a mudança alcança a apresentação do app. [app/chamados/services/pergunta_no_app.py:90](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:90) [app/chamados/services/pergunta_no_app.py:98](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:98)

A **lista exata das 142 que mudariam** é a união de todos os `pks_historico` publicados na saída acima, **excluindo somente col331/#22394**. Abaixo, os mesmos IDs explicitados como mudança **histórico → pendentes**, pela regra citada. [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71) [nao verificado: IDs e estado de produção fornecidos]

```text
col60: #31436
col191: #29167 #29168 #29169 #31724 #31725 #31726 #31727 #31728 #31729 #31730 #31731 #32909 #32910 #33593 #33594 #34182 #34183
col204: #31584 #31585 #31586 #31587
col224: #32291 #32292 #32293 #32294 #32295 #32296 #33925 #33926 #33927
col310: #32243 #32244 #32245 #32246 #34141 #34142
col349: #29502 #29503 #32131 #32132 #32133 #32134 #32135 #32136 #33749 #33750
col437: #29565 #32224 #32225 #32226 #32227 #32228 #32229 #32230 #32231 #32232 #32233 #32234 #32235 #32236 #32237 #32238
col446: #29872 #32589 #32590 #32591 #32592 #32593 #32594 #32595 #32596 #32597 #32598 #32599 #32600 #32601 #33146 #33147
col592: #28357 #31708 #31709 #31710 #31711 #31712 #31713 #31714 #31715 #31716 #31717
col607: #29114 #31334 #31335
col782: #32279 #32280 #32281 #32282 #32283 #32284 #32285 #32286 #32287 #32288 #32289 #32290
col789: #31839 #31840 #31841 #31842 #33549 #33550
col869: #28443 #29270 #31863 #31864 #31865 #31866 #31867 #31868 #31869 #31870 #31871 #31872 #31873
col893: #32459 #32460 #32461 #32462 #32463 #32464 #32465 #32466 #33577 #33578 #34171 #34172
col900: #38251 #38252
col903: #31472 #31473 #31476 #31477
```

**col331/#22394 permanece no histórico**: zerar o carimbo não satisfaz a condição de disputa aberta. Entre os 143 alvos, a partição passaria de **0 pendentes / 143 histórico / 0 fora** para **142 pendentes / 1 histórico / 0 fora**. Assim, o requisito do aval “apply só se o app não muda” não é atendido. [app/chamados/services/pergunta_no_app.py:71](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/pergunta_no_app.py:71) [nao verificado: estado de produção e aval fornecidos]

## 12/11 15:25 · sessao 09e311b52f21 · - · - · origem=Ronald (tmux)

A numeração abaixo corresponde à ordem do anexo. O resultado da suíte c4b e a comparação histórica exata com eb7bdd1a5 não foram verificados; a classificação descreve o mecanismo presente na cópia. [nao verificado]

| Falha | Classe / célula da matriz | Tratamento presente em c4b |
|---|---|---|
| 1. `Cancelar_na_caixa...` | Feriado salvo × futura gerada: incompatibilidade da representação da data recebida pelo evento. | **Adaptação local por `isinstance`**: ISO string vira `date.fromisoformat` antes da comparação; não muda o `DateField` nem o payload do signal. Elimina a operação `date < str`, mas mantém duas representações admitidas na entrada. `app/ponto/services/cadastro_futuro.py:42-49`; `app/core/signals.py:27`; `app/core/models.py:12`. |
| 2. `a_tela_de_feriado_novo...` | A mesma classe da falha 1; o erro ocorre no preparo do cenário, não no botão. [nao verificado] | A mesma conversão local de `app/ponto/services/cadastro_futuro.py:48`; um teste específico agora envia a string ISO pelo evento: `app/ponto/tests/test_cadastro_futuro_c4.py:146-152`. |
| 3. `Excluir_confirmado...` | A mesma classe da falha 1; o erro precede o clique de exclusão. [nao verificado] | A mesma conversão local: `app/ponto/services/cadastro_futuro.py:48`. O evento de apagar passa `instance.data` pelo mesmo serviço: `app/core/signals.py:30-33`. |
| 4. `Salvar_do_editar...` | A mesma classe da falha 1; futura gerada na data antiga/nova do feriado. | A mesma conversão local: `app/ponto/services/cadastro_futuro.py:48`. A data antiga é capturada no `pre_save` e as duas datas seguem no `post_save`: `app/core/signals.py:14-27`. |
| 5. `dna_congelado_sobrevive_edicao_de_template` | Template alterado × futura com carimbo de vínculo encerrado versus sucessor; conservação do passado. | **Mudou o cenário do teste**, não apenas o código de produção: agora primeiro exige que não reste futura nomeando o encerrado (`:163-164`), chama explicitamente o suplemento para o sucessor (`:165`) e fabrica um evento perdido por `TipoEscala.objects...update(...)` (`:169-170`). O `assertGreater` continua (`:177`); o teste depois verifica a porta `salvar_tipo` e a conservação do passado (`:183-201`). Portanto não é `skip`/allowlist/exceção, mas substituição da testemunha que produzia a divergência. `app/escala/tests/test_celula_dia.py:159-201`. |
| 6. `template_derivado_trilha_e_passado_intocado` | Encerrar com sucessor × futura anterior ao início do efeito, ainda coberta pelo vínculo antigo. | **Cura na origem do alcance**: `fechar_vigencia` começa no primeiro dia cuja cobertura mudou, `min(fim_antigo,fim_novo)+1`, em vez de regenerar toda futura do vínculo. O selo original continua comparando a foto de tudo anterior ao início. `app/colaboradores/services/vinculo.py:640-650`; `app/escala/tests/test_escala_auto_executor.py:69-73`. RED específico preserva até DNA antigo de um dia ainda coberto: `app/ponto/tests/test_cadastro_futuro_c4.py:133-144`. |
| 7. `ninguem_consulta_o_modelo_por_fora` | Feriado salvo × seleção das datas antiga/nova: propriedade da consulta ao cadastro. | **Centralização na origem autorizada da consulta**: signal chama `core.feriados.data_gravada`; a consulta reside nesse módulo. Não há allowlist nova no contrato, que continua excluindo apenas dono e CRUD. `app/core/signals.py:18-19`; `app/core/feriados.py:86-93`; `app/core/tests/test_contract_feriado.py:27-30,58-64`. |
| 8. `o_censo_que_IMPEDE...` | Cron vigia × futura carimbada: geradora usada como resposta local, fora do juiz. | **Cura do uso na origem**: vigia entrega geradora e célula a `vinculo_do_dia`, testa ausência e usa a resposta `ec`; não compara mais a resposta com o carimbo. `app/ponto/services/vigia_cadastro_futuro.py:28-49`. `TOMADORES` continua vazio, sem adicionar o vigia: `app/escala/tests/test_contract_juiz_escala.py:69-83`. |
| 9. `sitio_novo_e_lista_velha...` | Cron × célula gerada de hoje: escolha do vínculo por `ativa=True`. | **Cura na origem**: `_regenerar_hoje_existentes` usa `vinculos_do_dia_carregado`, depois a mesma porta da célula. `app/ponto/management/commands/gerar_celulas.py:303-316`. Não foi adicionado à lista `HOJE`; o teste continua exigindo exatamente o sítio artificial na lista de novos. `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:417-422`. |
| 10. `a_arvore_nao_tem_sitio_novo...` | Mesma classe da falha 9; é a fiscalização da árvore, em vez da mordida do instrumento. | Mesma cura de `app/ponto/management/commands/gerar_celulas.py:303-316`; contrato permanece exigindo listas novas/velhas vazias: `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:364-369`. |
| 11. `vulture_zero` | Porta do admin de template × assinatura do callback; questão de instrumento, sem transição de célula própria. | **Convenção `_form` para parâmetro não usado**, na própria assinatura; não elimina o parâmetro, pois o callback o recebe. Não há inclusão de `form` no ignore: continuam os seis nomes de framework já enumerados. `app/escala/admin.py:11-14`; `app/core/tests/test_contract_esmeril.py:13-15,26-29`. |

Observação para a matriz: as quatro falhas do feriado não fornecem quatro provas independentes de transição; o anexo mostra o mesmo `TypeError` durante `Feriado.objects.create`. Não demonstram o comportamento dos cliques após o preparo. [nao verificado]

O tratamento do vínculo inativo sem fim está explícito e não altera o juiz: o vigia o conta em `na_lista_do_admin_c6`, deixando os dois contadores de estrutura em zero; o juiz continua aceitando a geradora carimbada dentro das datas, enquanto `tem_vigencia_declarada` a recusa para regeneração. Isso materializa a candidata (b), não a candidata (a). `app/ponto/services/vigia_cadastro_futuro.py:31-45`; `app/escala/alimentacao.py:189-196,270-286`; `app/ponto/tests/test_cadastro_futuro_c4.py:183-200`.

O N de vínculos em produção que deveria ter sido publicado antes da construção não aparece no dado raspado e não foi medido nesta leitura. [nao verificado] [nao verificado]

## 12/11 15:24 · sessao 7fb82434cf82 · - · - · origem=Ronald (tmux)

Censo PRE-cura salvo em `docs/lastro_c18_censo.txt`. Único arquivo alterado por este agente; referências preservam as linhas da base c17.

## 12/11 15:23 · sessao 7fb82434cf82 · - · - · origem=Ronald (tmux)

Censo somente leitura, referências da base c17 antes da edição. Nenhuma afirmação sobre frota/banco/log foi verificada.

**Escritores da grade e unidade**

| Sítio | Unidade e operação |
|---|---|
| `escala/utils.py:621`, `:662` | `_match_marcos`: instante absoluto → lista HH:MM; dois pontos originais de perda da data. |
| `escala/utils.py:762`, `:1008`, `:1037`, `:1826` | Inicializam dia sem órfãs: lista vazia. |
| `escala/utils.py:776`, `:788` | Builder simples recebe e publica HH:MM de `_match_marcos`. |
| `escala/utils.py:799` | Sobra: `locs[_k][0]` absoluto → HH:MM anexada ao dia. |
| `escala/utils.py:803`, `:1862` | Ordenam a lista HH:MM; lista paralela deverá acompanhar a mesma permutação. |
| `escala/utils.py:963`, `:967` | Clip da janela: apagam órfãs de dia futuro/anterior ao piso. |
| `escala/utils.py:1046` | Legacy remapeia/copía `sd['orfas']` HH:MM. |
| `escala/utils.py:1156` | Overlay de folga apaga a lista. |
| `escala/utils.py:1234`, `:1277`, `:1282` | Falta, ausência integral e justificativa apagam a lista. |
| `escala/utils.py:1462`, `:1591`, `:1782` | Mapa intermediário por data: inicialização, dia intermitente vazio, lista HH:MM retornada pelo alinhamento. |
| `escala/utils.py:1725`, `:1746`, `:1757` | Builder por turno recebe HH:MM; ponta sem coluna e intervalo excedente acrescentam `celula['hora']` HH:MM. Aqui também é necessário preservar o instante antes da perda. |
| `escala/utils.py:1835` | Publica cópia HH:MM do mapa na linha do dia. |
| `escala/utils.py:1856`, `:1858` | Sobras sem slot/anônimas: instante `locs[_k][0]` → HH:MM. |
| `escala/utils.py:1993`, `:1998`, `:2021`, `:2064` | Reconciliação cria aliases `_orf_t`, `_orf_saida`, `_lista_orf` e remove uma ocorrência HH:MM da lista original. A remoção correspondente do instante é obrigatória. |

**Escritores de ata e lavra/relavra**

| Sítio | Unidade e operação |
|---|---|
| `ponto/services/cartorio.py:729`, `:733`, `:736` | `ata_do_dia`, produtor persistido: `n_orfas` é contagem; `orfas` é lista HH:MM convertida por `str`. |
| `ponto/services/cartorio.py:1247`, `:1289` | Caminho NB chama o mesmo `ata_do_dia`, aplica dispensa e encaminha para lavra. |
| `ponto/services/cartorio.py:1432`, `:1433` | Ata de julgamento em memória chama o mesmo produtor com dispensa/teto. |
| `ponto/services/cartorio.py:1847`, `:1849` | Lavra/relavra normal chama o mesmo produtor, dispensa/teto e `lavrar_veredito(ata=...)`. |
| `escala/utils.py:1912`, `:1920` | **Outro escritor de objeto `.ata`, temporário em memória:** cópia da célula usada pelo juiz de realizado recebe `lampadas` e `orfas` HH:MM. Não persiste. Deve ser citado no censo; não é o escritor persistido do cartório. |

**Leitores principais**

| Sítio | Unidade lida/usada |
|---|---|
| `escala/services/leitor_celula.py:180`, `:187`, `:190` | `_orfas_da_ata`: lista HH:MM; ata sem horas vira `['?'] * n_orfas`, preservando contagem/ignorância. |
| `escala/services/leitor_celula.py:390`, `:420` | Inicializa lista vazia e transporta as horas da ata para o dia. |
| `escala/utils.py:427` | Chama `_match_marcos`, mas descarta o retorno `_orfas`. |
| `escala/utils.py:1113` | `montar_realizado`: repassa cópia HH:MM, sem reinterpretar. |
| `ponto/services/espelho.py:699`, `:702`, `:854` | Recebe/publica HH:MM como `orfas_grade`; ausência de montagem → lista vazia. |
| `colaboradores/services/calendario.py:380`, `:381`, `:622` | Converte HH:MM em conjunto por dia/mês e consulta para desenhar a batida. |
| `ponto/services/cartorio.py:590`, `:619`, `:727` | `lampadas_do_dia`: HH:MM → minutos do dia para lâmpadas; não lê instante de órfã. |
| `ponto/services/cartorio.py:733`, `:836`, `:1422` | Contagem/presença: `len(orfas)` e `n_orfas`. |
| `relatorios/pdf_espelho.py:477`, `:478`, `:482`, `:485` | S1 c17 inventa `(data da linha, HH:MM)` e casa com timestamp formatado; chave a eliminar. |
| `relatorios/pdf_espelho.py:174`, `:178`, `:522`, `:574` | Recebem conjunto de instantes: identidade da entrada espúria, supressão de turno e marca na batida. |
| `templates/ponto/espelho.html:151`, `:240` | Exibem diretamente cada HH:MM de `orfas_grade`. Nenhum outro acesso de template a essas chaves apareceu na busca. |

**Demais leitores HH:MM**

- `relatorios/cartao_pela_celula.py:288`: conjunto de horas da ata por data.
- `ponto/juiz_batida.py:161`, `:164`: copia as horas para o contrato de períodos.
- `ponto/services/cluster_espurio.py:29`, `:31`, `:34`: valida lista, converte HH:MM em minutos e detecta rajada; `:206`, `:210` usa as horas em texto/contexto.
- `ponto/services/vigia_pareador_celula.py:74`, `:78`, `:111`: conjunto HH:MM da ata, compara a hora da batida.
- `ponto/services/esmeril_espelho.py:125`: HH:MM → minutos/faixas de meia hora.
- `ponto/turnos.py:2667`: HH:MM → minutos do dia para cobertura parcial.
- `ponto/propositor.py:39`, `:53`, `:55`: valida e ordena horas reais em minutos.
- `escala/orfa_classe.py:58`, `:74`: lista HH:MM, classificação/rajadas.
- `ponto/management/commands/propor_reorganizacao.py:104`: acrescenta horas órfãs às marcas reais.
- `ponto/management/commands/tripwire_celulas.py:165`, `:264`: horas da ata → minutos, comparadas aos marcos.
- `chamados/services/materializacao.py:180`: acrescenta horas órfãs às marcas usadas pelo propositor.

**Demais leitores de contagem/presença**

- `escala/services/furos_diarios.py:22`: `len(orfas)`.
- `ponto/status_dia.py:188`: contagem de batidas = casadas + órfãs.
- `ponto/supra_juiz.py:379`: presença de fato; `:514`, `:517`, `:520`: presença, contagem e texto das horas.
- `folha/causas.py:43`, `:100`: presença de órfãs nos dias úteis/folgas.
- `ponto/services/bordas_realizado.py:49` e `ponto/services/simular_dia.py:12`: presença.
- `ponto/management/commands/detectar_par_relampago.py:140`: soma contagens.
- `chamados/juizes.py:510`: presença de `orfas` ou `n_orfas`.
- `chamados/detector_anomalias.py:212`: recebe `_orfas` de `_match_marcos`; retorno não é consumido naquele sítio.

**Juiz existente para o contador**

Use o inverso de `folha/export.py:597::competencia_fechada_para_celula`, passando `empresa_id`, `mes`, `ano` e `colaborador_id`. A autoridade documenta aberta como lista de fontes vazia; compõe exportação vigente e holerite publicado (`folha/export.py:614`, `:618`). O enumerador existente `_competencias_exportadas` já usa esse juiz e converte competência em janela (`ponto/management/commands/gerar_celulas.py:147`, `:152`, `:173`, `:176`).

Não deduzir abertura por `FechamentoMensal.status`: a própria guarda rejeita essa equivalência (`ponto/management/commands/gerar_celulas.py:131`).

_4323 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
