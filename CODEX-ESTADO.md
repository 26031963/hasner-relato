# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 17:09** · publicado em **13/11/2026 17:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 17:09 · sessao 234786b384a9 · E3-FORMA-SQL · leitura · origem=Code

**A recontagem fecha em 170 colab-dias: 62 ESTRUTURA, 34 CADASTRO e 74 BATIDA. Há 21 com ação humana e FUTURO=`nada`. Não há, pelos casos escritos solicitados e pelos fatos apresentados, colab-dia demonstrado como ATUAL obrigatório.** (`a1v3_det.txt:1–170`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

Referências abreviadas:

- **D:n** = `a1v3_det.txt:n`, contando somente as linhas `V3_DET` coladas, desde 1.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **P:n** = [app/chamados/management/commands/diff_e3_sombra.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py):n.

**1. Grupo × dono L-099**

Contagens calculadas sobre as 170 linhas fornecidas; os donos são os registrados no anexo. (D:1–170)

| Grupo | ESTRUTURA | CADASTRO | BATIDA | None | Total |
|---|---:|---:|---:|---:|---:|
| G2 | 17 | 10 | 41 | 0 | 68 |
| G3 | 18 | 3 | 9 | 0 | 30 |
| G4 | 26 | 16 | 23 | 0 | 65 |
| G8 | 1 | 5 | 1 | 0 | 7 |
| **Total** | **62** | **34** | **74** | **0** | **170** |

**2. Combinações de razões e cobertura pela regra escrita**

O teste “só fila sem pergunta” exige uma instância com ação `perguntar/sistema/emissor`; os testes de porta humana exigem âncora concreta na própria célula ou sob B. O caso de isenção exige `isento_ponto`. Nenhum desses cenários pode ser identificado integralmente apenas pela ausência de porta no anexo. (T:21–47,57–82) [nao verificado]

Nas tabelas, **SC = “sem caso escrito”; escolha ATUAL/FUTURO não determinada por esse arquivo**. As letras seguintes identificam a lacuna:

| Código | Lacuna de cobertura ou fato faltante |
|---|---|
| **F** | Falta caso escrito para a combinação apresentada com fila/porta residual. Não reproduz o teste “sem pergunta + perguntar/sistema/emissor”, nem o de isenção. (T:32–38,57–65) | [nao verificado]
| **H** | Há ação humana de pergunta, mas faltam **porta da ação, identidade/localização da âncora e presença na fila global** para aplicar o caso da própria célula ou A/B. O anexo registra somente `[acao,de]`. (D:1–170; T:21–47,67–82) |
| **U** | Falta caso nesse arquivo para `ausencia_a_decidir`, inclusive sua combinação com `aceso/nao_venceu`. A ação não comprova porta de chamado: o código a cria com porta ausente. (T:21–127; S:114–115,283–291) |
| **C** | Falta caso nesse arquivo para `celula_acusa_sem_pergunta_viva` com `validar/admin`; o código cria essa ação sem porta. (T:21–127; S:329–336) |

As combinações abaixo **preservam a multiplicidade**, desconsiderando apenas a ordem. Exemplo: `A³P` significa três instâncias `aceso` e uma `passivo_sem_linha`. Todas as contagens foram calculadas sobre D:1–170; cada exemplo tem sua linha individual.

Legenda:

`A=aceso`; `P=passivo_sem_linha`; `R=aguarda_resposta`; `N=nao_venceu`; `U=ausencia_a_decidir`; `V=veredito_sem_lavra_vigente`; `E=efeito_sem_lampada`; `D=dia_sem_trabalho`; `C=celula_acusa_sem_pergunta_viva`; `L=aguarda_validacao`; `J=encerrada_pelo_juiz`.

| Grupo G2 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| A³P | 17 | 120067 (D:1), 118928 (D:6) | SC/F |
| A²P² | 7 | 119985 (D:2), 108826 (D:56) | SC/F |
| AP | 15 | 140114 (D:21), 117245 (D:55) | SC/F |
| AP³ | 1 | 108035 (D:129) | SC/F |
| P⁴ | 3 | 108622 (D:13), 109494 (D:42) | SC/F |
| A⁴ | 3 | 129083 (D:47), 129090 (D:48) | SC/F |
| A³R | 4 | 118930 (D:7), 118934 (D:11) | SC/H |
| A²R² | 5 | 118932 (D:9), 105773 (D:17) | SC/H |
| AR³ | 2 | 129598 (D:85), 116554 (D:104) | SC/H |
| AP²R | 1 | 118931 (D:8) | SC/H |
| AV | 3 | 140121 (D:22), 140122 (D:23) | SC/F |
| A³V | 1 | 130771 (D:131) | SC/F |
| A³E | 1 | 130929 (D:89) | SC/F |
| U⁴ | 1 | 138643 (D:98) | SC/U |
| U² | 1 | 129575 (D:127) | SC/U |
| CD³ | 1 | 120319 (D:150) | SC/C |
| A³L | 1 | 141817 (D:166) | SC/H |
| AJ | 1 | 142013 (D:167) | SC/F |
| **Total** | **68** | D:1–170 | |

| Grupo G3 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| A³R | 2 | 141714 (D:3), 141810 (D:165) | SC/H |
| A³P | 2 | 115432 (D:33), 137166 (D:84) | SC/F |
| A⁴P | 1 | 135811 (D:50) | SC/F |
| A³N | 2 | 132340 (D:34), 127949 (D:130) | SC/F |
| A²N² | 1 | 133374 (D:39) | SC/F |
| AN³ | 1 | 130294 (D:61) | SC/F |
| N⁴ | 2 | 129189 (D:36), 130418 (D:145) | SC/F |
| A⁴ | 5 | 136215 (D:43), 137154 (D:53) | SC/F |
| A² | 11 | 121500 (D:57), 138912 (D:95) | SC/F |
| U² | 1 | 117062 (D:54) | SC/U |
| U⁴ | 1 | 112501 (D:115) | SC/U |
| ANU² | 1 | 127887 (D:159) | SC/U |
| **Total** | **30** | D:1–170 | |

| Grupo G4 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| A³P | 12 | 130160 (D:4), 130162 (D:5) | SC/F |
| A²P² | 10 | 118933 (D:10), 108633 (D:14) | SC/F |
| AP² | 1 | 139432 (D:20) | SC/F |
| AP³ | 3 | 111346 (D:40), 122073 (D:142) | SC/F |
| AP | 9 | 119517 (D:28), 115650 (D:35) | SC/F |
| P⁴ | 10 | 140928 (D:26), 104784 (D:27) | SC/F |
| P² | 4 | 116436 (D:52), 124364 (D:101) | SC/F |
| A⁴ | 11 | 140331 (D:29), 130931 (D:90) | SC/F |
| A² | 5 | 135067 (D:46), 110506 (D:58) | SC/F |
| **Total** | **65** | D:1–170 | |

| Grupo G8 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| N⁴ | 1 | 131162 (D:30) | SC/F |
| D⁴ | 1 | 116182 (D:41) | SC/F |
| A³P | 1 | 113307 (D:62) | SC/F |
| DP | 2 | 130346 (D:117), 130348 (D:118) | SC/F |
| P⁴ | 2 | 128533 (D:155), 127477 (D:160) | SC/F |
| **Total** | **7** | D:1–170 | |

Não aparecem razões `lampada_nao_sabe` nem `ausencia_aprovada` nestas 170 linhas. Existem instâncias com `lampada=null`, mas nelas a razão apresentada é `passivo_sem_linha`; `null` não autoriza renomear a razão. (D:1–170, particularmente D:20,50,122)

**3. ATUAL obrigatório + dono ESTRUTURA**

**Lista comprovada: vazia.** Isso significa **zero demonstrados**, não prova de que nenhum defeito exista: falta cobertura escrita para as combinações ou o fato da âncora exigido pelos testes. (T:21–127; D:1–170) [nao verificado]

Os **sete ESTRUTURA com ação humana e FUTURO=`nada`** são os seguintes. Eles não podem ser promovidos automaticamente a “ATUAL obrigatório”. (D:8–9,17,98,115,127,150; T:40–47,67–73)

| Grupo | celula_pk — lista completa dessa condição | Código que produz a redução ao FUTURO |
|---|---|---|
| G2 | 118931 (D:8), 118932 (D:9), 105773 (D:17) | A ação recebe âncora somente se ela estiver na fila; publicação exige porta inteira pertencente à fila global. Sem rota, retorna `None`, projetado como `nada`. (S:128–133,547–565; P:49) |
| G2 | 138643 (D:98), 129575 (D:127) | `decidir_ausencia/admin` nasce sem porta; depois passa pela mesma redução. (S:114–115,283–291,547–565; P:49) |
| G2 | 120319 (D:150) | `validar/admin` da acusação sem pergunta nasce sem porta; depois passa pela mesma redução. (S:335–336,547–565; P:49) |
| G3 | 112501 (D:115) | `decidir_ausencia/admin` nasce sem porta; depois passa pela mesma redução. (S:114–115,283–291,547–565; P:49) |

**4. CADASTRO e BATIDA — lista do admin**

| Grupo | CADASTRO | BATIDA | Total admin |
|---|---:|---:|---:|
| G2 | 10 | 41 | 51 |
| G3 | 3 | 9 | 12 |
| G4 | 16 | 23 | 39 |
| G8 | 5 | 1 | 6 |
| **Total** | **34** | **74** | **108** |

Contagens: D:1–170. A separação CADASTRO/BATIDA para pauta, com ESTRUTURA como fatia, está escrita em `app/ponto/services/pautas_dos_discordantes.py:40–41`.

**5. FUTURO=`nada` com ação humana**

**Sim: 21 colab-dias, sendo G2=16, G3=5 e G4/G8=0.** A lista completa é esta. (D:1–170)

| Grupo | Ação/de | celula_pk | Escrita da ação |
|---|---|---|---|
| G2 | responder/colaborador | 118930 (D:7), 118931 (D:8), 118932 (D:9), 118934 (D:11), 105773 (D:17), 106286 (D:78), 129598 (D:85), 116546 (D:103), 116554 (D:104), 116542 (D:112), 103972 (D:114), 105151 (D:148) | S:130–133,308–313 |
| G2 | decidir_ausencia/admin | 138643 (D:98), 129575 (D:127) | S:283–291 |
| G2 | validar/admin | 120319 (D:150) | S:335–336 |
| G2 | validar/admin | 141817 (D:166) | S:135–138,308–313 |
| G3 | responder/colaborador | 141714 (D:3), 141810 (D:165) | S:130–133,308–313 |
| G3 | decidir_ausencia/admin | 117062 (D:54), 112501 (D:115), 127887 (D:159) | S:283–291 |

**Essa observação não demonstra contradição interna da tarefa:** seu `fio` resume os donos das ações, mesmo sem porta; a função posterior `porta_da_tarefa` devolve `None` sem rota, e a projeção da folha chama isso de `nada`. O código declara expressamente que ausência de porta não significa ausência de tarefa. (S:540–565; P:49–59)

Há inclusive teste escrito admitindo `responder/colaborador` com `porta=None` quando o chamado está silenciado. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

**6. Quem escreve, quem lê e significado dos campos**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| **razao** | `_tarefa_pura` inicializa e escreve por ramo; `_pela_decisao` fornece as razões relativas à pergunta; `expor_admin` pode substituí-las. (S:118–163,249–260,262–313,318–336) | A publicação da porta não consulta razão de instância. O diff consulta somente a razão **geral da tarefa** para classificar isenção. O backfill reconhece `passivo_sem_linha` e agrega razões para explicar o resultado. (S:547–556; P:63–70; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`) |
| **lampada** | `instancias_do_turno` copia `acesa_lavrada`; `_tarefa_pura` copia para a instância publicada. (S:87–105,258–260) A origem é `ata.lampadas[].acesa`, preservada pelo leitor único. (`app/escala/services/leitor_celula.py:134–137`) A ata é montada pelo cartório. (`app/ponto/services/cartorio.py:672–678,711–738`) | A tarefa distingue `True`, `None` e o ramo restante, normalmente `False`. `True` dispensa antes do teto; `None` produz `lampada_nao_sabe`; depois são examinados os valores de `vencido`. (S:262–282) |
| **vencido** | O cartório calcula `quando + 30 minutos <= agora`; a carga fornece esse resultado, que é casado por tipo/hora e copiado para a instância. (`app/ponto/services/cartorio.py:862,882–893`; `app/ponto/turnos.py:3060–3066`; S:74–105,506–514) | A tarefa interpreta `None` como teto desconhecido e `False` como ainda não vencido. O teste ocorre depois da dispensa por dia sem trabalho/lâmpada acesa. (S:262–282) |
| **acoes** | `_acao` define `{acao,de,porta}`, com porta padrão `None`; decisões, ausência, emissão de pergunta e acusação da célula criam as ações. (S:114–162,283–313,335–336) | `_portas_da_tarefa` reúne as ações, publica somente portas inteiras presentes na fila global e calcula o fio pelos donos. O backfill também lê ações e reconhece humanos sem porta. (S:547–556; `app/chamados/management/commands/backfill_fio_morto_e4b.py:100–115`) |

**`lampada=None` não equivale a `False`.** O leitor único converte `None` em acesa para seu campo `missing`, mas mantém o valor original em `acesa_lavrada`, que é o campo usado pela tarefa. Não há essa conversão na tarefa. (`app/escala/services/leitor_celula.py:119–137`; S:87–105,272–282)

**`vencido` é por instante do marco encadeado no turno, incluindo travessia da meia-noite; não é comparação por dia civil.** `data_civil` é extraída separadamente do instante casado. (`app/ponto/services/cartorio.py:865–893`; S:74–105)

**`porta_da_tarefa` e as projeções do diff não reinterpretam `lampada`, `vencido` ou razão de instância: não leem esses campos.** A redução acontece na publicação da porta e descarta o fio humano quando não há rota. (S:547–565; P:46–59)

Há, porém, **diferença entre descrição e predicado na pauta**: o texto diz “não estão esperando NINGUÉM”, enquanto o código seleciona ausência de porta, condição compatível com os humanos sem porta acima. A folha documenta o significado mais estreito: nenhuma ação humana exigível passa por uma porta da fila. (`app/ponto/services/pautas_dos_discordantes.py:46–56,79–84`; `app/folha/fora_da_folha.py:52–58,77–89`; S:547–565)

## 13/11 17:07 · sessao 04d6f45b7ddb · - · - · origem=Ronald (tmux)

Recontagem exata das 170 linhas `V3_DET`, numeradas desde 1:

| Grupo | ESTRUTURA | CADASTRO | BATIDA | None | Total |
|---|---:|---:|---:|---:|---:|
| G2 | 17 | 10 | 41 | 0 | 68 |
| G3 | 18 | 3 | 9 | 0 | 30 |
| G4 | 26 | 16 | 23 | 0 | 65 |
| G8 | 1 | 5 | 1 | 0 | 7 |
| Total | 62 | 34 | 74 | 0 | 170 |

Fonte: `a1v3_det.txt:1–170` do texto fornecido.

Combinações abaixo usam **conjunto de razões distintas**, sem distinguir multiplicidade.

| Grupo | Razões | n | Exemplos |
|---|---|---:|---|
| G2 | aceso + passivo_sem_linha | 40 | 120067 (:1), 119985 (:2) |
| G2 | aceso + aguarda_resposta | 11 | 118930 (:7), 118932 (:9) |
| G2 | aceso + passivo_sem_linha + aguarda_resposta | 1 | 118931 (:8) |
| G2 | passivo_sem_linha | 3 | 108622 (:13), 109494 (:42) |
| G2 | aceso + veredito_sem_lavra_vigente | 4 | 140121 (:22), 140122 (:23) |
| G2 | aceso | 3 | 129083 (:47), 129090 (:48) |
| G2 | aceso + efeito_sem_lampada | 1 | 130929 (:89) |
| G2 | ausencia_a_decidir | 2 | 138643 (:98), 129575 (:127) |
| G2 | celula_acusa_sem_pergunta_viva + dia_sem_trabalho | 1 | 120319 (:150) |
| G2 | aceso + aguarda_validacao | 1 | 141817 (:166) |
| G2 | aceso + encerrada_pelo_juiz | 1 | 142013 (:167) |
| G3 | aceso + aguarda_resposta | 2 | 141714 (:3), 141810 (:165) |
| G3 | aceso + passivo_sem_linha | 3 | 115432 (:33), 135811 (:50) |
| G3 | aceso + nao_venceu | 4 | 132340 (:34), 133374 (:39) |
| G3 | nao_venceu | 2 | 129189 (:36), 130418 (:145) |
| G3 | aceso | 16 | 136215 (:43), 137154 (:53) |
| G3 | ausencia_a_decidir | 2 | 117062 (:54), 112501 (:115) |
| G3 | aceso + nao_venceu + ausencia_a_decidir | 1 | 127887 (:159) |
| G4 | aceso + passivo_sem_linha | 35 | 130160 (:4), 130162 (:5) |
| G4 | passivo_sem_linha | 14 | 140928 (:26), 104784 (:27) |
| G4 | aceso | 16 | 140331 (:29), 135067 (:46) |
| G8 | nao_venceu | 1 | 131162 (:30) |
| G8 | dia_sem_trabalho | 1 | 116182 (:41) |
| G8 | aceso + passivo_sem_linha | 1 | 113307 (:62) |
| G8 | dia_sem_trabalho + passivo_sem_linha | 2 | 130346 (:117), 130348 (:118) |
| G8 | passivo_sem_linha | 2 | 128533 (:155), 127477 (:160) |

Todas as referências `:n` acima são `a1v3_det.txt:n`. Não aparecem razões `lampada_nao_sabe` nem `ausencia_aprovada` nas 170 linhas. (`a1v3_det.txt:1–170`)

Ações humanas com FUTURO `nada`: **21 colab-dias**, G2=16, G3=5, G4=G8=0. (`a1v3_det.txt:1–170`)

| Grupo / ação | Lista completa |
|---|---|
| G2 responder | 118930 (:7), 118931 (:8), 118932 (:9), 118934 (:11), 105773 (:17), 106286 (:78), 129598 (:85), 116546 (:103), 116554 (:104), 116542 (:112), 103972 (:114), 105151 (:148) |
| G2 decidir_ausencia | 138643 (:98), 129575 (:127) |
| G2 validar | 120319 (:150), 141817 (:166) |
| G3 responder | 141714 (:3), 141810 (:165) |
| G3 decidir_ausencia | 117062 (:54), 112501 (:115), 127887 (:159) |

Desses, **7 têm dono ESTRUTURA**: G2=118931 (:8), 118932 (:9), 105773 (:17), 138643 (:98), 129575 (:127), 120319 (:150); G3=112501 (:115). Isso identifica fatos e donos; não determina que ATUAL seja correto. (`a1v3_det.txt:8–150`, referências individuais na lista)

Lista admin CADASTRO+BATIDA: G2=51, G3=12, G4=39, G8=6, total=108. (`a1v3_det.txt:1–170`)

## 13/11 17:06 · sessao fa8f1832d687 · - · - · origem=Ronald (tmux)

Leituras e escritores encontrados, todos na cópia:

| Campo de instância | Quem escreve | Quem lê e significado |
|---|---|---|
| `lampada` | `app/chamados/services/tarefa_do_dia.py:104–105` copia o valor de `acesa_lavrada`, obtido em `:87–91`. O leitor único copia `ata.lampadas[].acesa` diretamente em `app/escala/services/leitor_celula.py:134–137`. A ata é montada em `app/ponto/services/cartorio.py:730–738`, com valores True/None/False em `:672–678,711–724`; o teto pode trocar False por None em `:907–917`. | A tarefa distingue True (`tarefa_do_dia.py:262–271`), None (`:272–275`) e False que segue ao teto (`:276–282`). O outro campo `missing` do leitor trata None como acesa (`leitor_celula.py:119–120`), mas **a tarefa usa `acesa_lavrada`**, preservando o triestado (`:134–137`). |
| `vencido` | `app/ponto/services/cartorio.py:892–893`: `quando + timedelta(minutes=teto_min) <= agora`, com default de 30 min em `:862`. A carga usa esse corpo em `app/ponto/turnos.py:3060–3066`; a tarefa chama com `self.agora` e copia o resultado em `tarefa_do_dia.py:506–514,104–105`. | `tarefa_do_dia.py:276–282`: None = marco sem teto conhecido; False = ainda não venceu. O cálculo usa **instante do marco encadeado no turno**, não comparação do dia civil (`cartorio.py:865–868,882–893`). |
| `razao` | Inicialização e cópia em `tarefa_do_dia.py:258–260`; ramificações escrevem em `:263,273,277,281,285–290,296,303,309`. `_pela_decisao` fornece razões de pergunta em `:130–163`. Exposição administrativa escreve em `:249–255`; instâncias adicionais em `:318–323`; `celula_acusa_sem_pergunta_viva` em `:335–336`. | Não determina a publicação da porta: `_portas_da_tarefa` não a consulta (`:547–556`). Leitor operacional adicional `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–99` reconhece `passivo_sem_linha`; `:116–117` agrega razões para explicar ausência de ato. O diff usa apenas **razão da tarefa**, para reconhecer isenção (`diff_e3_sombra.py:70`). |
| `acoes` | Formato e default de porta em `tarefa_do_dia.py:114–115`; decisões em `:130–162`; ausência em `:286–291`; perguntar em `:300,304`; cópia da decisão em `:308–313`; exposição admin em `:253–255`; acusação sem pergunta em `:335–336`. | `_portas_da_tarefa` achata ações em `:547`, publica só porta inteira ∩ fila global em `:548–551`, e calcula **fio da tarefa pelos donos das ações** em `:552–556`. Backfill lê ações e reconhece humanos sem porta em `backfill_fio_morto_e4b.py:100–115`. |

**Ação humana com FUTURO=`nada` não significa que o fio interno da tarefa seja `nada`.** A tarefa calcula `fio='colab'/'admin'` pelos donos mesmo sem porta inteira; a projeção posterior pode descartá-lo ao retornar None. Isso é explícito no comentário “ausência de porta não diz ausência de tarefa”. (`app/chamados/services/tarefa_do_dia.py:540–556`)

Cadeia exata que produz esse resultado:

1. `responder/validar` só recebem âncora inteira se ela estiver na fila particular carregada: `tarefa_do_dia.py:128–140`; a carga dessa fila está em `:415–418`.
2. `decidir_ausencia/admin` pode nascer diretamente com porta None: `:114–115,283–291`.
3. `celula_acusa_sem_pergunta_viva` produz `validar/admin` com porta None: `:335–336`.
4. Só `porta` inteira pertencente a `alimentacao['fila_ids']` entra na publicação: `:547–548`.
5. Sem essas portas, `rota=None`; o fio interno ainda preserva o humano: `:552–556`.
6. `porta_da_tarefa` devolve None quando a rota é None: `:559–565`.
7. O diff converte None em folha `fio='nada'`, vigia `fio=None/exige_admin=False`, beco `sem_porta=True`, pauta `sem_fio=True`: `app/chamados/management/commands/diff_e3_sombra.py:46–59`.

**Porta e projeções não reinterpretam `lampada`, `vencido` nem razão por instância:** elas não leem esses campos. A redução perde essas informações e também o fio humano quando não há rota. (`tarefa_do_dia.py:547–565`; `diff_e3_sombra.py:46–59`)

Há diferença entre a descrição textual da pauta — “não estão esperando NINGUÉM” — e o predicado executado, que testa ausência de porta por meio de `_fio_do_dia`. Esse predicado admite ações humanas sem porta. (`app/ponto/services/pautas_dos_discordantes.py:46–56,79–84`; `app/folha/fora_da_folha.py:77–89`; `tarefa_do_dia.py:283–291,547–565`)

A folha documenta a interpretação mais estreita: nenhuma ação exigível **passa por uma porta da fila**, e seu texto de `nada` diz que alguém precisa olhar. (`app/folha/fora_da_folha.py:52–58`)

## 13/11 17:05 · sessao c41b2ea51ab4 · - · - · origem=Ronald (tmux)

Em `test_e3_portas_tarefa.py`, nenhum caso cobre explicitamente as combinações `aceso`, `nao_venceu`, `lampada_nao_sabe`, `passivo_sem_linha`, `veredito_sem_lavra_vigente`, `ausencia_aprovada`, `ausencia_a_decidir`, `dia_sem_trabalho` ou `encerrada_pelo_juiz` observadas no V3; esse arquivo escreve os casos abaixo. (`app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

| Caso escrito | Premissas necessárias e resultado |
|---|---|
| Só fila sem pergunta | Marco apagado, nenhuma pergunta, ação `perguntar/sistema/emissor`; rota ausente e fila vazia. FUTURO nesses campos, desde que as premissas sejam comprovadas. (`app/chamados/tests/test_e3_portas_tarefa.py:32–38`) |
| A residual, pergunta sob B | Âncora B viva fora da célula; ação responder com `porta=b.pk`; rota da pergunta, fio colab e verbo cobrar de B. (`app/chamados/tests/test_e3_portas_tarefa.py:21–30,40–47`) |
| Âncora da própria célula | Pergunta ancorada no chamado da célula, publicado na fila; rota da fila, fio colab. (`app/chamados/tests/test_e3_portas_tarefa.py:67–73`) |
| Isento | `isento_ponto=True`, razão geral `isento_ponto`, nenhuma ação/rota/fila/verbo e fio nada. As razões gerais vazias do V3 não reproduzem esse caso. (`app/chamados/tests/test_e3_portas_tarefa.py:57–65`) |
| Verbo independente da ação | Responder pode coexistir com verbo validar de B; continua exigindo B do cenário A/B. (`app/chamados/tests/test_e3_portas_tarefa.py:75–82`) |
| Relógio | Testa silêncio do chamado e propagação de `agora`; não testa vencimento de marco nem lâmpada desconhecida. (`app/chamados/tests/test_e3_portas_tarefa.py:84–111`) |

Para subgrupos com ação humana, o fato decisivo ausente no V3 é a **porta da ação e sua presença na fila global**, além da localização da âncora para distinguir os cenários A/B e própria célula; o V3 trouxe apenas `[acao,de]`. Os testes exigem essas identidades concretas. (`app/chamados/tests/test_e3_portas_tarefa.py:40–47,67–73`)

**Ação humana com FUTURO `nada` não prova contradição interna.** O contrato implementado escreve expressamente que “ausencia de porta nao diz ausencia de tarefa”; o fio pode ser humano e a rota ausente. Só portas inteiras presentes na fila global produzem rota; `porta_da_tarefa` retorna `None` sem rota, e o diff projeta `None` como `nada`. (`app/chamados/services/tarefa_do_dia.py:540–565`; `app/chamados/management/commands/diff_e3_sombra.py:46–59`)

Há inclusive caso escrito E2 em que a ação é `responder/colaborador`, mas `porta=None`, pois a âncora está silenciada. Portanto essa combinação é admitida pela tarefa; não há esse caso específico no arquivo E3 solicitado. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

O anexo permite listar os humanos com FUTURO `nada`, mas não transformar essa lista em “ATUAL obrigatório” nem em bloqueantes ESTRUTURA sem demonstrar a porta pertinente. (`app/chamados/services/tarefa_do_dia.py:128–133,547–565`; `app/chamados/tests/test_e3_portas_tarefa.py:40–47,67–73`)

## 13/11 16:50 · sessao b2ed85ab4ddc · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

| Empresa | Competência | entra / None | fora / furo_espelho | fora / rescisao_modulo_proprio | fora / ferias_com_batida | fora / sem_codigo_dominio | Contado / CLS_TOTAL | Bate? | Referência |
|---|---|---:|---:|---:|---:|---:|---:|---|---|
| 2 | 2026-10 | 239 | 178 | 59 | 0 | 1 | 477 / 477 | Sim | [nao verificado] |
| 2 | 2026-11 | 194 | 235 | 22 | 1 | 1 | 453 / 453 | Sim | [nao verificado] |
| 3 | 2026-10 | 80 | 26 | 11 | 1 | 1 | 119 / 119 | Sim | [nao verificado] |
| 3 | 2026-11 | 52 | 50 | 12 | 1 | 0 | 115 / 115 | Sim | [nao verificado] |
| 4 | 2026-10 | 14 | 8 | 1 | 0 | 1 | 24 / 24 | Sim | [nao verificado] |
| 4 | 2026-11 | 14 | 7 | 2 | 0 | 1 | 24 / 24 | Sim | [nao verificado] |

| Empresa | Linhas DESL | Referência |
|---|---:|---|
| 2 | 309 | [nao verificado] |
| 3 | 60 | [nao verificado] |
| 4 | 13 | [nao verificado] |

`EXP_TOTAL 26 0` [nao verificado]

## 13/11 16:50 · sessao b1896194f747 · - · - · origem=Ronald (tmux)

Conferência independente confirmou: DESL empresa 2 = **309**, empresa 3 = **60**, empresa 4 = **13**; total = **382**. Transcrevi os IDs por empresa em memória e contei; quantidade de IDs únicos também foi 309/60/13. Fonte: bloco fornecido, sem arquivo:linha [nao verificado].

## 13/11 16:50 · sessao 7cbf4a9c7c5a · - · - · origem=Ronald (tmux)

Empresa 2, 2026-11 [nao verificado]:

| Status | Motivo | Linhas |
|---|---|---:|
| entra | None | 194 |
| fora | furo_espelho | 235 |
| fora | rescisao_modulo_proprio | 22 |
| fora | ferias_com_batida | 1 |
| fora | sem_codigo_dominio | 1 |

Total contado: **453**, bate com `CLS_TOTAL 2 2026-11 453` [nao verificado].

Checagem em memória: transcrição de cada ID com sua categoria; **453 registros e 453 IDs distintos** [nao verificado].

## 13/11 16:50 · sessao d52a31c2a0bf · - · - · origem=Ronald (tmux)

Empresa 2 × 2026-10: entra/None **239**; fora/furo_espelho **178**; fora/rescisao_modulo_proprio **59**; fora/sem_codigo_dominio **1**; outros **0**. Soma **477**, bate com `CLS_TOTAL 2 2026-10 477`. [nao verificado]

Método: contagem em memória dos pares ID+categoria das linhas fornecidas; **477 IDs únicos**. [nao verificado]

## 13/11 16:49 · sessao b1896194f747 · - · - · origem=Ronald (tmux)

| Empresa | Competência | entra / None | fora / furo_espelho | fora / rescisao_modulo_proprio | fora / ferias_com_batida | fora / sem_codigo_dominio | Total contado | CLS_TOTAL bate? | Evidência |
|---|---|---:|---:|---:|---:|---:|---:|---|---|
| 3 | 2026-10 | 80 | 26 | 11 | 1 | 1 | 119 | Sim, 119 | [nao verificado] |
| 3 | 2026-11 | 52 | 50 | 12 | 1 | 0 | 115 | Sim, 115 | [nao verificado] |
| 4 | 2026-10 | 14 | 8 | 1 | 0 | 1 | 24 | Sim, 24 | [nao verificado] |
| 4 | 2026-11 | 14 | 7 | 2 | 0 | 1 | 24 | Sim, 24 | [nao verificado] |

Checagem: transcrição em memória de cada ID e categoria, contagem por categoria e conferência de IDs únicos: 119, 115, 24 e 24 respectivamente; nenhum ID duplicado em cada grupo. Fonte: bloco fornecido, sem arquivo:linha [nao verificado].

## 13/11 16:48 · sessao b424e8236fa7 · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

| Empresa | Competência | Linhas CLS | CLS_TOTAL | Batem? |
|---|---|---:|---:|---|
| 2 | 2026-10 | 477 | 477 | Sim [nao verificado] |
| 2 | 2026-11 | 453 | 453 | Sim [nao verificado] |
| 3 | 2026-10 | 119 | 119 | Sim [nao verificado] |
| 3 | 2026-11 | 115 | 115 | Sim [nao verificado] |
| 4 | 2026-10 | 24 | 24 | Sim [nao verificado] |
| 4 | 2026-11 | 24 | 24 | Sim [nao verificado] |

## 13/11 16:47 · sessao 8a676b1dfcb3 · - · - · origem=Ronald (tmux)

| Empresa | Competência | Linhas CLS | CLS_TOTAL | Bate? |
|---|---|---:|---:|---|
| 2 | 2026-10 | 477 | 477 | Sim [nao verificado] |
| 2 | 2026-11 | 453 | 453 | Sim [nao verificado] |
| 3 | 2026-10 | 119 | 119 | Sim [nao verificado] |
| 3 | 2026-11 | 115 | 115 | Sim [nao verificado] |
| 4 | 2026-10 | 24 | 24 | Sim [nao verificado] |
| 4 | 2026-11 | 24 | 24 | Sim [nao verificado] |

## 13/11 16:46 · sessao b98d0e6c569f · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

1. Contagem das linhas `CLS` fornecidas. Cada coluna indica `status / motivo`; os totais coincidem com `CLS_TOTAL`. [nao verificado]

| Empresa | Competência | entra / None | fora / furo_espelho | fora / rescisao_modulo_proprio | fora / sem_codigo_dominio | fora / ferias_com_batida | Total | Referência |
|---|---|---:|---:|---:|---:|---:|---:|---|
| 2 | 2026-10 | 223 | 194 | 59 | 1 | 0 | 477 | [nao verificado] |
| 2 | 2026-11 | 193 | 236 | 22 | 1 | 1 | 453 | [nao verificado] |
| 3 | 2026-10 | 77 | 29 | 11 | 1 | 1 | 119 | [nao verificado] |
| 3 | 2026-11 | 52 | 50 | 12 | 0 | 1 | 115 | [nao verificado] |
| 4 | 2026-10 | 13 | 9 | 1 | 1 | 0 | 24 | [nao verificado] |
| 4 | 2026-11 | 14 | 7 | 2 | 1 | 0 | 24 | [nao verificado] |

2. **Não é possível determinar quantos desses `fora/furo_espelho` são desligados nem listar seus IDs como desligados:** falta `situacao` por `colaborador_id` na foto fornecida. [nao verificado]

Na cópia, o juiz considera desligado quem tem `situacao == 'desligado'` ([desligamento.py:32](/home/ronald/codex_ws/3e655a73aafe60d8d6df950181abed3454db9dc4/app/colaboradores/services/desligamento.py:32)); para esses colaboradores, a retenção por espelho retorna vazia ([export.py:1207](/home/ronald/codex_ws/3e655a73aafe60d8d6df950181abed3454db9dc4/app/folha/export.py:1207), [export.py:1211](/home/ronald/codex_ws/3e655a73aafe60d8d6df950181abed3454db9dc4/app/folha/export.py:1211)).

Para afirmar quem passaria a `entra`, falta também o resultado da homologação desses IDs. [nao verificado] Após o crivo do espelho, ainda existe a saída `fora/nao_homologado` ([export.py:974](/home/ronald/codex_ws/3e655a73aafe60d8d6df950181abed3454db9dc4/app/folha/export.py:974), [export.py:976](/home/ronald/codex_ws/3e655a73aafe60d8d6df950181abed3454db9dc4/app/folha/export.py:976)).

3. **EXP:** nenhuma linha `EXP` consta no dado fornecido; falta esse trecho para transcrever. [nao verificado]

**FM10 — transcrição integral do dado fornecido:** [nao verificado]

```text
FM10 2 477 0490b42dcbafac20016a58290371e84a6db8c679dc8623a870c6a3953936ebe0
FM10 3 119 8958a156829db89e92940c8c1965023f41b252a5a38f5918ec6d8bd2cb5f93a0
FM10 4 24 60f5e04ce9f5ea898f4d325ff5f02cec43f8315b1aee7a2f13af3c44a745e09f
```

## 13/11 16:45 · sessao e84e0ec0f488 · - · - · origem=Ronald (tmux)

Contagem exclusiva das linhas `CLS` fornecidas; todas as células abaixo: `[nao verificado]` quanto a arquivo:linha, pois a fonte é o texto do usuário.

| Empresa | Competência | entra / None | fora / furo_espelho | fora / rescisao_modulo_proprio | fora / ferias_com_batida | fora / sem_codigo_dominio | Total |
|---|---|---:|---:|---:|---:|---:|---:|
| 3 | 2026-10 | 77 | 29 | 11 | 1 | 1 | 119 |
| 3 | 2026-11 | 52 | 50 | 12 | 1 | 0 | 115 |
| 4 | 2026-10 | 13 | 9 | 1 | 0 | 1 | 24 |
| 4 | 2026-11 | 14 | 7 | 2 | 0 | 1 | 24 |

Cada soma coincide com seu `CLS_TOTAL`; transcrevi os IDs por categoria em listas em memória e conferi que total e quantidade de IDs distintos coincidem em cada grupo. `[nao verificado]`

## 13/11 16:44 · sessao 994e7d801813 · E3-FORMA-SQL · leitura · origem=Code

**A recontagem fecha em 351 colab-dias. G5 tem 132, não 125. Não há, nos fatos fornecidos e nos casos escritos, prova suficiente para declarar algum colab-dia obrigatoriamente ATUAL e, portanto, FUTURO errado. Há escolhas de campos amparadas pela regra e várias combinações sem caso escrito.** (a1v2_par_det.txt:1–351; app/chamados/tests/test_e3_portas_tarefa.py:32–111)

Uso estas abreviações nas referências:

- **D:n** = `a1v2_par_det.txt:n`, contando exclusivamente as linhas `PAR_DET` coladas, começando em 1.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.

Os dois scripts `logs/sombra/e3_par_a_par.py` e `e3_par_det.py` não estão disponíveis nesta cópia; conferir sua emissão específica ficou **[nao verificado]**. O instrumento disponível confirma ATUAL=C17, FUTURO=`porta_da_tarefa`, e as projeções dos consumidores. (app/chamados/management/commands/diff_e3_sombra.py:46–59,126–130)

**1. Regra aplicável por grupo**

Para G6–G10, explicito abaixo a numeração adotada: o enunciado não identifica suas assinaturas anteriores; a correspondência com aqueles números antigos é **[nao verificado]**.

| Grupo | Quantidade e assinatura | Caso escrito e escolha que ele permite |
|---|---|---|
| **G1** | **35**, somente fila encolhe. (D:1–351) | **Sem caso escrito para múltiplos chamados da célula.** T:67–73 cobre uma pergunta sob um chamado da célula; não decide conservar/excluir os demais chamados. Faltam identidade, âncora e exigibilidade de cada porta retirada. |
| **G2** | **68**, `chamado → nada`, fila esvazia. (D:1–351) | **53 sem vivas:** a cláusula “só fila sem pergunta” manda ausência de porta humana, portanto **FUTURO nesses campos**. Contudo, o caso também exige `perguntar/sistema/emissor`, ação ausente nesses 53: não há conformidade integral demonstrada. **15 com vivas: sem caso escrito** para a combinação apresentada. (T:3,32–38; exemplos D:2–3,10–11) |
| **G3** | **30**, `colab → nada`, fila esvazia. (D:1–351) | **1 sem vivas**, pk **115432**: mesma conclusão parcial do caso 1, **FUTURO para ausência de porta**. **29 com vivas: sem caso escrito** que autorize escolher ATUAL ou FUTURO só com esses campos. (D:52; T:32–38) |
| **G4** | **65**, `admin → nada`, fila esvazia. (D:1–351) | Todas têm vivas e ações vazias: **sem caso escrito** correspondente. Faltam decisões E1, identidade/âncora, lâmpadas, vencimento e razões por instância para explicar por que nenhuma ação permanece. (exemplos D:5–6; T:32–82; app/chamados/services/tarefa_do_dia.py:230–313) |
| **G5** | **132**, `admin → colab`, sem diferença de fila. (D:1–351) | T:75–82 escreve que **`responder/colaborador` pode coexistir com verbo `validar`**. Isso ampara essa combinação, mas não certifica os 132 nem decide o desempate nos **8 com ações colab+admin**. **Sem caso escrito geral que imponha um dos lados ao grupo inteiro.** |
| **G6** | **4**, `admin → colab` e fila encolhe. (D:23,94,96,180) | **Sem caso escrito** para a combinação: três têm ações colab+admin; uma somente colab; todas retiram outra porta da célula. Faltam regra de desempate e caso para múltiplas portas. (T:67–82) |
| **G7** | **2**, fila esvazia, rota `porta_fila → porta_pergunta`, fio permanece. (D:26,225) | **130217:** corresponde à escolha A/B: responder sob B, retirar A residual; **FUTURO** nos campos comparados. **134521:** a ação é `validar/admin/B`; **sem caso escrito nesse arquivo para essa variante**, que não é a ação responder usada no caso A/B. (T:40–55; D:26,225) |
| **G8** | **7**, `admin → nada`, rota da pergunta desaparece; sem diferença de fila. (D:48,69,113,230–231,300,328) | Vivas presentes e ações vazias: **sem caso escrito** correspondente. Faltam identidade/âncora, decisões e exigibilidade por instância. (T:32–82; app/chamados/services/tarefa_do_dia.py:230–313) |
| **G9** | **7**, `chamado → colab`; tarefa com verbo `historico`. (D:105–111) | T:100–111 admite verbo `historico` e fila vazia, mas **não decide esse fio**. **Sem caso escrito para escolher entre chamado e colab nessa combinação.** |
| **G10** | **1**, `admin → colab`, fila esvazia e rota passa à pergunta; A=`decidir`, B=`validar`. (D:302) | O caso A/B ampara **FUTURO para rota/fila**, preservando o verbo de B; responder com verbo validar também está escrito. **O fio nessa combinação não recebe um teste explícito de desempate entre as duas formas de resumi-lo.** (T:40–55,75–82) | [nao verificado]

A classificação automática `so_fila_sem_pergunta` verifica fila e ausência de vivas; ela não verifica todas as premissas do teste. Há inclusive ressalva escrita de que tarefa `nao_sei` por marco fora do turno **não seria o caso 1**. (app/chamados/management/commands/diff_e3_sombra.py:63–70; app/chamados/tests/test_e3_leitores_c17_chamadores.py:46–47)

**2. Subgrupos e exemplos**

Cada exemplo abaixo é **pk da célula**, seguido da referência ao anexo. Subgrupos unitários têm apenas um exemplo possível.

| Grupo | Subgrupos exatos; exemplos |
|---|---|
| **G1 — 35** | **29** com `cobrar+decidir`: 141754 (D:1), 138821 (D:50). **4** com `cobrar+cobrar`: 129205 (D:195), 129879 (D:282). **1** com `cobrar+cobrar+decidir`: 142967 (D:341). **1** com `validar+decidir+decidir`: 142942 (D:340). Nenhuma dessas variantes ganha caso explícito para múltiplas portas em T:67–82. |
| **G2 — 68** | **53 vivas vazias:** **51** sem ações — 120067 (D:2), 119985 (D:3); **1** com `rejulgar/sistema/cartorio` — 130929 (D:157); **1** com `validar/admin/null` — 120319 (D:294). **15 vivas presentes:** **12** com `responder/colaborador/null` — 118930 (D:10), 118931 (D:11); **2** com `decidir_ausencia/admin/null` — 138643 (D:166), 129575 (D:241); **1** com `validar/admin/null` — 141817 (D:338). A condição “sem pergunta” muda entre 53 e 15; nenhuma das ações observadas reproduz integralmente T:37–38. |
| **G3 — 30** | **1** sem vivas: 115432 (D:52). **24** com vivas e ações vazias: 132340 (D:53), 129189 (D:57). **2** com `responder/colaborador/null`: 141714 (D:4), 141810 (D:337). **3** com `decidir_ausencia/admin/null`: 117062 (D:87), 112501 (D:223), 127887 (D:321). Por verbos: **28** somente cobrar, **1** cobrar+decidir (D:223), **1** somente decidir (D:321). |
| **G4 — 65** | **61** somente validar: 130162 (D:6), 118933 (D:13). **4** validar+decidir: 130160 (D:5), 123932 (D:217), 129563 (D:235), 132052 (D:264). Todas têm vivas e ações vazias; trocar esses verbos não encontra caso específico em T:32–82. |
| **G5 — 132** | **102** fila vazia, sem ação admin: 124787 (D:7), 124794 (D:8). **1** fila vazia, colab+admin: 115080 (D:139). **22** fila presente, sem ação admin: 123615 (D:16), 117853 (D:17). **7** fila presente, colab+admin: 130970 (D:90), 129663 (D:152). Portanto, **124 sem admin e 8 com admin**. Dos 124, um também tem ação sistema: 131734 (D:38). O caso misto colab+admin não está decidido em T:75–82. |
| **G6 — 4** | **3** colab+admin: 127078 (D:23), 132926 (D:94), 139136 (D:96). **1** somente colab: 119775 (D:180). O caso misto e a retirada de porta adicional continuam sem teste específico. (T:67–82) | [nao verificado]
| **G7 — 2** | **1** responder/colaborador/B, verbos A/B cobrar: 130217 (D:26), coberto pela escolha A/B. **1** validar/admin/B, verbos A/B validar: 134521 (D:225), variante sem caso escrito nesse arquivo. (T:40–55) |
| **G8 — 7** | Vivas presentes e ações vazias: 131162 (D:48), 116182 (D:69). Nenhuma separação adicional que decida ATUAL/FUTURO foi demonstrada pelos campos apresentados. (D:48,69,113,230–231,300,328; T:32–111) |
| **G9 — 7** | Todos com responder/colaborador e verbo da tarefa histórico: 110014 (D:105), 110016 (D:106). O teste de histórico não escolhe o fio. (T:100–111) | [nao verificado]
| **G10 — 1** | 120408 (D:302): A decidir, B validar, responder/colaborador/B. Não há segundo exemplo. |

**3. Onde o FUTURO é produzido e dono L-099**

**Não há lista comprovada de “ATUAL obrigatório/FUTURO errado”.** As lacunas acima não autorizam declarar ATUAL correto; tampouco certificam FUTURO nos casos sem cobertura escrita. (T:32–111)

Os pontos que produzem o FUTURO observado são:

| Comportamento | Arquivo:linha |
|---|---|
| A ação recebe porta inteira apenas quando a âncora pertence à fila carregada; caso contrário, recebe `None`. | app/chamados/services/tarefa_do_dia.py:128–129 |
| Decisões E1 escolhem responder, validar, materializar ou decidir ausência. | app/chamados/services/tarefa_do_dia.py:130–162 |
| Lâmpadas, teto, cobertura e identidade podem deixar a instância sem ação ou `nao_sei`. | app/chamados/services/tarefa_do_dia.py:262–313 |
| Só portas inteiras presentes nas ações e na fila global entram na publicação; isso pode encolher a fila. | app/chamados/services/tarefa_do_dia.py:547–551 |
| O fio é escolhido pelos donos das ações, com colab antes de admin. | app/chamados/services/tarefa_do_dia.py:552–556 |
| Sem rota, `porta_da_tarefa` devolve `None`; a projeção da folha transforma isso em `nada`. | app/chamados/services/tarefa_do_dia.py:559–565; app/chamados/management/commands/diff_e3_sombra.py:49–50 |

**Dono individual L-099: [nao verificado].** O anexo não fornece os argumentos necessários ao juiz: horas do dia, cadastro × realidade, folga não definida, vínculo, batida fora do turno e fase deslocada. Motivo de pergunta não substitui esses argumentos. (app/ponto/management/commands/e6_oraculo.py:53–54; D:1–351)

O juiz prioriza cadastro por folga/vínculo; depois batida fora do turno ou quantidade ímpar; depois fase deslocada; finalmente cadastro × realidade ou estrutura residual. Atribuir ESTRUTURA automaticamente a uma divergência de porta não seria executar esse juiz. (app/ponto/management/commands/e6_oraculo.py:98–110)

**4. Perda de porta humana**

**São 170 colab-dias passando a `nada`: 163 com verbo explicitamente apresentado em `verbos_c17`, mais 7 G8 cuja existência de verbo `validar` é deduzida do ATUAL `admin` pelo código do C17.** (D:1–351; app/chamados/juizes.py:1633–1639,1677–1688)

**Nenhuma linha apresenta literalmente status `aberto`: os campos mostrados são `null`. A quantidade realmente aberta é [nao verificado].** Não interpretei `null` como aberto. (D:1–351)

| Grupo | Quantidade | Pks das células |
|---|---:|---|
| **G2** | **68** | 103972, 104004, 104127, 105151, 105621, 105623, 105729, 105768, 105773, 106282, 106286, 106462, 106602, 106604, 106608, 106939, 107178, 108035, 108622, 108826, 109494, 109708, 110555, 110559, 110573, 111382, 112703, 112716, 113113, 113667, 113671, 113673, 113675, 113679, 113681, 115993, 116542, 116546, 116554, 117245, 118928, 118930, 118931, 118932, 118934, 119985, 120067, 120233, 120319, 123531, 129083, 129090, 129093, 129575, 129598, 129745, 129747, 129749, 129751, 130771, 130929, 138643, 140114, 140121, 140122, 140128, 141817, 142013. (D:2–339) |
| **G3** | **30** | 112501, 115432, 117062, 121500, 127887, 127949, 129189, 129217, 129559, 129561, 129567, 129569, 129571, 129573, 129577, 130044, 130294, 130418, 130781, 132340, 133374, 135811, 136215, 137154, 137166, 138912, 138914, 138916, 141714, 141810. (D:4–337) |
| **G4** | **65** | 33784, 104040, 104784, 106350, 106594, 106963, 108591, 108633, 110506, 111346, 111845, 115650, 116432, 116436, 118933, 119517, 120325, 120326, 120378, 120486, 122073, 123033, 123487, 123534, 123614, 123932, 123998, 124364, 124370, 124774, 127039, 127041, 127964, 128516, 128519, 129384, 129563, 129565, 129885, 129901, 130026, 130160, 130162, 130931, 130933, 130935, 130937, 130939, 132052, 132144, 133364, 135067, 135778, 138672, 139432, 140331, 140522, 140524, 140580, 140707, 140928, 141797, 143313, 143393, 143395. (D:5–345) |
| **G8** | **7** | 113307 (D:113), 116182 (D:69), 127477 (D:328), 128533 (D:300), 130346 (D:230), 130348 (D:231), 131162 (D:48). |

Nos sete G8, `verbos_c17={}` **não prova ausência de chamado com verbo**: o ATUAL é `porta_pergunta/admin`, e esse resultado do C17 exige âncora na fila global e verbo validar. O pk desse chamado não está apresentado nesses registros. (D:48,69,113,230–231,300,328; app/chamados/juizes.py:1633–1639,1677–1688)

**5. Prioridade colab/admin e cobrar/validar**

A escada `cobrar > validar` está em **`app/chamados/juizes.py:1624–1628`**, não em `core/juizes.py`: o comentário escreve que a pergunta devida pelo colaborador precede a validação pelo admin. A tarefa documenta colab/admin/sistema e aplica essa ordem aos **donos das ações**. (app/chamados/services/tarefa_do_dia.py:39,552–554)

**Não há, em `test_e3_portas_tarefa.py`, caso escrito que determine qual dessas duas formas de resumir o dia vence quando elas divergem, nem caso explícito de ações simultâneas colab+admin.** O teste escreve justamente que ação responder e verbo validar podem coexistir. (T:32–111, especialmente 75–82) [nao verificado]

Há uma escolha escrita adicional para o **vigia**: o papel da porta vem do fio da tarefa, independentemente do verbo do painel. Isso decide a leitura do vigia; não constitui um teste da prioridade interna entre ações colab e admin. (app/chamados/tests/test_e3_bloqueantes.py:15–16,193–198)

## 13/11 16:41 · sessao c2b6a137d107 · - · - · origem=Ronald (tmux)

Auditoria independente confirmou:

| Grupo | Contagem | Subgrupos |
|---|---:|---|
| G2 | 68 | 53 com `vivas_c17=[]`; 15 não vazias. Das 53: 51 ações vazias, 130929 `rejulgar/sistema/cartorio`, 120319 `validar/admin/null`. Das 15: 12 `responder/colaborador/null`, 2 `decidir_ausencia/admin/null` (138643,129575), 1 `validar/admin/null` (141817). |
| G3 | 30 | 1 com vivas vazias (115432), 29 não vazias; verbo `cobrar` em 29, `decidir` em 127887. Ações vazias em 25; responder/null em 141714 e141810; decidir_ausencia/admin/null em117062,112501,127887. |
| G4 | 65 | Todas com vivas não vazias e ações vazias. Verbo somente validar em61; validar+decidir em130160,123932,129563,132052. |
| G8 | 7 | 131162,116182,113307,130346,130348,128533,127477. |

Contagens auditadas por transcrição manual do anexo em memória e contagem JavaScript; referências de linhas individuais ficaram a cargo do agente contagem.

Q4: **170 perdem porta humana**, sendo **163 com verbo explicitamente mostrado em `verbos_c17`** (G2+G3+G4) e **7 G8 com existência de verbo `validar` deduzida do fio ATUAL `admin`**: C17 só retorna porta com `_verbos` não vazio e calcula fio por esses verbos; `admin` exige presença de `validar`. (app/chamados/juizes.py:1633–1639,1677–1688)

Nos sete G8, `fila_cel_c17=[]`, mas a rota ATUAL é `porta_pergunta`; portanto existe âncora sob chamado na fila global. Não se deve interpretar `verbos_c17={}` como ausência de chamado; o mapa completo retornado pelo C17 necessariamente contém `validar`, embora seu pk não esteja mostrado. (app/chamados/juizes.py:1677–1688)

Todos os campos `status`/`status_disputa` mostrados nas vivas são `null`; **nenhum `"aberto"` explícito**. Quantidade realmente aberta: **[nao verificado]**. As listas completas dos163 pks foram enviadas por mensagem.

## 13/11 16:41 · sessao 372c5381d4b8 · - · - · origem=Ronald (tmux)

Recontagem das **351 linhas PAR_DET**, numeradas a partir da primeira linha do anexo colado:

| Grupo por assinatura | Contagem |
|---|---:|
| G1: apenas fila encolhe | 35 |
| G2: chamado→nada, fila→() | 68 |
| G3: colab→nada, fila→() | 30 |
| G4: admin→nada, fila→() | 65 |
| G5: admin→colab, sem diferença de fila | **132** |
| G6: admin→colab, fila encolhe | 4 |
| G7: fila→(), rota fila→pergunta, sem diferença de fio | 2 |
| G8: admin→nada, rota pergunta→None, sem diferença de fila | 7 |
| G9: chamado→colab | 7 |
| G10: admin→colab, fila→(), rota fila→pergunta | 1 |

A numeração G6..G10 acima é convenção minha, porque o prompt não identifica as antigas assinaturas pequenas. As contagens somam 351; a contagem antiga G5=125 não se reproduz.

Subgrupos conferidos:

| Grupo | Fatos | n | Exemplos pk:linha PAR_DET |
|---|---|---:|---|
| G1 | verbos cobrar+decidir, qualquer ordem | 29 | 141754:1; 138821:50 |
| G1 | cobrar+cobrar | 4 | 129205:195; 129879:282 |
| G1 | cobrar+cobrar+decidir | 1 | 142967:341 |
| G1 | validar+decidir+decidir | 1 | 142942:340 |
| G2 | vivas vazias, ações vazias | 51 | 120067:2; 119985:3 |
| G2 | vivas vazias, rejulgar sistema | 1 | 130929:157 |
| G2 | vivas vazias, validar admin, porta null | 1 | 120319:294 |
| G2 | vivas não vazias, responder colab, porta null | 12 | 118930:10; 118931:11 |
| G2 | vivas não vazias, decidir_ausencia admin, porta null | 2 | 138643:166; 129575:241 |
| G2 | vivas não vazias, validar admin, porta null | 1 | 141817:338 |
| G3 | vivas vazias | 1 | 115432:52 |
| G3 | vivas não vazias, ações vazias | 24 | 132340:53; 129189:57 |
| G3 | vivas não vazias, responder colab, porta null | 2 | 141714:4; 141810:337 |
| G3 | vivas não vazias, decidir_ausencia admin, porta null | 3 | 117062:87; 112501:223; terceiro 127887:321 |
| G3 | só cobrar | 28 | 141714:4; 115432:52 |
| G3 | cobrar+decidir | 1 | 112501:223 |
| G3 | só decidir | 1 | 127887:321 |
| G4 | só validar | 61 | 130162:6; 118933:13 |
| G4 | validar+decidir | 4 | 130160:5; 123932:217; demais 129563:235 e 132052:264 |
| G5 | fila vazia, sem ação admin | 102 | 124787:7; 124794:8 |
| G5 | fila vazia, colab+admin | 1 | 115080:139 |
| G5 | fila não vazia, sem ação admin | 22 | 123615:16; 117853:17 |
| G5 | fila não vazia, colab+admin | 7 | 130970:90; 129663:152 |
| G6 | colab+admin | 3 | 127078:23; 132926:94 |
| G6 | só colab | 1 | 119775:180 |
| G7 | responder colab sob B; verbos A/B cobrar | 1 | 130217:26 |
| G7 | validar admin sob B; verbos A/B validar | 1 | 134521:225 |
| G8 | fila vazia, verbos vazios, ações vazias; vivas não vazias | 7 | 131162:48; 116182:69 |
| G9 | fila vazia; verbo_tarefa historico; responder colab | 7 | 110014:105; 110016:106 |
| G10 | A decidir, B validar; responder colab sob B | 1 | 120408:302 |

G5 possui **123 registros somente com ações colab**, **8 com colab+admin** e **1 com colab+sistema**, este último 131734:38. Portanto “sem admin” é o rótulo correto para os 124 restantes; “somente colab” não cobre 131734:38.

Para Q4: **163 registros** têm FUTURO nada e verbo em `verbos_c17`: G2=68, G3=30, G4=65. Listas completas e linhas foram enviadas em mensagens anteriores. Os sete G8 não têm verbo C17, e todos os status presentes no anexo são null; nenhum registro comprova `status='aberto'`. Os G8 são 131162:48, 116182:69, 113307:113, 130346:230, 130348:231, 128533:300 e 127477:328.

## 13/11 16:39 · sessao b13326346ebb · - · - · origem=Ronald (tmux)

- **As sondas citadas não estão disponíveis na cópia**, e não pude conferir seu formato ou a emissão de `PAR_DET`; o instrumento disponível é `app/chamados/management/commands/diff_e3_sombra.py:63-70`, cujos rótulos indicam apenas fila/vivas/âncoras, não validam todas as premissas dos testes. [nao verificado: e3_par_a_par.py/e3_par_det.py]

- **Caso 1:** nenhuma porta humana quando há fila sem pergunta; a tarefa deve publicar ação `perguntar/sistema/emissor`. G2/G3 sem vivas podem coincidir com a dimensão “sem pergunta”, mas ações vazias, `validar/admin/None` ou `rejulgar/sistema/cartorio` não reproduzem integralmente o caso escrito. Faltam lâmpadas, teto, vínculo, decisões E1 e razões por instância para decidir por que não há pergunta exigível. `app/chamados/tests/test_e3_portas_tarefa.py:32-38`; a ressalva explícita de que `nao_sei` por marco fora do turno **não é caso 1** está em `app/chamados/tests/test_e3_leitores_c17_chamadores.py:46-47`.

- **Fila encolhendo (G1):** não há teste com duas ou mais portas da célula, com verbos diferentes ou dois `cobrar`, que escolha quais devem continuar. O teste da âncora da própria célula usa um chamado e uma pergunta, e espera `(ch.pk,)`, `cobrar`, `colab`. Para multicall: **sem caso escrito** no arquivo solicitado; faltam identidade/âncora e exigibilidade de cada porta excluída. `app/chamados/tests/test_e3_portas_tarefa.py:67-73`.

- **Fila A/pergunta B:** há caso escrito para tarefa `responder/colaborador/B`, rota da pergunta, fila vazia, preservando o verbo de B; A residual sai. Quando B tem módulo `gps_fora_geofence`, a ação continua `responder` e o verbo deve ser `validar`. Essa convivência é deliberada, não prova erro de prioridade. `app/chamados/tests/test_e3_portas_tarefa.py:40-55`; `app/chamados/tests/test_e3_portas_tarefa.py:75-82`.

- **G2/G3/G4 com vivas e sem porta:** o teste “só fila sem pergunta” não cobre esses fatos. A simples presença em `vivas_c17` não informa qual âncora pertence a qual pergunta, últimas decisões, passivo, estado de lâmpada, teto, posição/instância ou por que uma ação recebeu `porta=None`. **Sem caso escrito que determine ATUAL** apenas por esses campos. A produção deriva `porta_admin` da presença da âncora na fila e usa E1 para escolher a ação. `app/chamados/services/tarefa_do_dia.py:128-162`; a exigibilidade também depende de `app/chamados/services/tarefa_do_dia.py:262-313`.

- **G5 responder/colaborador com verbo validar:** o arquivo escreve explicitamente essa combinação; o verbo não se traduz da ação. Isso sustenta a coexistência `fio=colab` e `verbo=validar`, mas **não resolve teste misto** com ações simultâneas `validar/admin` e `responder/colaborador`, nem prova cada caso de produção sem o módulo/âncora. `app/chamados/tests/test_e3_portas_tarefa.py:75-82`.

- **Prioridade:** a escada `cobrar > validar` está em **`app/chamados/juizes.py`**, não `core/juizes.py`. O comentário escreve sua justificativa: o dia chega ao admin depois de o colaborador falar, atribuindo-a ao aval de 30/10 23:5x item (a). `app/chamados/juizes.py:1624-1628`. A tarefa documenta `colab, admin, sistema` e executa essa ordem sobre os donos das ações. `app/chamados/services/tarefa_do_dia.py:39`; `app/chamados/services/tarefa_do_dia.py:552-556`. **Não há caso no teste solicitado que decida qual escada vence quando divergem**, particularmente no caso misto; `test_verbo_nao_e_traducao_da_acao_da_tarefa` mostra que não são equivalentes. `app/chamados/tests/test_e3_portas_tarefa.py:75-82`. Outro teste escreve que o papel no vigia é o fio da tarefa, independentemente do verbo. `app/chamados/tests/test_e3_bloqueantes.py:15-16`; `app/chamados/tests/test_e3_bloqueantes.py:193-198`.

- **Dono L-099 por colab-dia: [nao verificado].** O anexo não fornece os insumos exigidos por `dono_da_divergencia`: horas do dia, cadastro × realidade, folga não definida, ausência de vínculo, batida fora do turno e fase deslocada. Motivos de pergunta não substituem esses insumos julgados. `app/ponto/management/commands/e6_oraculo.py:53-54`. A ordem é cadastro prioritário por folga/vínculo; batida fora do turno; quantidade ímpar; fase deslocada; cadastro × realidade; estrutura residual. `app/ponto/management/commands/e6_oraculo.py:98-110`.

- **Pergunta com status aberto:** no anexo, os campos de status mostrados são `null`; não se pode convertê-los em `aberto`. Contagem literal de status `aberto` e lista de pks dependem da recontagem; existência de status efetivamente aberto **[nao verificado]**. A escolha de uma ação `responder` tampouco significa status aberto, pois E1 permite resposta após recusa/reabertura. `app/chamados/services/tarefa_do_dia.py:139-140`.

## 13/11 16:33 · sessao 6d1c9cc531a4 · E3-FORMA-SQL · leitura · origem=Code

**O anexo não pode ser certificado como saída do comparador desta cópia.** O código imprime JSON agregado; não escreve `PAR_DIF`, `PAR_TOTAL` nem pares `[cópia, base]`. Ele identifica C17 como `antes` e tarefa como `depois`, mas não confirma a ordem do serializador apresentado. ([diff_e3_sombra.py:107](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:107), [diff_e3_sombra.py:126](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:126).)

Também contei **344 registros, com 344 PKs distintos e dez assinaturas**, não 351: ficam sete diferenças declaradas sem linha correspondente no texto recebido. As contagens e os PKs abaixo são do anexo colado, sem arquivo local para referência: **[não verificado]**.

Há uma incompatibilidade objetiva com a ordem declarada: a tarefa desta cópia **nunca produz `fio='chamado'`**; esse valor pertence ao C17 preservado. Não inverti as colunas para acomodar o resultado. ([tarefa_do_dia.py:552](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:552), [juizes.py:1628](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1628).)

**1. Agrupamento literal do anexo**

Convenções: `F` = `fora_da_folha._porta`; `V` = `vigia_fio.censo_alcance`; `C` = `cadastro_realidade.beco_em_lote`; `P` = `pautas_dos_discordantes.dias_sem_fio`. Os campos são as projeções definidas em [diff_e3_sombra.py:46](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:46).

As setas seguem **primeira posição → segunda posição**, declaradas pelo enunciado como cópia → base **[não verificado]**. Para apresentar os conjuntos completos sem repeti-los:

- **S(x,r)** = `F.fio x→nada`; `V.fio x→None`; `V.rota r→None`; `C.sem_porta False→True`; `P.sem_fio False→True`.
- **A** = `F.fio admin→colab`; `V.fio admin→colab`; `V.exige_admin True→False`.
- **f+** = `F.fila não vazia→()`.
- **f⊃** = `F.fila superconjunto próprio→subconjunto não vazio`.

Essas convenções apenas abreviam os registros fornecidos **[não verificado]**.

| Grupo | Assinatura completa | Contagem | Dois exemplos: PK da célula |
|---|---|---:|---|
| G1 | `f⊃` | 35 | 141754; 138821 |
| G2 | `S(chamado, porta_fila)` + `f+` | 68 | 120067; 119985 |
| G3 | `S(colab, porta_fila)` + `f+` | 30 | 141714; 115432 |
| G4 | `S(admin, porta_fila)` + `f+` + `V.exige_admin True→False` | 65 | 130160; 130162 |
| G5 | `A` | 125 | 124787; 124794 |
| G6 | `A` + `f⊃` | 4 | 127078; 132926 |
| G7 | `f+` + `V.rota porta_fila→porta_pergunta` | 2 | 130217; 134521 |
| G8 | `S(admin, porta_pergunta)` + `V.exige_admin True→False` | 7 | 131162; 116182 |
| G9 | `F.fio chamado→colab`; `V.fio chamado→colab` | 7 | 110014; 110016 |
| G10 | `A` + `f+` + `V.rota porta_fila→porta_pergunta` | 1 | 120408; não existe segundo exemplo |
| **Total** | | **344** | **[não verificado: anexo colado]** |

Os mecanismos legíveis dos dois caminhos são estes; **C17 abaixo é o código antigo preservado na cópia, não uma comprovação dos bytes de `main` anterior à E3 [não verificado]**.

| Grupos | C17 preservado | Tarefa da cópia | O que se pode concluir |
|---|---|---|---|
| G1 | Inclui todos os chamados visíveis da célula na fila. ([juizes.py:1677](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1677)) | Inclui somente portas das ações exigíveis pertencentes à célula. ([tarefa_do_dia.py:548](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:548)) | Sob a mesma alimentação, a fila da tarefa é subconjunto da C17; esses corpos não justificam a ampliação declarada. |
| G2, G9 | Verbos diferentes de `cobrar`/`validar` podem resultar em `chamado`. ([juizes.py:1633](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1633)) | O vocabulário calculado não contém `chamado`. ([tarefa_do_dia.py:553](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:553)) | A primeira posição não pode ser essa saída da tarefa. |
| G3 | Abre por fila da célula ou âncora de pergunta viva, com texto válido e no app. ([juizes.py:1677](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1677)) | A decisão pode exigir resposta do colaborador; a publicação depende da porta dessa ação. ([tarefa_do_dia.py:129](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:129), [tarefa_do_dia.py:548](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:548)) | Falta a pergunta, decisão e alimentação usadas para identificar a causa concreta. |
| G4, G8 | Usa os critérios de alcance acima e traduz verbos em fio. ([juizes.py:1681](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1681), [juizes.py:1633](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1633)) | A sequência pode exigir validação administrativa; há exposição administrativa específica nos marcos dispensados. ([tarefa_do_dia.py:138](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:138), [tarefa_do_dia.py:243](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:243)) | A assinatura não informa qual decisão/instância produziu a ação. |
| G5 | `cobrar` vence `validar` entre os verbos das portas. ([juizes.py:1628](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1628)) | O dono é escolhido entre as ações exigíveis: colaborador antes de admin. ([tarefa_do_dia.py:552](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:552)) | Há mudança de fonte semântica; faltam ações e verbos individuais para explicar cada registro. |
| G6 | Combina os mecanismos de G1 e G5. ([juizes.py:1686](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1686)) | Combina seleção de portas e donos das ações. ([tarefa_do_dia.py:548](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:548)) | Mesmas limitações de G1 e G5. |
| G7 | `porta_fila` se existe qualquer fila da célula. ([juizes.py:1687](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1687)) | `porta_fila` somente se existe porta exigível da célula; outra âncora gera `porta_pergunta`. ([tarefa_do_dia.py:550](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:550)) | Sob a mesma alimentação, não explica a direção declarada. |
| G10 | Combina os mecanismos de G5 e G7. ([juizes.py:1687](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1687)) | Combina seleção de rota e donos das ações. ([tarefa_do_dia.py:552](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:552)) | Falta a execução identificada para atribuir causa. |

**2. Dono L-099 e julgamento**

Para **cada grupo G1–G10**, tanto o dono quanto a classificação “universo antes inalcançável/cópia certa” ou “regressão/cópia errada” são **[não decidível]**. A assinatura não contém os insumos exigidos pelo juiz; seu resíduo `ESTRUTURA` só pode ser aplicado depois de avaliar as condições anteriores. ([e6_oraculo.py:98](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/ponto/management/commands/e6_oraculo.py:98).)

| Precedência efetiva | Dono |
|---|---|
| Folga não definida ou ausência de vínculo aplicável | CADASTRO |
| Batida fora de turno ou quantidade ímpar de batidas | BATIDA |
| Fase deslocada | CADASTRO |
| Presença em cadastro × realidade | CADASTRO |
| Resíduo | ESTRUTURA |

Essa ordem está em [e6_oraculo.py:98](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/ponto/management/commands/e6_oraculo.py:98). Faltam esses fatos **por colab-dia**, além do emissor dos pares e da base histórica identificada **[não verificado]**.

As regras que permitiriam julgar cenários completos estão escritas antes dos testes: [nao verificado]

- Só fila sem pergunta: nenhuma porta humana; sistema deve perguntar.
- Fila residual A com pergunta sob B: publicar a porta de B, sem A residual.
- Ler o verbo de B no universo do painel.
- Isento com fila residual: nenhuma ação ou porta.

Referência: [test_e3_portas_tarefa.py:1](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py:1). As etiquetas do anexo não substituem todos os fatos desses cenários. L-110 determina que DIFF mede impacto, não correção; L-114 distingue impacto de certificado. ([LEIS.md:136](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/docs/LEIS.md:136), [LEIS.md:152](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/docs/LEIS.md:152).)

**3. Discordância entre leitores da mesma cópia**

**Nenhum grupo apresentado demonstra essa discordância [não verificado: anexo colado].** Neste instrumento, todos os leitores são projeções da **mesma variável `porta`**: os dois fios vêm do mesmo campo; `sem_porta` e `sem_fio` são o mesmo teste `porta is None`. `nada` na folha e `None` no vigia representam a mesma ausência. ([diff_e3_sombra.py:46](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:46).)

O comparador declara que **não executa os censos nem suas outras decisões**. Portanto não prova concordância das execuções reais; para isso faltam saídas dos leitores com a mesma entrada, estado e instante. **[não decidível]** ([diff_e3_sombra.py:47](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:47).) Uma discordância efetiva nessas condições violaria a propriedade de igualdade entre leitores, válida para qualquer dado pela L-114. ([LEIS.md:152](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/docs/LEIS.md:152).)

**4. Classes e reconciliação**

Quem atribui as classes é **`casos_do_par`**, usando fila da célula, perguntas vivas C6, suas âncoras e a razão da tarefa. Não são classificações produzidas pelos quatro leitores. ([diff_e3_sombra.py:63](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:63).)

| Classe | Condição no código | Contagem no texto |
|---|---|---:|
| `todos` | Sempre | 344 |
| `so_fila_sem_pergunta` | Fila da célula, sem pergunta viva C6 | 54 |
| `fila_A_pergunta_B` | Fila da célula e âncora viva na fila global fora desse conjunto | 8 |
| `verbos_A_B` | Exatamente o mesmo predicado da anterior | 8 |
| `isento_fila_residual` | Razão `isento_ponto` e fila residual | 0 |

Predicados: [diff_e3_sombra.py:64](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:64). Contagens: **[não verificado: anexo colado]**.

A reconciliação é: `so_fila` = G2(53) + G3(1); cada classe A/B = G3(4) + G4(1) + G7(2) + G10(1). As ocorrências de etiquetas somam **414**, porque há sobreposição; os grupos somam **344 colab-dias**. Não reconciliam os **351** declarados. **[não verificado: anexo colado]**

**5. Quem escreve e quem lê**

Aqui “escrever” significa montar a saída em memória. `sem_porta`, `sem_fio` e `exige_admin` são campos fabricados pela projeção do comparador. ([diff_e3_sombra.py:46](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:46).)

| Campo | Escritor lógico | Leitor real |
|---|---|---|
| `fio` | Cópia: donos das ações, [tarefa_do_dia.py:552](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:552). C17: verbos, [juizes.py:1633](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1633). | Folha: [fora_da_folha.py:86](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/folha/fora_da_folha.py:86). Vigia: [vigia_fio.py:648](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/vigia_fio.py:648). |
| `fila` | Cópia: [tarefa_do_dia.py:550](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:550). C17: [juizes.py:1686](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1686). | Folha, números dos chamados publicados: [fora_da_folha.py:235](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/folha/fora_da_folha.py:235). |
| `rota` | Cópia: [tarefa_do_dia.py:555](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:555). C17: [juizes.py:1687](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1687). | Vigia, contador por rota: [vigia_fio.py:652](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/vigia_fio.py:652). |
| `sem_porta` | `porta is None`: [diff_e3_sombra.py:56](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:56). | Beco aplica o teste sobre a porta, além de suas outras condições: [cadastro_realidade.py:497](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/escala/services/cadastro_realidade.py:497). |
| `sem_fio` | `porta is None`: [diff_e3_sombra.py:59](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:59). | Pauta compara o fio da folha com `FIO_SEM_PORTA`: [pautas_dos_discordantes.py:79](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/ponto/services/pautas_dos_discordantes.py:79). |
| `exige_admin` | `porta.fio == 'admin'`: [diff_e3_sombra.py:55](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:55). | Vigia usa a mesma comparação: [vigia_fio.py:653](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/vigia_fio.py:653). |

As distinções semânticas relevantes são:

- **Turno × civil:** a tarefa usa `data_evento` como dia do turno, explicitamente sem reinterpretá-lo como dia civil. `data_civil` é separado, derivado do instante local do marco. C17 recolhe perguntas por `celula_id`. O anexo não permite conferir a correspondência entre esses vínculos. ([tarefa_do_dia.py:219](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:219), [tarefa_do_dia.py:104](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:104), [juizes.py:1599](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1599).)
- **Fila global × janela:** `fila_ids` e a partição dos verbos vêm do universo global; somente `fila_cel` é recortada às células solicitadas. A tupla pública `fila` significa chamados visíveis da própria célula, não todas as âncoras globais. ([juizes.py:1557](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1557), [fora_da_folha.py:235](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/folha/fora_da_folha.py:235).)
- **Sem porta ≠ sem tarefa:** pode existir ação do sistema ou tarefa indeterminada sem porta publicada. Logo `sem_fio=True` não prova inexistência de pendência. ([tarefa_do_dia.py:559](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:559), [fora_da_folha.py:52](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/folha/fora_da_folha.py:52).)
- **Instante:** o próprio comparador declara que o teste C17 “no app” lê o relógio da máquina, enquanto a tarefa recebe `agora`. Portanto nem essa equivalência temporal está assegurada pelo instrumento. ([diff_e3_sombra.py:107](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:107).)

## 13/11 16:30 · sessao 5c944161d1b8 · - · - · origem=Ronald (tmux)

344 linhas: 35 + 68 + 30 + 65 + 125 + 4 + 2 + 7 + 7 + 1. Minha transcrição inicial omitiu os PKs 107710 e 130818; corrigidos. Diferença para `PAR_TOTAL diferentes 351`: 7 linhas. [não verificado: contagem do texto do prompt, sem arquivo local]

## 13/11 16:29 · sessao e3b2fb8631ff · - · - · origem=Ronald (tmux)

Contagem independente do texto herdado: **342 linhas**, PKs únicos. Confirmados: fila cresce35; chamado/nada68; colab/nada30; admin/nada com fila65; admin/colab sem fila123; admin/colab fila cresce4; rota+fila2; admin/nada porta_pergunta7; chamado/colab7; admin/colab+rota+fila1. [não verificado: dado transcrito do prompt, sem arquivo local]

Classes:54 `so_fila_sem_pergunta`,8 `fila_A_pergunta_B`,8 `verbos_A_B`,0 `isento_fila_residual`. [não verificado: dado do prompt]

Os dois PKs que explicariam a diferença para sua contagem344 —115231 e107710— **não constam na versão do prompt herdada que recebi**; não os acrescentei por sequência. [não verificado]

_4687 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
