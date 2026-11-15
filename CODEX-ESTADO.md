# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 19:40** · publicado em **15/11/2026 19:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 19:40 · sessao dd6001c0d496 · - · - · origem=Ronald (tmux)

Todas as referências abaixo são relativas à raiz da cópia; não executei testes nem fiz alterações. [nao verificado]

**Chamadores de `censo`**

| Classe | Sítios |
|---|---|
| Produção direta | Import e chamada no command: `app/folha/management/commands/vigiar_retido_calado.py:14`, `:33`. |
| Produção indireta | Cron `ENC(... args='--alarme')`: `app/config/crons.py:1681`; dependência do lavrador `:1683`; papel `vigia` `:2260`. |
| Testes ORM diretos | Helpers `app/folha/tests/test_vigia_retido_calado.py:29`, `:161`; chamadas `:90`, `:126`, `:139`, `:140`; command interceptado com `wraps=vrc.censo` `:148`. |
| Testes ORM pelo command | `app/folha/tests/test_vigia_retido_calado.py:62`, `:65`, `:67`, `:149`. |
| Testes sem banco, função real extraída | `app/chamados/tests/test_e3_r3_c4_sem_banco.py:154`, `:157`; `test_e3_r3_c5_sem_banco.py:103`; `test_e3_r3_c6_sem_banco.py:253`; `test_e3_r3_c14_sem_banco.py:350`; `test_e3_r3_c15_sem_banco.py:85`; `test_e3_r3_c16_sem_banco.py:74`. |
| Chamadas negativas de assinatura | `app/chamados/tests/test_e3_r3_c5_sem_banco.py:243`, `:245`: sem `hoje`, devem continuar `TypeError`. |
| Testes do command com censo substituído | `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `:281`; `test_e3_r3_c14_sem_banco.py:317`, `:321`. |

**Retorno e leitores**

`censo` escreve as nove chaves: `retratos`, `competencias_sem_retrato`, `bloqueados`, `calados`, `roteados`, `dias_sem_leitura_sistema`, `linhas_sem_campo_leitura`, `por_empresa`, `cego` (`app/folha/vigia_retido_calado.py:61`, `:62`, `:88`, `:89`).

| Saída | Quem escreve | Quem lê e efeito de 4.2 |
|---|---|---|
| `retratos`: `(empresa,mes,ano)` | `app/folha/vigia_retido_calado.py:76` | `linhas` imprime quantidade `:95`; `cego` usa vazio `:89`. Passa a conter somente fotos nas visíveis/interseção e nas janelas. |
| `competencias_sem_retrato`: `(empresa,mes,ano)` | `app/folha/vigia_retido_calado.py:58`, `:61` | `linhas` conta `:95` e nomeia cada par `:96`. Precisa passar a incluir empresa visível sem **nenhuma** foto; hoje loop só existe a partir das fotos `:51`. |
| `bloqueados`, `calados`, `roteados` | `app/folha/vigia_retido_calado.py:77`, `:78`, `:79` | `linhas`: universo `:95`, roteados `:101`, contador `:102`; command alarma `calados` somente com passivo, `app/folha/management/commands/vigiar_retido_calado.py:17`, `:37`. Valores podem mudar pela seleção de fotos. |
| `dias_sem_leitura_sistema` | `app/folha/vigia_retido_calado.py:88` | `linhas` `:103`, `:105`; command passivo `app/folha/management/commands/vigiar_retido_calado.py:17`. Mantém positivo conhecido; `None` só sem soma conhecida e com legado `app/folha/vigia_retido_calado.py:88`. |
| `linhas_sem_campo_leitura` | `app/folha/vigia_retido_calado.py:80` | `linhas` `:104`, `:105`; muda somente seleção do universo. |
| `por_empresa`: chave `(empresa,ano,mes)` | `app/folha/vigia_retido_calado.py:83` | `linhas` `:98`, `:99` lê mês/ano/bloqueados/calados/roteados. Campos aninhados de leitura são cobrados nos testes `app/chamados/tests/test_e3_r3_c6_sem_banco.py:269`, `test_e3_r3_c14_sem_banco.py:309`, `:352`. **Ordem difere** de `retratos` e `competencias_sem_retrato`. |
| `cego` | `app/folha/vigia_retido_calado.py:89` | `linhas` `:102`; único estrutural atual, `app/folha/management/commands/vigiar_retido_calado.py:16`, acionado `:38`. Não identifica cobertura parcial se já existe um bloqueado. |

O contador de ausências **já é impresso**, mas ainda não é estrutural no alarme: o command só tem `ALARMA_SEMPRE=('cego',)` (`app/folha/management/commands/vigiar_retido_calado.py:16`), enquanto `competencias_sem_retrato` é apenas listado (`app/folha/vigia_retido_calado.py:94`, `:96`). Para cumprir “par sem retrato no contador estrutural”, é preciso consumir a falta no alarme, preservando sua lista nomeada; a alternativa exata de nome/schema adicional não foi decidida no texto **[nao verificado]**.

**Cron, placar, tela, RELATO, contador**

- Cron continua chamando o command com `--alarme`; comentário “só pelo cego” fica incorreto se a falta de par virar estrutural (`app/config/crons.py:1679`, `:1681`; `app/folha/management/commands/vigiar_retido_calado.py:5`).
- Placar não recebe o dict: `encadeado` passa pelo envelope (`bin/encadeado.sh:102`); o envelope grava `nome/emp/ini/fim/dur/exit` (`bin/cron_run.sh:13`); `pipeline_placar` conta `exit==2` como alarme (`app/core/management/commands/pipeline_placar.py:77`). Assim mudam potencialmente rc/alarmes/duração, sem nova chave no JSON do placar.
- Tela consulta fotos diretamente (`app/folha/views_retrato.py:192`), e frescor separado (`:210`); template mostra `frescor_aviso[*].rotulo_admin` (`app/templates/folha/_retrato_barra.html:22`). Não é migração de consumidor VRC: permanece divergência de universo porque aviso só aceita empresas com foto (`app/folha/views_retrato.py:209`, `:211`).
- `RELATO` registra medidas históricas, não é consumidor executável: explica origem/unidade e command (`app/docs/RELATO.md:4347`, `:4349`), conserva smoke histórico (`:4353`, `:1644`). Não inferir números novos desses registros **[nao verificado]**.
- Contador separado `retratos_desatualizados` é `len(frescor)` com `desatualizado` **ou** `nunca lavrado`, sem VRC (`app/folha/retrato_folha.py:1448`, `:1454`).

**Testes que precisam mudar ou ser ampliados** [nao verificado]

1. Empresa sem qualquer foto: exigir pares nomeados; hoje só exige `cego=1` (`app/folha/tests/test_vigia_retido_calado.py:31`, `:33`; sem banco `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`, `:113`).
2. Alarme de cobertura parcial: hoje aceita `--alarme` com só foto 10 e falta 11 (`app/folha/tests/test_vigia_retido_calado.py:64`, `:65`); passa a precisar foto 11 ou esperar rc2.
3. Caso de falta parcial preserva positivo (`app/folha/tests/test_vigia_retido_calado.py:186`, `:190`; sem banco `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`, `:108`): acrescentar verificação estrutural/command.
4. Selo compartilhado prova apenas helper de competências, não empresas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:127`, `:133`, `:134`): acrescentar visíveis/interseção e empresa sem fotos.
5. Adaptar fixtures sem banco ao import/consulta de `empresas_visiveis`; hoje c4/c5/c6/c14/c15/c16 só simulam fotos na fase leitora (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:149`; `test_e3_r3_c5_sem_banco.py:96`; `test_e3_r3_c6_sem_banco.py:246`; `test_e3_r3_c14_sem_banco.py:341`; `test_e3_r3_c15_sem_banco.py:77`; `test_e3_r3_c16_sem_banco.py:63`).
6. Mock c11 precisa chave estrutural se command mantiver acesso `d[k]`; retorno atual tem apenas `cego/calados/dias_sem_leitura_sistema` (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`; acesso `app/folha/management/commands/vigiar_retido_calado.py:38`).
7. Mutantes de seleção/falta c16 dependem de strings do loop antigo e precisam alvo vivo se refatorado (`app/chamados/tests/e3_r3_c16_mutantes.py:26`, `:28`). Mutantes de c4/c5/c6/c14 preservam contratos de balde/unidade/desconhecido (`e3_r3_c4_mutantes.py:33`, `e3_r3_c5_mutantes.py:44`, `e3_r3_c6_mutantes.py:45`, `e3_r3_c14_mutantes.py:53`).

**Insumos: escritor/leitor e significado**

- `Empresa.ativa/cnpj/razao_social`: visibilidade definida por `empresas_visiveis` = ativa, exclui CNPJ `11222333*`, ordena razão social (`app/colaboradores/queries.py:85`). Escritores desses campos não investigados **[nao verificado]**.
- `--empresa`: command escreve lista de IDs ou `None` (`app/folha/management/commands/vigiar_retido_calado.py:31`); censo lê filtro (`app/folha/vigia_retido_calado.py:47`). Lavrador usa uma PK intersectando visíveis (`app/folha/management/commands/lavrar_retrato_folha.py:51`, `:53`).
- `hoje`: command escreve pelo relógio da borda (`app/folha/management/commands/vigiar_retido_calado.py:33`); helper usa para janelas (`app/folha/retrato_folha.py:1379`), balde usa para hoje/futuro (`:491`), C5 recebe (`app/folha/vigia_retido_calado.py:60`).
- Foto `empresa/mes/ano/linhas/lavrado_em`: escritor lavrador `update_or_create` por trio, defaults linhas/carimbo (`app/folha/management/commands/lavrar_retrato_folha.py:77`); unicidade garantida (`app/folha/models.py:366`). Censo lê empresa/mes/ano/linhas (`app/folha/vigia_retido_calado.py:51`, `:56`, `:65`), não usa `lavrado_em` para universo; frescor lê carimbo (`app/folha/retrato_folha.py:1413`).
- Linhas `colab/status/motivo/dias/datas/dias_sem_leitura/dias_sem_porta/dono`: montador escreve (`app/folha/retrato_folha.py:1237`); VRC pede bloqueados pela porta `linhas_do_retrato` (`app/folha/vigia_retido_calado.py:65`), balde por `dono_da_linha` (`:66`), C5 pelo `colab` (`:67`), dias sem leitura por presença e tamanho (`:72`, `:73`). `dono` gravado significa **fio**, não balde (`app/folha/retrato_folha.py:1246`, `:1248`; leitor `:446`, `:486`).
- C5: hoje recebe IDs originais; `None` significa **todas** em `colabs_nunca_bateu`, inclusive ocultas (`app/folha/vigia_retido_calado.py:60`; `app/escala/services/cadastro_realidade.py:163`). C17 deve passar IDs efetivamente visíveis/intersectados para alinhar empresas; sua janela continua anterior até ontem (`cadastro_realidade.py:162`, `:167`), não “competência do dia civil”.
- Não há mesmo significado em todos os consumidores: consulta documenta que export/internos não usam visibilidade de propósito (`app/colaboradores/queries.py:82`); vigia hoje parte de todas as fotos (`app/folha/vigia_retido_calado.py:46`), frescor usa visíveis (`app/folha/retrato_folha.py:1410`), tela por PK pode pegar oculta (`app/folha/views_retrato.py:161`). Competência é fim da janela por empresa (`app/folha/retrato_folha.py:1380`), enquanto filtro civil `lavrado_em` é explicitamente recusado (`app/folha/vigia_retido_calado.py:23`).

## 15/11 19:39 · sessao bb4d3cb27c21 · - · - · origem=Ronald (tmux)

(b) **O conjunto de correções de relógio não coincide com “todos os sítios de relógio no salto 1”**; o anexo distingue leituras próprias, fallbacks suprimidos e omissões, e o código confirma essa distinção (`app/relatorios/furos.py:59`, `app/colaboradores/services/calendario.py:192`, `app/chamados/juizes.py:888`).

| Classificação | Sítio e motivo |
|---|---|
| **DENTRO correto** | `furos_espelho → escalas_em_limbo`: salto 1, leitura própria `timezone.localdate()`, embora a view já tenha `_agora` e `hoje`; falta transmitir o dia recebido (`app/relatorios/views.py:1519`, `app/relatorios/views.py:1520`, `app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`). |
| **FALTA ao conjunto de sítios salto 1, mas FORA expresso** | `retrato_pdf → gerar_pdf_auditavel`: salto 1, outra chamada `timezone.now()`; `_recorte_da_barra` só devolve `hoje`, não `_agora` (`app/folha/views_retrato.py:245`, `app/folha/views_retrato.py:386`, `app/folha/views_retrato.py:441`, `app/relatorios/pdf_base.py:133`). |
| **FALTA ao conjunto de sítios salto 1, mas FORA expresso** | `retrato_raiox → placar_aceite`: salto 1, **duas** leituras próprias, `.now()` e `.localdate()`, no ramo `dono == ['colab']` (`app/folha/views_retrato.py:511`, `app/folha/views_retrato.py:513`, `app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:287`). |
| **Sítio salto 1 ausente da lista; fallback já suprimido** | `calendario_ponto/contexto_drawer → contexto_calendario`: o helper contém `.now()`, mas ambas as bordas fornecem `agora`; não representa nova leitura nesses caminhos (`app/colaboradores/views.py:1952`, `app/colaboradores/services/drawer.py:123`, `app/colaboradores/services/drawer.py:124`, `app/colaboradores/services/calendario.py:192`). |
| **Sítio salto 1 adicional se incluídas as entradas auxiliares do anexo** | `ListaDoAdmin._carregar → cadastro_realidade.lista`: `.now()` no salto 1; fica suprimido quando `self.agora` foi fornecido, mas o construtor permite `None` (`app/chamados/services/vigia_fio.py:177`, `app/chamados/services/vigia_fio.py:179`, `app/chamados/services/vigia_fio.py:194`, `app/escala/services/cadastro_realidade.py:780`). |
| **SOBRA somente se a lista for intitulada “sítios de relógio salto 1”** | `dataset_furos` não lê relógio: tem `agora=None` e repassa o argumento ao D4 e calendário; o D4 está no salto 2 da view. Tornar `agora` obrigatório é endurecimento de contrato válido, não remoção de relógio próprio no salto 1 (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`). |

**Não classificaria selo e docstrings como SOBRA da obra:** são instrumentos e documentação, não nós de relógio do grafo; o defeito do selo é concreto, pois `_le_relogio` usa conjunto de nomes e `_instante_da_borda` conta alvos de atribuição (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:182`). O selo do drawer só compara `['now']`; o do relatório combina esse conjunto com a quantidade de alvos, sem contar todas as chamadas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:152`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:156`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:158`).

**Não são relógios salto 1 faltantes:** `viva_em_disputa_fechada` e `carimbos_sem_lastro_por_dono` chamam `pergunta_viva` sem argumentos no salto 1, mas `pergunta_viva` não consulta relógio: apenas deriva o dia se recebeu instante; os relógios surgem nos degraus seguintes (`app/chamados/services/vigia_fio.py:718`, `app/chamados/services/vigia_fio.py:776`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:897`). A propriedade `estado` é salto 2 a partir de `dono_fora_do_fio`, não salto 1 dos censos principais (`app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:387`, `app/chamados/services/vigia_fio.py:615`).

A identidade exata de “docstrings G3” não aparece nomeada nos arquivos examinados **[nao verificado]**. Documentação evidentemente afetada por 4.2: o vigia promete “universo do retrato gravado” e descreve faltantes, embora derive empresas apenas dos retratos existentes; a docstring do helper promete que os três consumidores medem o mesmo universo (`app/folha/vigia_retido_calado.py:16`, `app/folha/vigia_retido_calado.py:27`, `app/folha/vigia_retido_calado.py:51`, `app/folha/retrato_folha.py:1372`). O help text do carimbo já diz “INICIO da rodada”, também na migração declarada apenas de help text (`app/folha/models.py:358`, `app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:1`, `app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:16`).

## 15/11 19:39 · sessao 94c8f53bb9d0 · - · - · origem=Ronald (tmux)

A comparação exata `f31a8d09e × base da obra` fica **[nao verificado]**: não há metadados Git nesta cópia; os artefatos identificam bases parciais distintas — LU3 `522b7eb48` (`app/chamados/tests/e3_lu3_censo.txt:3`; `app/chamados/tests/e3_lu3_entrega.txt:2`), LU2 `de702cb2f` (`app/chamados/tests/e3_lu2_censo.txt:1`) e c12 `20f5a0338` (`app/chamados/tests/e3_r3_c12_entrega.txt:1`).

Outros leitores que montam universos, confirmados no código:

| Leitor | Universo próprio e divergência |
|---|---|
| `_recorte_da_barra`, compartilhado por tela/PDF/raiox | Dropdown visível, mas seleção por PK não cruza visibilidade; `todas` usa a competência atual da primeira empresa; URL escolhe outro par; navegação calcula ±1 mês (`app/folha/views_retrato.py:147`, `:153`, `:157`, `:161`, `:164`, `:179`, `:181`, `:189`, `:192`). As três views chamam o recorte (`app/folha/views_retrato.py:258`, `:386`, `:487`). |
| Aviso de frescor | Refiltra o frescor pelas empresas com foto existente, omitindo justamente as sem retrato (`app/folha/views_retrato.py:209`, `:211`). |
| Painel do colaborador | Empresa do colaborador cruzada com visíveis, competência exata da URL; não percorre as duas janelas (`app/colaboradores/views.py:975`, `:977`, `:982`). |
| `vigia_fio.censo_alcance` | Visíveis com interseção, mas usa `competencias_a_lavrar`, cuja anterior só entra se não entregue (`app/chamados/services/vigia_fio.py:517`, `:520`, `:535`; `app/ponto/services/he_pendente_lavrado.py:220`, `:223`). |
| `vigia_fio.censo` antigo | Chamados vivos com célula, sem cruzar visibilidade quando `empresas=None`; não escolhe duas competências (`app/chamados/services/vigia_fio.py:290`, `:292`). |
| `ListaDoAdmin` | Cruza novamente empresas visíveis e o recorte fornecido (`app/chamados/services/vigia_fio.py:191`, `:193`, `:194`). |
| C5 `colabs_nunca_bateu` | `None` significa todas as empresas; seleciona células entre início da anterior e ontem. Em 4.2 deve receber os IDs **efetivos visíveis**, para não conservar o universo largo subsidiário (`app/escala/services/cadastro_realidade.py:162`, `:163`, `:167`; chamada atual `app/folha/vigia_retido_calado.py:60`). |
| `contexto_drawer` | Recompõe atual e anterior por `_ja(atual_ini-1dia)`, com piso visual próprio (`app/colaboradores/services/drawer.py:111`, `:115`, `:116`, `:118`, `:123`). |
| `contexto_calendario` | Default mês/ano civil; modo competência consulta `janela_fechamento`; demais modos constroem períodos próprios (`app/colaboradores/services/calendario.py:194`, `:205`, `:209`, `:210`, `:227`, `:239`). |
| Consulta plural do calendário | Calcula janela atual da empresa para excluir dias correntes da oferta (`app/colaboradores/services/calendario.py:140`, `:143`, `:150`). |
| `CargaDaTarefa._ler_exportadas` | Deriva competência de cada dia recebido, sem usar as duas competências de hoje (`app/chamados/services/tarefa_do_dia.py:467`, `:471`, `:479`, `:482`). |
| `escalas_em_limbo` | Universo de escalas ativas, recorte opcional de empresa e foto `FolgaDia` do mês civil (`app/relatorios/furos.py:60`, `:63`, `:86`, `:88`). |

Os módulos `vigia_fio`, calendário, drawer e furos constam do diff **parcial LU3** documentado (`app/chamados/tests/e3_lu3_entrega.txt:151`, `:163`, `:164`, `:168`); pertença de cada função ao diff integral solicitado: **[nao verificado]**.

Escritores e significado das fontes usadas por 4.2:

- `Empresa.ativa`, `cnpj` e `dia_inicio_competencia` são campos definidos em `app/colaboradores/models.py:43`, `:44`, `:46`. A tela recebe os valores e chama `atualizar_empresa`/`criar_empresa` (`app/colaboradores/views.py:492`, `:493`, `:496`, `:521`, `:532`); as portas gravam por `create`/`save` (`app/colaboradores/services/estrutura.py:13`, `:15`, `:18`, `:21`). Também existe escritor explícito de normalização do CNPJ (`app/colaboradores/services/estrutura.py:25`, `:28`), e `EmpresaAdmin` deixa apenas `regime_trabalhista` somente leitura (`app/colaboradores/admin.py:8`, `:19`).
- `empresas_visiveis` lê `ativa=True` e exclui CNPJ `11222333*` (`app/colaboradores/queries.py:85`, `:86`); o contrato documentado do helper diz que integrações e lógica interna não o usam deliberadamente (`app/colaboradores/queries.py:79`, `:82`). Logo, a equivalência “empresa = visível” **não é universal** (`app/escala/services/cadastro_realidade.py:163`; `app/chamados/services/vigia_fio.py:290`).
- `competencias_do_retrato` deriva mês/ano do fim de anterior/atual, com deduplicação (`app/folha/retrato_folha.py:1377`, `:1380`, `:1382`). As janelas leem o corte empresarial; competência corresponde ao mês do **fim**, não ao mês civil de hoje (`app/ponto/janelas.py:10`, `:16`, `:36`, `:50`, `:60`, `:66`).
- `RetratoFolha.empresa/mes/ano/linhas/lavrado_em` são gravados pelo lavrador via `update_or_create`; `lavrado_em` recebe o instante inicial da rodada (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:77`, `:79`). A chave única é empresa/mês/ano (`app/folha/models.py:366`). Frescor lê o par e o carimbo (`app/folha/retrato_folha.py:1412`, `:1413`); vigia lê o par e as linhas, sem selecionar por `lavrado_em` (`app/folha/vigia_retido_calado.py:51`, `:55`, `:65`).
- O lavrador explícito pode gravar competência fora das duas janelas (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `:58`); a tela e o painel podem lê-la por URL (`app/folha/views_retrato.py:181`, `:192`; `app/colaboradores/views.py:975`, `:982`). Portanto, “competência lida = competência das duas janelas automáticas” também **não é universal**.

## 15/11 18:53 · sessao 3b3ebdd4be3c · E3-RESTO-R3 · censo · origem=Code

Os caminhos abaixo são relativos a `app/`. Distingo **leitura própria**, **fallback suprimido pelo argumento recebido** e **omissão de argumento**. Execução efetiva em produção: **[não verificado]**.

O caminho solicitado `folha/vigia_fio.py` não foi localizado **[não verificado]**. Incluí separadamente o módulo encontrado, `chamados/services/vigia_fio.py`, cujas entradas são `censo` e `censo_alcance`; equivalência com o caminho solicitado: **[não verificado]** (`chamados/services/vigia_fio.py:266`, `chamados/services/vigia_fio.py:487`).

**1. Relógio transitivo — até seis chamadas**

**Instante disponível nas bordas**

| Borda | O que tem em mãos |
|---|---|
| `lavrar_retrato_folha.Command.handle` | `_agora` e seu dia local (`folha/management/commands/lavrar_retrato_folha.py:49`, `folha/management/commands/lavrar_retrato_folha.py:50`). |
| `vigiar_retido_calado.Command.handle` | Lê somente o dia, diretamente no argumento de `censo`; não conserva um `datetime` (`folha/management/commands/vigiar_retido_calado.py:33`). |
| `_recorte_da_barra` | `_agora` e `_hoje`; devolve o dia às views, mas não devolve `_agora` (`folha/views_retrato.py:177`, `folha/views_retrato.py:178`, `folha/views_retrato.py:238`, `folha/views_retrato.py:245`). |
| `calendario_ponto` | `_agora`, passado ao calendário (`colaboradores/views.py:1951`, `colaboradores/views.py:1952`). |
| `contexto_drawer` | `agora` e dia local (`colaboradores/services/drawer.py:21`, `colaboradores/services/drawer.py:22`). |
| `furos_espelho` | `_agora`, passado ao dataset (`relatorios/views.py:1519`, `relatorios/views.py:1543`). |
| `frescor_dos_retratos` | `agora` obrigatório (`folha/retrato_folha.py:1386`). |
| `vigia_fio.censo` / `censo_alcance` | O primeiro recebe ou lê `agora`; o segundo o exige (`chamados/services/vigia_fio.py:266`, `chamados/services/vigia_fio.py:282`, `chamados/services/vigia_fio.py:487`). |

**Lavrador**

- **Salto 0:** `handle → timezone.now()` — leitura da borda (`folha/management/commands/lavrar_retrato_folha.py:49`).
- **Salto 3:** `handle → contadores → linhas_do_retrato → recortar`. `recortar` aceita `hoje=None`, e `linhas_do_retrato` o omite. Aqui só há eixo `f` ou nenhum grupo; não se consulta o leitor de dono (`folha/management/commands/lavrar_retrato_folha.py:68`, `folha/retrato_folha.py:120`, `folha/retrato_folha.py:121`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:635`, `folha/retrato_folha.py:632`).
- **Salto 4:** `handle → montar_linhas → _informativo_rescisao → informativo_rescisao → _dias_sem_decisao`. Contém `_tz.localdate()`, mas **o fallback está suprimido**: o dia da borda atravessa todas essas chamadas (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1220`, `folha/retrato_folha.py:1288`, `folha/export.py:1228`, `folha/export.py:1238`).
- **Salto 5:** `handle → montar_linhas → fora_da_folha → dias_que_seguram → classificar_export → motivos_retencao_espelho`. A última chamada **omite `hoje`**, cujo default é `None`. `motivos_retencao_celula` fica no salto 6; seu `_dias_sem_decisao`, com relógio, fica no **salto 7**, fora deste teto (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1214`, `folha/fora_da_folha.py:200`, `folha/fora_da_folha.py:147`, `folha/export.py:968`, `folha/export.py:1301`, `folha/export.py:1307`, `folha/export.py:1212`).
- **Salto 6:** `handle → montar_linhas → fora_da_folha → _carga → carga_da_tarefa → CargaDaTarefa.__init__ → qs_na_fila`. Contém `timezone.now()`, **suprimido pelo `agora` propagado**. O construtor é chamado pela classe devolvida pela fábrica local (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1214`, `folha/fora_da_folha.py:205`, `folha/fora_da_folha.py:67`, `chamados/services/tarefa_do_dia.py:730`, `chamados/services/tarefa_do_dia.py:452`, `chamados/services/painel.py:190`).

**Vigia do retido calado**

- **Salto 0:** `handle → timezone.localdate()` — leitura da borda (`folha/management/commands/vigiar_retido_calado.py:33`).
- **Salto 2:** `handle → censo → colabs_nunca_bateu`. Contém `timezone.localdate()`, **suprimido pelo `hoje` posicional recebido** (`folha/management/commands/vigiar_retido_calado.py:33`, `folha/vigia_retido_calado.py:60`, `escala/services/cadastro_realidade.py:161`).
- **Salto 3:** `handle → censo → linhas_do_retrato → recortar`. **Omite `hoje=None`**, no mesmo caminho restrito ao eixo `f` descrito acima (`folha/management/commands/vigiar_retido_calado.py:33`, `folha/vigia_retido_calado.py:65`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:635`).

**Recorte e suas três views**

- `_recorte_da_barra → timezone.now()` é a leitura direta. `retrato_folha`, `retrato_pdf` e `retrato_raiox` chegam a ela por suas chamadas ao recorte (`folha/views_retrato.py:177`, `folha/views_retrato.py:258`, `folha/views_retrato.py:386`, `folha/views_retrato.py:487`).
- `_recorte_da_barra → linhas_do_retrato → recortar` **omite `hoje=None`**, no salto 2 do recorte e no salto 3 de cada view. Novamente, não consulta o eixo de dono nesse caminho (`folha/views_retrato.py:198`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:632`, `folha/retrato_folha.py:635`).
- `retrato_pdf → gerar_pdf_auditavel → timezone.now()` acrescenta uma **leitura própria**, no salto 1. A view já recebeu o dia do recorte; o instante completo não veio no retorno (`folha/views_retrato.py:386`, `folha/views_retrato.py:441`, `folha/views_retrato.py:245`, `relatorios/pdf_base.py:133`).
- `retrato_raiox → placar_aceite` acrescenta **duas leituras próprias**, `_tz.now()` e `_tz.localdate()`, no salto 1, quando `dono == ['colab']`. O import de `placar_aceite` é local (`folha/views_retrato.py:511`, `folha/views_retrato.py:512`, `folha/views_retrato.py:513`, `chamados/services/verbos.py:286`, `chamados/services/verbos.py:287`).
- `frescor_dos_retratos` não acrescenta leitura autônoma nesse fecho: converte `agora`, passa data explícita às janelas e formata carimbos explícitos (`folha/retrato_folha.py:1408`, `folha/retrato_folha.py:1411`, `folha/retrato_folha.py:1441`, `folha/retrato_folha.py:1444`, `ponto/janelas.py:60`, `ponto/janelas.py:66`).

**Família compartilhada por calendário, drawer e relatório**

Use estes prefixos para as cadeias da tabela:

- **K:** `calendario_ponto → contexto_calendario`, passando `agora` (`colaboradores/views.py:1952`).
- **D:** `contexto_drawer → contexto_calendario`, passando `agora` (`colaboradores/services/drawer.py:123`, `colaboradores/services/drawer.py:124`).
- **F:** `furos_espelho → dataset_furos → contexto_calendario`, passando `agora` nas duas arestas (`relatorios/views.py:1543`, `relatorios/furos.py:160`, `relatorios/furos.py:164`).

| Cadeia depois do prefixo | Salto K/D; F | Leitura ou omissão |
|---|---:|---|
| `contexto_calendario` | 1; 2 | `timezone.now()` **suprimido** pelo instante recebido (`colaboradores/services/calendario.py:192`). |
| `contexto_calendario → status_dos_dias_lavrados` | 2; 3 | `now()` **suprimido**; recebe `_agora` (`colaboradores/services/calendario.py:638`, `ponto/status_dia.py:252`, `ponto/status_dia.py:253`). |
| `contexto_calendario → pergunta_viva` | 2; 3 | Import local com apelido `_pv`; **omite `hoje=None`**, mas fornece `agora`, do qual o juiz deriva o dia (`colaboradores/services/calendario.py:498`, `colaboradores/services/calendario.py:506`, `chamados/juizes.py:865`, `chamados/juizes.py:888`). |
| `… → pergunta_viva → _pergunta_viva_calc → _dentro_da_janela_viva` | 4; 5 | `date.today()` **suprimido** pelo dia derivado do instante (`chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:595`). |
| `… → pergunta_viva → _pergunta_viva_calc → classificar_falta` | 4; 5 | Import local `_cf`; `_tz.now()` **suprimido** pelo `agora` recebido (`chamados/juizes.py:1470`, `chamados/juizes.py:1472`, `chamados/juizes.py:1474`, `escala/servico_jornada.py:387`). |
| `contexto_calendario → folha_manda → turno_aberto_de` | 3; 4 | `now()` **suprimido** pelo instante recebido (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:398`, `ponto/turnos.py:2291`). |
| `contexto_calendario → folha_manda → ate_ontem` | 3; 4 | `localdate()` **suprimido** pelo dia fornecido (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:413`, `relatorios/cartao_pela_celula.py:40`). |
| `contexto_calendario → folha_manda → totais_da_folha → fechamento_lido → recalcular_fechamento_mes` | 5; 6 | **Leituras próprias** em `timezone.now()` nas linhas 616 e 1086; o instante da borda não é transportado até o recálculo (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:323`, `relatorios/cartao_pela_celula.py:73`, `ponto/services/fechamento.py:1759`, `ponto/services/fechamento.py:616`, `ponto/services/fechamento.py:1086`). |
| `… → recalcular_fechamento_mes → motivos_retencao_espelho` | 6; 7, fora | Import local `_mre`; **omite `hoje=None`** (`ponto/services/fechamento.py:1066`, `ponto/services/fechamento.py:1067`, `folha/export.py:1301`). |
| `contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado` | 6; 7, fora | **Leitura própria de `date.today()`**, quando existe tipo de escala; inclui despacho pela instância `_carga` (`colaboradores/services/calendario.py:252`, `ponto/turnos.py:3036`, `ponto/turnos.py:1939`, `ponto/turnos.py:1633`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:117`, `ponto/juiz_batida.py:125`). |

Duas condições delimitam essa tabela:

- `folha_manda → totais_da_folha` exige competência inteira e fechamento existente. `recalcular_fechamento_mes` também contém relógios nas linhas 1143, 1161 e 1189, mas esses ramos de escrita ficam **suprimidos** por `somente_leitura=True` nesse caminho (`relatorios/cartao_pela_celula.py:321`, `relatorios/cartao_pela_celula.py:71`, `ponto/services/fechamento.py:1761`, `ponto/services/fechamento.py:1128`, `ponto/services/fechamento.py:1143`, `ponto/services/fechamento.py:1161`, `ponto/services/fechamento.py:1189`).
- Pelo prefixo **F**, os caminhos através de `folha_manda` são apenas alcançabilidade **sintática**: `NAO_PERGUNTADO` impede a execução desse bloco (`relatorios/furos.py:164`, `colaboradores/services/calendario.py:594`, `colaboradores/services/calendario.py:597`, `colaboradores/services/calendario.py:614`).

**Ramos adicionais do drawer e do relatório**

- `contexto_drawer → perguntas_vivas_de → pergunta_viva`: import local `_vivas_de` de `chamados.models`, reexportado de `chamados.juizes`. **Omite `hoje` e `agora`**, apesar de o drawer já possuir ambos (`colaboradores/services/drawer.py:21`, `colaboradores/services/drawer.py:79`, `colaboradores/services/drawer.py:80`, `chamados/models.py:4226`, `chamados/juizes.py:1504`, `chamados/juizes.py:865`).
- Esse ramo chega a `_dentro_da_janela_viva` e `classificar_falta` no **salto 4**, permitindo suas leituras próprias; pela perna `_pergunta_materializada_de_fato`, chega novamente aos dois no **salto 5** (`chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`, `chamados/juizes.py:1486`, `chamados/juizes.py:696`, `chamados/juizes.py:701`, `chamados/juizes.py:595`, `escala/servico_jornada.py:387`).
- Na mesma cadeia, `_pergunta_materializada_de_fato` **omite efetivamente `agora`** no salto 4, e `_slot_esperado` o omite no salto 5: os `**` condicionais viram `{}` quando o instante é `None`. As assinaturas têm default `None` (`chamados/juizes.py:1486`, `chamados/juizes.py:1487`, `chamados/juizes.py:665`, `chamados/juizes.py:755`, `chamados/juizes.py:344`).
- `contexto_drawer → turnos_do_colab → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: **`date.today()` próprio no salto 5**. O import local é `_tdc` (`colaboradores/services/drawer.py:44`, `colaboradores/services/drawer.py:47`, `ponto/turnos.py:1915`, `ponto/turnos.py:1939`, `ponto/turnos.py:1633`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:125`).
- `furos_espelho → escalas_em_limbo`: **`timezone.localdate()` próprio no salto 1**, embora `_agora` já exista na borda (`relatorios/views.py:1519`, `relatorios/views.py:1554`, `relatorios/furos.py:59`).
- `furos_espelho → dataset_furos → status_dos_dias_lavrados`: contém `now()` no **salto 2**, **suprimido** pelo instante recebido. O default `agora=None` de `dataset_furos` permanece, mas essa borda fornece o argumento (`relatorios/views.py:1543`, `relatorios/furos.py:103`, `relatorios/furos.py:123`, `ponto/status_dia.py:253`).

**Complemento: módulo real `chamados/services/vigia_fio.py`**

Para tornar as cadeias seguintes explícitas:

- **T(C/A):** `censo:359` ou `censo_alcance:593 → CargaDaTarefa.tarefa → resolver_dia_oferecido_nas_datas → contexto_calendario` (`chamados/services/vigia_fio.py:359`, `chamados/services/vigia_fio.py:593`, `chamados/services/tarefa_do_dia.py:613`, `colaboradores/services/calendario.py:157`).
- **L(C):** `censo → dono_fora_do_fio → dia_do_admin → _carregar → cadastro_realidade.lista` (`chamados/services/vigia_fio.py:387`, `chamados/services/vigia_fio.py:261`, `chamados/services/vigia_fio.py:202`, `chamados/services/vigia_fio.py:194`).
- **L(A):** `censo_alcance → dia_na_lista → _carregar → cadastro_realidade.lista` (`chamados/services/vigia_fio.py:621`, `chamados/services/vigia_fio.py:210`, `chamados/services/vigia_fio.py:194`).
- **P(C/A):** `censo:387` ou `censo_alcance:615 → dono_fora_do_fio → PerguntaDisputa.estado → pergunta_viva`. A propriedade **omite ambos os argumentos**; acesso à propriedade conta como chamada (`chamados/services/vigia_fio.py:387`, `chamados/services/vigia_fio.py:615`, `chamados/services/vigia_fio.py:259`, `chamados/models.py:3127`, `chamados/juizes.py:865`).

| Cadeia | Saltos C / A | Leitura e transporte do instante |
|---|---:|---|
| `censo → now()` | 0 / — | Fallback da própria borda, quando não recebeu `agora` (`chamados/services/vigia_fio.py:282`). |
| `censo/censo_alcance → carga_da_tarefa → CargaDaTarefa.__init__ → qs_na_fila` | 3 / 3 | `now()` **suprimido** pelo instante propagado (`chamados/services/vigia_fio.py:357`, `chamados/services/vigia_fio.py:592`, `chamados/services/tarefa_do_dia.py:730`, `chamados/services/tarefa_do_dia.py:452`, `chamados/services/painel.py:190`). |
| `T(C/A)` | 3 / 3 | `contexto_calendario:192`, fallback suprimido; o instante atravessa a função plural (`chamados/services/tarefa_do_dia.py:614`, `colaboradores/services/calendario.py:159`, `colaboradores/services/calendario.py:192`). |
| `T(C/A) → status_dos_dias_lavrados` | 4 / 4 | `now()` suprimido (`colaboradores/services/calendario.py:638`, `ponto/status_dia.py:253`). |
| `T(C/A) → folha_manda → turno_aberto_de / ate_ontem` | 5 / 5 | `now()` e `localdate()` suprimidos (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:398`, `relatorios/cartao_pela_celula.py:413`, `ponto/turnos.py:2291`, `relatorios/cartao_pela_celula.py:40`). |
| `… → tarefa → ler → alimentar_fila → particionar_verbos / qs_na_fila` | 4 / 4 | Função local `ler`; ambos os relógios suprimidos pelo instante recebido (`chamados/services/tarefa_do_dia.py:600`, `chamados/services/tarefa_do_dia.py:588`, `chamados/services/tarefa_do_dia.py:590`, `chamados/juizes.py:1573`, `chamados/juizes.py:1576`, `chamados/juizes.py:1584`, `chamados/services/verbos.py:35`, `chamados/services/painel.py:190`). |
| `L(C/A)` | 4 / 3 | `lista → now()`, suprimido pelo `self.agora` fornecido pelos censos (`chamados/services/vigia_fio.py:194`, `escala/services/cadastro_realidade.py:780`). |
| `L(C/A) → _jornada_que_mente` | 5 / 4 | **`date.today()` próprio**; `lista` passa somente empresas (`escala/services/cadastro_realidade.py:791`, `escala/services/cadastro_realidade.py:224`). |
| `L(C/A) → _afastado_sem_registro` | 5 / 4 | `localdate()` suprimido pelo `hoje` passado (`escala/services/cadastro_realidade.py:808`, `escala/services/cadastro_realidade.py:588`). |
| `L(C/A) → _afastado_sem_registro → afastados_hoje` | 6 / 5 | **`localdate()` próprio**: a chamada não fornece o dia (`escala/services/cadastro_realidade.py:592`, `colaboradores/services/situacao_exibida.py:45`). |
| `L(C/A) → _nunca_bateu → colabs_nunca_bateu` | 6 / 5 | `localdate()` suprimido pelo `hoje` propagado (`escala/services/cadastro_realidade.py:864`, `escala/services/cadastro_realidade.py:179`, `escala/services/cadastro_realidade.py:161`). |
| `L(A) → _jornada_que_mente → periodos_do_dia → intervalo_cadastrado` | 7, fora / 6 | **`date.today()` próprio**, quando existe tipo de escala (`escala/services/cadastro_realidade.py:238`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:125`). |
| `P(C/A) → _pergunta_viva_calc → _dentro_da_janela_viva / classificar_falta` | 5 / 5 | **Leituras próprias**: a propriedade perdeu o instante que os censos possuem (`chamados/models.py:3127`, `chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`, `chamados/juizes.py:595`, `escala/servico_jornada.py:387`). |
| `P(C/A) → _pergunta_viva_calc → _pergunta_materializada_de_fato →` esses dois leitores | 6 / 6 | Mesmas leituras próprias; `agora` é omitido pela expansão condicional (`chamados/juizes.py:1486`, `chamados/juizes.py:1487`, `chamados/juizes.py:696`, `chamados/juizes.py:701`). |

Ainda no fecho desses censos:

- `censo → vigia_carimbo.classificar → _pergunta_materializada_de_fato`: **omite `hoje` e `agora` no salto 2**. O import local `_juiz` passa por reexportação de `chamados.models`. Seus leitores `_dentro_da_janela_viva` e `classificar_falta` ficam no salto 3 e podem ler relógio próprio (`chamados/services/vigia_fio.py:415`, `chamados/services/vigia_carimbo.py:95`, `chamados/services/vigia_carimbo.py:106`, `chamados/models.py:4226`, `chamados/juizes.py:665`, `chamados/juizes.py:696`, `chamados/juizes.py:701`).
- `censo → classificar → _slot_esperado`: **omite `agora` no salto 2**; também existe a omissão condicional pela perna `_pergunta_materializada_de_fato → _slot_esperado` (`chamados/services/vigia_fio.py:415`, `chamados/services/vigia_carimbo.py:110`, `chamados/juizes.py:344`, `chamados/juizes.py:755`).
- `censo_alcance → dias_que_seguram → classificar_export → motivos_retencao_espelho`: **omite `hoje` no salto 3**. Seguindo `motivos_retencao_celula → _dias_sem_decisao`, alcança **`localdate()` próprio no salto 5**. Pela outra perna, `_dias_que_retem`, o mesmo leitor fica no salto 4, mas com fallback suprimido (`chamados/services/vigia_fio.py:537`, `folha/fora_da_folha.py:147`, `folha/export.py:968`, `folha/export.py:1307`, `folha/export.py:1212`, `folha/export.py:1238`, `folha/fora_da_folha.py:165`, `folha/fora_da_folha.py:121`).
- `censo_alcance → alimentar_portas → alimentar_fila → particionar_verbos / qs_na_fila`: relógios no **salto 3**, suprimidos pelo instante propagado (`chamados/services/vigia_fio.py:560`, `chamados/juizes.py:1619`, `chamados/juizes.py:1573`, `chamados/juizes.py:1576`, `chamados/services/verbos.py:35`, `chamados/services/painel.py:190`).
- `censo_alcance → alimentar_portas → pergunta_viva`: **omite `hoje` no salto 2**, mas fornece `agora`; seus leitores ficam no salto 4 e têm fallbacks suprimidos (`chamados/services/vigia_fio.py:560`, `chamados/juizes.py:1628`, `chamados/juizes.py:888`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`).
- `T(C/A) → pergunta_viva` **omite `hoje` no salto 4**, fornecendo o instante. `… → tarefa → _ler_vida → pergunta_viva` fornece ambos. As expansões condicionais posteriores fornecem `agora` nesses caminhos (`colaboradores/services/calendario.py:506`, `chamados/services/tarefa_do_dia.py:574`, `chamados/services/tarefa_do_dia.py:537`, `chamados/services/tarefa_do_dia.py:541`, `chamados/juizes.py:1487`, `chamados/juizes.py:755`).
- `censo/censo_alcance → tarefa → _ler_vida → carga_da_vida → CargaDaVida.__init__ → _slot_esperado`: **omite `agora` no salto 5**, mas passa `carga=self`; o juiz recupera o instante dessa carga (`chamados/services/vigia_fio.py:359`, `chamados/servic

[truncado em 20000 de 35668 caracteres]

## 15/11 18:49 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Confirmado:

- `_carregar → lista → _folga_nao_definida_sem_porta → beco_em_lote → alimentar_portas → alimentar_fila → particionar_verbos/qs_na_fila`: relógios no salto **6**, guardados pelo instante propagado (`app/chamados/services/vigia_fio.py:194`; `app/escala/services/cadastro_realidade.py:850`, `:559`, `:460`; `app/chamados/juizes.py:1619`, `:1573`, `:1576`, `:1584`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:190`).
- No mesmo prefixo, `alimentar_portas → pergunta_viva` omite `hoje` no salto **5**; recupera-o de `agora` (`app/chamados/juizes.py:1628`, `:888`, `:890`).
- `dia_do_admin`, `dia_na_lista` e `colab_na_lista` acrescentam **1**: omissão no salto **6**, relógios no **7**, fora (`app/chamados/services/vigia_fio.py:202`, `:210`, `:228`).
- `papel_do_dia → dia_na_lista` acrescenta **2**: omissão no **7**, fora (`app/chamados/services/vigia_fio.py:220`, `:210`).
- `dono_fora_do_fio → dia_do_admin → _carregar → lista → _jornada_que_mente → periodos_do_dia → intervalo_cadastrado`: `date.today()` no salto **6**, dentro (`app/chamados/services/vigia_fio.py:261`, `:202`, `:194`; `app/escala/services/cadastro_realidade.py:791`, `:238`; `app/ponto/juiz_batida.py:270`, `:125`).

## 15/11 18:47 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Fecho consolidado do módulo real `app/chamados/services/vigia_fio.py`. Abaixo, profundidade é número de chamadas a partir da função indicada; acesso à propriedade `estado` conta como chamada (`app/chamados/services/vigia_fio.py:259`, `app/chamados/models.py:3117`).

**Cadeias comuns usadas nas listas**

- **T(C)**: `censo:359 → CargaDaTarefa.tarefa:613 → resolver_dia_oferecido_nas_datas:157 → contexto_calendario`. **T(A)** substitui a primeira chamada por `censo_alcance:593`. Arquivos: `app/chamados/services/vigia_fio.py:359`, `:593`; `app/chamados/services/tarefa_do_dia.py:613`; `app/colaboradores/services/calendario.py:157`.
- **L(C)**: `censo:387 → dono_fora_do_fio:261 → ListaDoAdmin.dia_do_admin:202 → ListaDoAdmin._carregar:194 → cadastro_realidade.lista`. Arquivos: `app/chamados/services/vigia_fio.py:387`, `:261`, `:202`, `:194`.
- **L(A)**: `censo_alcance:621 → ListaDoAdmin.dia_na_lista:210 → ListaDoAdmin._carregar:194 → cadastro_realidade.lista`. Arquivo: `app/chamados/services/vigia_fio.py:621`, `:210`, `:194`.
- **P(C)**: `censo:387 → dono_fora_do_fio:259 → PerguntaDisputa.estado:3127 → pergunta_viva`. **P(A)** substitui a primeira chamada por `censo_alcance:615`. Arquivos: `app/chamados/services/vigia_fio.py:387`, `:615`, `:259`; `app/chamados/models.py:3127`.
- **V**: `pergunta_viva → _pergunta_viva_calc`, pela chamada sem carga em `app/chamados/juizes.py:897`; os caminhos alimentados usam a chamada com carga em `:892`.

**`censo` — todos os leitores físicos alcançados até seis saltos**

| Profundidade | Cadeia e leitor | Instante |
|---:|---|---|
| 0 | `censo → timezone.now()` (`app/chamados/services/vigia_fio.py:282`) | Lê somente quando `agora is None`; a função admite instante externo (`:266`, `:282`). |
| 3 | `censo:357 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila`, leitor `timezone.now()` (`app/chamados/services/painel.py:190`) | `agora` propagado; fallback suprimido (`app/chamados/services/tarefa_do_dia.py:452`). |
| 3 | T(C), leitor `contexto_calendario → timezone.now()` (`app/colaboradores/services/calendario.py:192`) | `agora` propagado (`:159`); fallback suprimido. |
| 3 | `censo:415 → vigia_carimbo.classificar:106 → _pergunta_materializada_de_fato:696 → _dentro_da_janela_viva`, leitor `date.today()` (`app/chamados/juizes.py:595`) | Instante não propagado pelo chamador `_juiz(p)` (`app/chamados/services/vigia_carimbo.py:106`). |
| 3 | Mesmo prefixo por `classificar:106 → _pergunta_materializada_de_fato:701 → classificar_falta`, leitor `_tz.now()` (`app/escala/servico_jornada.py:387`) | `agora=None` chega do `_juiz(p)`; leitura condicionada a existir escala e tipo (`:385`). |
| 4 | `censo:359 → tarefa:600 → função local ler:588 → alimentar_fila:1573 → particionar_verbos`, leitor `now()` (`app/chamados/services/verbos.py:35`) | Instante propagado (`app/chamados/services/tarefa_do_dia.py:590`; `app/chamados/juizes.py:1573`). |
| 4 | T(C) `→ contexto_calendario:638 → status_dos_dias_lavrados`, leitor `now()` (`app/ponto/status_dia.py:253`) | Instante propagado (`app/colaboradores/services/calendario.py:638`); fallback suprimido. |
| 4 | L(C), leitor `lista → now()` (`app/escala/services/cadastro_realidade.py:780`) | Instante propagado (`app/chamados/services/vigia_fio.py:194`); fallback suprimido. |
| 5 | T(C) `→ contexto_calendario:614 → folha_manda:398 → turno_aberto_de`, leitor `now()` (`app/ponto/turnos.py:2291`) | Instante propagado (`app/relatorios/cartao_pela_celula.py:398`); fallback suprimido. |
| 5 | T(C) `→ contexto_calendario:614 → folha_manda:413 → ate_ontem`, leitor `localdate()` (`app/relatorios/cartao_pela_celula.py:40`) | `hoje` derivado do instante e passado (`:413`); fallback suprimido. |
| 5 | L(C) `→ lista:791 → _jornada_que_mente`, leitor **incondicional** `date.today()` (`app/escala/services/cadastro_realidade.py:224`) | A borda já dispõe de instante e dia; a chamada passa somente empresas (`:791`). |
| 5 | L(C) `→ lista:808 → _afastado_sem_registro`, leitor `localdate()` (`app/escala/services/cadastro_realidade.py:588`) | `hoje` passado em `:808`; fallback suprimido. |
| 6 | L(C) `→ lista:808 → _afastado_sem_registro:592 → afastados_hoje`, leitor `localdate()` (`app/colaboradores/services/situacao_exibida.py:45`) | `dia` omitido em `app/escala/services/cadastro_realidade.py:592`; leitura própria. |
| 6 | L(C) `→ lista:864 → _nunca_bateu:179 → colabs_nunca_bateu`, leitor `localdate()` (`app/escala/services/cadastro_realidade.py:161`) | `hoje` passado nas duas chamadas (`:864`, `:179`); fallback suprimido. |

Há ainda caminhos relevantes para leitores já enumerados: P(C) `→ V → _dentro_da_janela_viva:595` ou `classificar_falta:387`, profundidade 5; P(C) `→ V → _pergunta_materializada_de_fato →` esses mesmos leitores, profundidade 6. Nesses caminhos `estado` omite o instante (`app/chamados/models.py:3127`; `app/chamados/juizes.py:1468`, `:1472`, `:1486`, `:696`, `:701`).

O `intervalo_cadastrado:125` fica no salto **7** por L(C), portanto fora: `lista:791 → _jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado` (`app/escala/services/cadastro_realidade.py:791`, `:238`; `app/ponto/juiz_batida.py:270`, `:125`).

**`censo_alcance` — todos os leitores físicos alcançados até seis saltos**

- Não lê relógio diretamente: exige `agora` e deriva o dia por `_dia_de(agora)` (`app/chamados/services/vigia_fio.py:487`, `:516`).
- Herda os cinco leitores da cadeia T(A), nas mesmas profundidades de T(C): `contexto_calendario` 3, `status_dos_dias_lavrados` 4, `turno_aberto_de` 5, `ate_ontem` 5; todos recebem instante ou dia explícitos (`app/chamados/services/vigia_fio.py:593`; `app/chamados/services/tarefa_do_dia.py:613`; `app/colaboradores/services/calendario.py:157`, `:159`, `:614`, `:638`; `app/relatorios/cartao_pela_celula.py:398`, `:413`).
- `censo_alcance:560 → alimentar_portas:1619 → alimentar_fila:1573/1576/1584 → particionar_verbos/qs_na_fila`, profundidade 3; leitores `app/chamados/services/verbos.py:35` e `app/chamados/services/painel.py:190`. Recebem `agora`; fallbacks suprimidos (`app/chamados/juizes.py:1619`, `:1573`, `:1576`, `:1584`).
- A construção da carga também alcança `qs_na_fila` no salto 3: `censo_alcance:592 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila:190`, com instante (`app/chamados/services/vigia_fio.py:592`; `app/chamados/services/tarefa_do_dia.py:730`, `:452`; `app/chamados/services/painel.py:190`).
- L(A) alcança `lista:780` no salto 3, `_jornada_que_mente:224` no 4, `_afastado_sem_registro:588` no 4, `afastados_hoje:45` no 5 e `colabs_nunca_bateu:161` no 5. Os leitores próprios são `_jornada_que_mente` e `afastados_hoje`; os outros fallbacks recebem o argumento (`app/chamados/services/vigia_fio.py:621`, `:210`, `:194`; `app/escala/services/cadastro_realidade.py:791`, `:808`, `:592`, `:864`, `:179`).
- L(A) `→ lista:791 → _jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado`, profundidade **6**, lê `date.today()` quando o tipo de escala existe (`app/ponto/juiz_batida.py:117`, `:125`, `:270`).
- `censo_alcance:537 → dias_que_seguram:165 → _dias_que_retem:121 → motivos_retencao_celula:1212 → _dias_sem_decisao`, profundidade 4, contém `localdate()` em `app/folha/export.py:1238`, suprimido pelo `hoje` propagado (`app/folha/fora_da_folha.py:165`, `:121`).
- **Outra cadeia até o mesmo leitor**: `censo_alcance:537 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho:1307 → motivos_retencao_celula:1212 → _dias_sem_decisao:1238`, profundidade **5**; aqui `hoje` foi omitido em `classificar_export:968`, portanto o relógio próprio pode executar (`app/folha/fora_da_folha.py:147`; `app/folha/export.py:968`, `:1307`, `:1212`, `:1238`).
- `censo_alcance:560 → alimentar_portas:1628 → pergunta_viva → V → _dentro_da_janela_viva/classificar_falta`, profundidade 4; contém `date.today()` e `now()` em `app/chamados/juizes.py:595` e `app/escala/servico_jornada.py:387`, mas `agora` permite obter `hoje` e suprime os fallbacks (`app/chamados/juizes.py:888`, `:890`, `:1628`).
- P(A) alcança esses dois leitores no salto **5**, agora sem instante; passando por `_pergunta_materializada_de_fato`, alcança-os no salto **6** (`app/chamados/services/vigia_fio.py:615`, `:259`; `app/chamados/models.py:3127`; `app/chamados/juizes.py:897`, `:1468`, `:1472`, `:1486`, `:696`, `:701`).
- `censo_alcance:652 → função local no_app:577 → _no_app:1645 → perguntas_do_app:61 → pergunta_viva → V → _dentro_da_janela_viva/classificar_falta`, profundidade **6**; recebe o instante de `alim['agora']`, suprimindo os fallbacks (`app/chamados/services/vigia_fio.py:652`, `:577`; `app/chamados/juizes.py:1645`; `app/chamados/services/pergunta_no_app.py:61`).

**Omissões de `hoje/agora=None` no fecho, incluindo as que recuperam o instante**

- P(C)/P(A): `pergunta_viva(self)` omite **ambos**, profundidade 3 (`app/chamados/models.py:3127`; assinatura `app/chamados/juizes.py:865`).
- `censo:415 → classificar:106 → _juiz(p)` omite ambos de `_pergunta_materializada_de_fato`, profundidade 2; `_juiz` é importado com apelido de `chamados.models`, que reexporta o juiz (`app/chamados/services/vigia_carimbo.py:95`, `:106`; assinatura `app/chamados/juizes.py:665`).
- `censo:415 → classificar:110 → _slot_esperado(p)` omite `agora`, profundidade 2 (`app/chamados/services/vigia_carimbo.py:110`; assinatura `app/chamados/juizes.py:344`).
- `_pergunta_materializada_de_fato:755 → _slot_esperado` omite dinamicamente `agora` quando `agora is None`; a expansão o inclui quando existe (`app/chamados/juizes.py:755`).
- `_pergunta_viva_calc:1486 → _pergunta_materializada_de_fato` faz a mesma expansão condicional de `agora`; alcançável no salto 5 por P(C)/P(A), no salto 4 pela alimentação de alcance e no salto 5 por `tarefa → _ler_vida → pergunta_viva` (`app/chamados/juizes.py:1486`, `:1487`; `app/chamados/services/tarefa_do_dia.py:574`, `:537`, `:541`).
- `censo_alcance:560 → alimentar_portas:1628 → pergunta_viva` omite `hoje`, profundidade 2; recupera-o do instante (`app/chamados/juizes.py:1628`, `:888`, `:890`).
- T(C)/T(A) `→ contexto_calendario:506 → _pv` omite `hoje`, profundidade 4; passa `agora` (`app/colaboradores/services/calendario.py:506`).
- Caminho de `no_app` acima: `perguntas_do_app:61 → pergunta_viva` omite `hoje`, profundidade 4; passa `agora` (`app/chamados/services/pergunta_no_app.py:61`).
- `censo_alcance:537 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho` omite `hoje`, profundidade 3 (`app/folha/export.py:968`, assinatura `:1301`).
- **Classe interna adicional:** `censo:359`/`censo_alcance:593 → tarefa:574 → _ler_vida:534 → carga_da_vida:196 → CargaDaVida.__init__:119 → _slot_esperado`, profundidade **5**; omite `agora`, mas passa `carga=self`, da qual o instante é recuperado (`app/chamados/services/carga_pergunta_viva.py:119`, `:196`; `app/chamados/juizes.py:347`, `:348`).

**Funções diagnósticas públicas**

- `viva_em_disputa_fechada:718 → pergunta_viva → V → _dentro_da_janela_viva/classificar_falta`, profundidade 3; leitores `date.today()`/`now()` em `app/chamados/juizes.py:595` e `app/escala/servico_jornada.py:387`. A chamada omite ambos os argumentos (`app/chamados/services/vigia_fio.py:718`).
- `carimbos_sem_lastro_por_dono:776` tem o mesmo fecho e profundidades, igualmente sem ambos (`app/chamados/services/vigia_fio.py:776`).
- `materializada_viva:813 → carimbos_sem_lastro_por_dono:776` acrescenta um salto: mesmos leitores na profundidade 4 (`app/chamados/services/vigia_fio.py:813`, `:776`).
- Nessas três funções, a perna `V → _pergunta_materializada_de_fato` também alcança os mesmos leitores, um salto depois, e `_slot_esperado` recebe omissão dinâmica de `agora`; profundidades respectivas 4/4/5 para o slot (`app/chamados/juizes.py:1486`, `:1487`, `:755`).
- `disputa_vazia`, `respondida_sem_resposta` e `competencia_do_dia_entregue` não trouxeram leitores ou omissões qualificadas em seus fechos até seis saltos (`app/chamados/services/vigia_fio.py:690`, `:737`, `:785`; chamadas `:697`, `:749`, `:801`, `:804`, `:806`).
- `CargaDaVida.__init__` usa `localtime(ts)` com argumento, sem leitura própria; seus métodos `grade`, `vinculo`, `turnos`, `plantadas`, `batida_na_janela` e `retratada` não acrescentaram leitor físico até seis saltos dos censos (`app/chamados/services/carga_pergunta_viva.py:119`, `:134`, `:152`, `:160`, `:164`, `:185`, `:188`, `:192`).

## 15/11 18:45 · sessao 3ffda51003e4 · - · - · origem=Ronald (tmux)

Adendo confirmado:

- Drawer → `perguntas_vivas_de` → `pergunta_viva` → `_pergunta_viva_calc` → `_pergunta_materializada_de_fato` → `_slot_esperado`: **5 saltos**. `_slot_esperado` aceita `agora=None` em `app/chamados/juizes.py:344`; a chamada em `:755` omite efetivamente `agora` quando ele é `None`, pois o `**` condicional vira `{}`. A borda possui instante em `app/colaboradores/services/drawer.py:21`.
- Nos caminhos através de `contexto_calendario`, o mesmo `**` fornece `agora`, porque a borda o passou e o calendário conserva `_agora` (`app/colaboradores/views.py:1952`; `app/colaboradores/services/drawer.py:124`; `app/colaboradores/services/calendario.py:192`).
- A nova passada AST, considerando `**` condicional, encontrou somente esses candidatos de omissão: `pergunta_viva(hoje)` em `app/colaboradores/services/calendario.py:506`; `pergunta_viva(hoje, agora)` em `app/chamados/juizes.py:1504`; `_pergunta_materializada_de_fato(agora)` em `:1486`; `_slot_esperado(agora)` em `:755`; `motivos_retencao_espelho(hoje)` em `app/ponto/services/fechamento.py:1067`.
- A leitura de `grade_da_celula` e helpers não acrescentou leitor de relógio; o fallback de previsto chama `minutos_previstos_do_dia` em `app/escala/services/leitor_celula.py:411`. Novos leitores por despacho dinâmico fora do fecho lido [não verificado].

## 15/11 18:44 · sessao 3ffda51003e4 · - · - · origem=Ronald (tmux)

Censo das três bordas, por AST com resolução de imports locais/aliases e leitura dos ramos. Caminhos são estáticos; execução em produção permanece [não verificado].

**Bordas já têm instante:** `calendario_ponto` lê `_tz.now()` em `app/colaboradores/views.py:1951`; `contexto_drawer` lê `timezone.now()` em `app/colaboradores/services/drawer.py:21`; `furos_espelho` lê `timezone.now()` em `app/relatorios/views.py:1519`.

**Família compartilhada pelo calendário e drawer**

Prefixos:

- Calendário: `calendario_ponto → contexto_calendario`, chamada `app/colaboradores/views.py:1952`, passa `agora`.
- Drawer: `contexto_drawer → contexto_calendario`, chamada `app/colaboradores/services/drawer.py:123`, passa `agora` na linha 124.

A partir desses prefixos, funções que contêm relógio ou satisfazem o critério de argumento omitido:

1. `contexto_calendario`: `timezone.now()` em `app/colaboradores/services/calendario.py:192`. Fallback **inativo nesses caminhos**, porque ambas as bordas passam `agora` (`views.py:1952`; `drawer.py:124`).
2. `contexto_calendario → status_dos_dias_lavrados`: chamada `app/colaboradores/services/calendario.py:638`; relógio `app/ponto/status_dia.py:253`, guardado por `agora is None` na linha 252. Recebe `_agora`, portanto fallback inativo.
3. `contexto_calendario → pergunta_viva`: alias local `_pv` importado em `app/colaboradores/services/calendario.py:498`, chamada na linha 506. Assinatura `hoje=None, agora=None` em `app/chamados/juizes.py:865`; chamada **omite hoje**, mas passa `agora`. O dia deriva do instante em `app/chamados/juizes.py:888`.
4. `… → pergunta_viva → _pergunta_viva_calc → _dentro_da_janela_viva`: chamadas `app/chamados/juizes.py:897` e `:1468`; leitura `date.today()` em `:595`. Recebe `hoje` derivado do instante; fallback inativo.
5. `… → pergunta_viva → _pergunta_viva_calc → classificar_falta`: import local `_cf` em `app/chamados/juizes.py:1470`, chamada `:1472`, passa `agora` em `:1474`; leitura `_tz.now()` em `app/escala/servico_jornada.py:387`. Fallback inativo porque recebe instante.
6. `contexto_calendario → folha_manda → ate_ontem`: chamadas `app/colaboradores/services/calendario.py:614` e `app/relatorios/cartao_pela_celula.py:413`; leitura `timezone.localdate()` em `app/relatorios/cartao_pela_celula.py:40`. `folha_manda` recebe `_agora` e passa dia derivado dele; fallback inativo.
7. `contexto_calendario → folha_manda → turno_aberto_de`: chamadas `app/colaboradores/services/calendario.py:614` e `app/relatorios/cartao_pela_celula.py:398`; leitura `timezone.now()` em `app/ponto/turnos.py:2291`. Recebe `agora`; fallback inativo. A própria chamada depende de `datas_turno_aberto` não vazio (`cartao_pela_celula.py:396`).
8. `contexto_calendario → folha_manda → totais_da_folha → fechamento_lido → recalcular_fechamento_mes`: chamadas `app/colaboradores/services/calendario.py:614`, `app/relatorios/cartao_pela_celula.py:323`, `:73`, `app/ponto/services/fechamento.py:1759`. **Salto 5** de calendário/drawer. Leituras possíveis `timezone.now()` em `app/ponto/services/fechamento.py:616` e `:1086`; esse caminho não recebe o instante da borda. Exige janela de competência inteira (`cartao_pela_celula.py:321`) e fechamento existente (`:71`). A mesma função contém relógios em `fechamento.py:1143`, `:1161`, `:1189`, mas estes são ramos de escrita **inativos neste caminho**, pois `fechamento_lido` passa `somente_leitura=True` em `:1761` e o ramo de leitura é `:1128`.
9. `… → recalcular_fechamento_mes → motivos_retencao_espelho`: import local `_mre` em `app/ponto/services/fechamento.py:1066`, chamada `:1067`. **Salto 6**; omite `hoje`, cujo default é `None` em `app/folha/export.py:1301`. A função encaminha `hoje=None` à autoridade seguinte em `:1307`; o relógio seguinte ultrapassa o limite de seis saltos.
10. `contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: chamadas `app/colaboradores/services/calendario.py:252`, `app/ponto/turnos.py:3036`, `:1939`, `:1633`, `app/ponto/juiz_batida.py:270`; leitura `date.today()` em `app/ponto/juiz_batida.py:125`. **Salto 6** de calendário/drawer; leitura própria, embora borda já tenha instante. É chamada de instância `_carga.turnos`, que um grafo limitado a nomes importados perde.

**Ramo adicional do drawer**

11. `contexto_drawer → perguntas_vivas_de → pergunta_viva`: import local alias `_vivas_de` em `app/colaboradores/services/drawer.py:79`, chamada `:80`; reexport de `chamados.models` para `chamados.juizes` em `app/chamados/models.py:4225` e `:4226`; `perguntas_vivas_de` chama `pergunta_viva(p)` em `app/chamados/juizes.py:1504`. **Omite hoje e agora**, ambos `None` por default (`:865`), embora drawer já tenha `agora` (`drawer.py:21`).
12. Esse ramo chega a `_dentro_da_janela_viva` no **salto 4**, via `pergunta_viva:897 → _pergunta_viva_calc:1468`, e pode ler `date.today()` em `app/chamados/juizes.py:595`.
13. Mesmo ramo chega a `classificar_falta` no **salto 4**, chamada `app/chamados/juizes.py:1472`, passa `agora=None` em `:1474`; pode ler `_tz.now()` em `app/escala/servico_jornada.py:387`.
14. `… → _pergunta_viva_calc → _pergunta_materializada_de_fato`: chamada `app/chamados/juizes.py:1486`; assinatura com defaults `hoje=None/agora=None` em `:665`. Passa `hoje=hoje`, mas **omite agora efetivamente nesse ramo**: o `**{'agora': agora}` só existe quando não `None` (`:1487`). A função volta a `_dentro_da_janela_viva` em `:696` e `classificar_falta` em `:701`, leitores no **salto 5**, com dia/instante `None`.
15. `contexto_drawer → turnos_do_colab → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: alias local `_tdc` importado `app/colaboradores/services/drawer.py:44`, chamada `:47`; demais chamadas `app/ponto/turnos.py:1915`, `:1939`, `:1633`, `app/ponto/juiz_batida.py:270`; `date.today()` em `:125`. **Salto 5**, leitura própria.

**Furos**

16. `furos_espelho → escalas_em_limbo`: import local em `app/relatorios/views.py:1517`, chamada `:1554`; leitura `timezone.localdate()` em `app/relatorios/furos.py:59`, **salto 1**, leitura própria apesar do `_agora` já disponível (`views.py:1519`).
17. `furos_espelho → dataset_furos → status_dos_dias_lavrados`: chamadas `app/relatorios/views.py:1543`, `app/relatorios/furos.py:123`; leitura guardada em `app/ponto/status_dia.py:253`, **salto 2**. Instante passado nas duas arestas, portanto fallback inativo.
18. `furos_espelho → dataset_furos → contexto_calendario`: chamadas `app/relatorios/views.py:1543`, `app/relatorios/furos.py:160`; passa `agora` em `:164`. Herda os itens 1, 3, 4 e 5 da família compartilhada com um salto extra: relógios guardados de `contexto_calendario:192`, `_dentro_da_janela_viva:595`, `classificar_falta:387`; `pergunta_viva` omite `hoje` em `calendario.py:506`, mas recebe instante.
19. **Somente no grafo sintático** de furos, também aparecem `folha_manda → ate_ontem`, `turno_aberto_de` e `… → recalcular_fechamento_mes` (este no salto 6). O ramo não executa nesta borda: `dataset_furos` fornece `datas_em_aberto=NAO_PERGUNTADO` em `app/relatorios/furos.py:164`, tornando falso `_perguntou` em `app/colaboradores/services/calendario.py:594` e excluindo o bloco que chama `folha_manda` (`:597` até `:614`).
20. O caminho de furos até `intervalo_cadastrado` exige **sete** saltos e está fora do teto: `furos_espelho → dataset_furos → contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`; alternativa pelo D4 chama `_carga.turnos` em `app/ponto/status_dia.py:273`, também totaliza sete.

Não encontrei chamadas de `salario_em`, `custo_mensal`, `vinculos_5x2_6x1_trabalha_feriado` ou `recusa_ancora_futura` no AST de `app/ponto/services/fechamento.py`, `app/ponto/services/espelho.py` ou `app/ponto/motor_calculo_v2.py`; nenhuma dessas arestas diretas deve ser acrescentada [não verificado além dos ASTs lidos].

## 15/11 18:43 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Resultados adicionais para consolidar, todos sob `app/`:

- `lavrar_retrato_folha.handle:65 → montar_linhas:1214 → fora_da_folha:205 → _carga:67 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila`; o leitor é `chamados/services/painel.py:190`, mas recebe `agora`, suprimindo seu fallback. A fábrica `_carga_da_tarefa_cls()` devolve a classe; o construtor é chamado pelo corpo de `carga_da_tarefa`, não pela fábrica (`chamados/services/tarefa_do_dia.py:368`, `:392`, `:624`, `:730`).

- `lavrar.handle:65 → montar_linhas:1214 → fora_da_folha:200 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho`: omite `hoje`, cujo default é `None` (`folha/export.py:1301`). Esta função chama `motivos_retencao_celula` no salto 6 (`folha/export.py:1307`); `_dias_sem_decisao` e seu relógio ficam no salto 7 (`folha/export.py:1212`, `:1238`).

- Outro caminho até `_dias_sem_decisao` está dentro do limite: `lavrar.handle:65 → montar_linhas:1220 → _informativo_rescisao:1288 → informativo_rescisao:1228 → _dias_sem_decisao:1238`. Nele `hoje` é propagado e o fallback não executa (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1220`, `:1288`, `folha/export.py:1228`, `:1238`).

- `contadores → linhas_do_retrato → recortar` omite `hoje=None`; também ocorre pelo vigia e pelas três views. É omissão de argumento, mas esses caminhos passam somente eixo `f` ou nenhum grupo, sem consultar dono (`folha/retrato_folha.py:111`, `:120`, `:121`, `:629`, `:632`, `:635`; `folha/vigia_retido_calado.py:65`; `folha/views_retrato.py:198`).

- `vigiar_retido_calado.handle:33 → censo:60 → colabs_nunca_bateu`: possui fallback `timezone.localdate()` em `escala/services/cadastro_realidade.py:161`, suprimido pelo `hoje` passado (`folha/vigia_retido_calado.py:60`).

- `_recorte_da_barra` lê `timezone.now()` em `folha/views_retrato.py:177`; suas três views chamadoras estão em `:258`, `:386`, `:487`. PDF acrescenta `gerar_pdf_auditavel → timezone.now()` (`folha/views_retrato.py:441`, `relatorios/pdf_base.py:133`). Raio-X acrescenta, condicionado a dono `colab`, `placar_aceite → _tz.now()` e `_tz.localdate()` (`folha/views_retrato.py:511`, `:513`, `chamados/services/verbos.py:286`, `:287`).

- `frescor_dos_retratos` exige `agora`; usa conversão com argumento, competências com data explícita e formatação de carimbos com argumento. Nenhum relógio autônomo apareceu nesse fecho (`folha/retrato_folha.py:1386`, `:1408`, `:1411`, `:1441`, `:1444`; `ponto/janelas.py:60`, `:66`).

Para o módulo real `chamados/services/vigia_fio.py`, atenção aos métodos que um grafo apenas de chamadas nomeadas perde:

- `ListaDoAdmin._carregar → lista` passa ambos os valores (`chamados/services/vigia_fio.py:194`). `lista` tem fallback `now` (`escala/services/cadastro_realidade.py:780`), mas chama `_jornada_que_mente(empresas)` em `:791`, cujo `date.today()` é **incondicional** (`:224`).

- `lista:808 → _afastado_sem_registro:592 → afastados_hoje` omite `dia`; este lê `localdate()` (`colaboradores/services/situacao_exibida.py:37`, `:45`). O fallback próprio de `_afastado_sem_registro` está suprimido pelo `hoje` recebido (`escala/services/cadastro_realidade.py:588`, `:808`).

- `_jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado:125` lê outro `date.today()` quando existe tipo de escala (`ponto/juiz_batida.py:117`, `:125`, `:270`).

- `lista:864 → _nunca_bateu:179 → colabs_nunca_bateu:161` contém fallback de `localdate`, mas propaga `hoje` (`escala/services/cadastro_realidade.py:864`, `:179`, `:161`).

- `censo:359` e `censo_alcance:593` chamam o método `CargaDaTarefa.tarefa`; este chama `_ler_vida` e a função local `ler`, além de `resolver_dia_oferecido_nas_datas(...agora=self.agora)` (`chamados/services/tarefa_do_dia.py:574`, `:578`, `:600`, `:613`). A função local chama `alimentar_fila` com instante (`:588`, `:590`). Esses métodos e a função local precisam integrar o limite de saltos.

## 15/11 18:43 · sessao db3018739a90 · - · - · origem=Ronald (tmux)

Complemento para o módulo real `app/chamados/services/vigia_fio.py`; correspondência com o caminho solicitado `folha/vigia_fio.py` [não verificado].

- `censo_alcance` escolhe empresas visíveis, com interseção opcional de IDs; escolhe competências via import local de `competencias_a_lavrar`, não pelo helper do RetratoFolha. Lê dias vivos de `dias_que_seguram` e deduplica por `(colab, data)`, sem consultar RetratoFolha. `app/chamados/services/vigia_fio.py:508-520`, `:534-544`.
- `competencias_a_lavrar` inclui a atual sempre e a anterior somente se `competencia_entregue` for falso. `app/ponto/services/he_pendente_lavrado.py:218-225`. O juiz considera registro vigente não invalidado da empresa/competência. `app/folha/export.py:580-594`.
- Contracaso derivável: em 25/10/2026, corte 21, com 10/2026 entregue, `competencias_do_retrato` seleciona 10 e 11; alcance seleciona somente 11. Ocorrência em produção [não verificado]. `app/folha/retrato_folha.py:1377-1383`; `app/ponto/services/he_pendente_lavrado.py:220-224`; cálculo das janelas em `app/ponto/janelas.py:24-33`, `:66-72`.
- `censo` antigo escolhe chamados com status vivo e célula, sem janela/competência; `empresas=None` não restringe visibilidade, e o recorte opcional aplica somente empresas fornecidas. Depois seleciona células apontadas com veredito acusando. `app/chamados/services/vigia_fio.py:289-293`, `:331-334`.
- Contracaso derivável: chamado vivo de competência antiga ou empresa oculta pode pertencer ao censo antigo e ficar fora do alcance por janela/visibilidade. Ocorrência em produção [não verificado]. `app/chamados/services/vigia_fio.py:290-292`, `:517-520`, `:534-537`.
- O universo subsidiário de `ListaDoAdmin` cruza sempre empresas visíveis, inclusive quando o censo antigo recebeu `None`; portanto não coincide necessariamente com as empresas dos chamados contados. `app/chamados/services/vigia_fio.py:189-194`, `:290-292`.

## 15/11 18:42 · sessao db3018739a90 · - · - · origem=Ronald (tmux)

Censo por AST/leitura, somente leitura. Os acessos de produção a `RetratoFolha` encontrados são os abaixo; os imports são diretos, incluindo os locais em funções. Não encontrei acesso via alias de módulo/atributo nem via `retratos_folha`; conclusão da varredura AST [não verificado por arquivo:linha].

1. **Universos escolhidos**

- **Fonte de empresas visíveis:** `ativa=True`, exclui CNPJ com prefixo `11222333`, ordena por razão social. Essa ordem também decide a primeira empresa padrão da tela. `app/colaboradores/queries.py:78-87`.
- **Lavrador:** importa localmente `empresas_visiveis`, `RetratoFolha`, `competencias_do_retrato`; empresas = visíveis, com interseção opcional por `--empresa`. Se **ambos** `--mes` e `--ano` forem verdadeiros, usa exclusivamente o par explícito; senão usa o helper por empresa. Um só dos dois argumentos é ignorado para escolha de competência. Grava por `(empresa, mes, ano)`. `app/folha/management/commands/lavrar_retrato_folha.py:44-61`, `:77-79`.
- **Helper compartilhado:** chama `janela_anterior` e `janela_atual`, usa mês/ano da data final e elimina pares repetidos. Import local e funções passadas por variável `_j`: essa chamada indireta consta na AST e na leitura. `app/folha/retrato_folha.py:1369-1383`.
- **Janelas:** `janela_atual(data, empresa)` usa `periodo_apuracao_atual` e corte da empresa; dia maior/igual ao corte desloca competência para mês seguinte. `janela_anterior` aplica `janela_atual` ao dia imediatamente anterior ao início da atual. Corte recebe objeto ou pk, tem padrão 21 e clamp 2–28. `app/ponto/janelas.py:24-33`, `:36-51`, `:60-72`.
- **Frescor:** importa localmente empresas e modelos, transforma o `agora` recebido em dia local, percorre **visíveis × helper por empresa**, consulta retrato por par exato e fechamento pelo mesmo par; ausência vira `nunca lavrado`. Não aceita recorte de empresas/competências explícitas. `app/folha/retrato_folha.py:1401-1418`.
- **Vigia retido:** command passa `None` sem `--empresa`, ou lista de IDs com a flag. `app/folha/management/commands/vigiar_retido_calado.py:30-33`. O censo começa com **todos os retratos**; filtro opcional de IDs não cruza com visíveis. Empresas são construídas **somente a partir dos retratos que existem**; só então consulta helper e separa presentes/ausentes da janela. `app/folha/vigia_retido_calado.py:39-58`. Contadores só percorrem `da_lavra`; `cego` significa nenhuma linha bloqueada ou nenhum retrato escolhido. `:64-89`.
- **Tela/PDF/raiox:** lista visíveis para opções, mas URL ou preferência não `todas` consulta `Empresa.objects.filter(pk=_eid)` sem exigir visibilidade; empresa inválida/ausente cai na primeira visível. Alvo é a empresa encontrada ou todas as visíveis. `app/folha/views_retrato.py:147-164`.
- **Competência da tela:** usa atual da empresa escolhida; em `todas`, usa atual da **primeira visível**, depois aplica um único `(mes, ano)` a **todas**. URL sobrepõe mês/ano independentemente; setas fazem ±1 mês. `app/folha/views_retrato.py:179-193`. `mes_ou` aceita 1–12 e `ano_ou` 2000–2100, com fallback recebido. `app/core/sanitiza.py:30-35`.
- **Aviso de frescor da tela:** chama frescor global, mas retém somente par mostrado e empresas que **já apareceram em `_retratos`**. `app/folha/views_retrato.py:208-212`.
- **Painel lateral do colaborador:** requer mês e ano válidos na URL, empresa cadastrada no colaborador e permissão; consulta par exato e **exige empresa em visíveis**, ao contrário da seleção de pk da tela principal. Imports locais. `app/colaboradores/views.py:975-984`.

**Divergências concretas deduzidas do código; ocorrência em produção [não verificado]:**

- Empresa visível sem **nenhum** retrato: lavrador/frescor a incluem; frescor produz ausências, mas vigia nunca cria sua entrada nem `competencias_sem_retrato`, porque deriva empresas dos retratos existentes. `app/folha/management/commands/lavrar_retrato_folha.py:51-61`; `app/folha/retrato_folha.py:1410-1418`; `app/folha/vigia_retido_calado.py:46-58`. A fixture vazia só verifica `retratos=[]`, `calados=0`, `cego=1`; não exige competências ausentes. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111-113`.
- Empresa inativa/diagnóstico com retrato de janela: `censo(None)` a inclui; lavrador/frescor/tela em `todas` a excluem. `app/colaboradores/queries.py:85-87`; `app/folha/vigia_retido_calado.py:46-56`; `app/folha/views_retrato.py:147-164`.
- A mesma empresa oculta pode ser aberta por `?empresa=<pk>`/preferência na tela principal, mas seu frescor não vem e seu bloco no painel lateral não encontra retrato. `app/folha/views_retrato.py:153-164`, `:210-212`; `app/folha/retrato_folha.py:1410`; `app/colaboradores/views.py:982-983`.
- `25/10/2026`, corte 21: automático/helper = competências 10 e 11; lavra explícita 09 pode existir/ser mostrada via URL, mas frescor e vigia não a avaliam. O caso 09 explícita versus 10/11 automáticas está materializado na fixture. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:35-37`, `:86-93`; escolhas em `app/folha/management/commands/lavrar_retrato_folha.py:57-61`, `app/folha/vigia_retido_calado.py:55-58`, `app/folha/views_retrato.py:181-193`.
- `15/11/2026`, empresa A corte 21, B corte 10: helper A = 10/11; B = 11/12. Se A vier primeiro em `todas`, tela mostra 11 para ambas e não mostra a atual 12 de B. Se B vier primeiro, tela pede 12 também para A, embora automática de A cubra 10/11. É consequência aritmética do código; empresas reais com esses cortes [não verificado]. `app/ponto/janelas.py:24-33`, `:66-72`; `app/folha/retrato_folha.py:1377-1383`; `app/folha/views_retrato.py:179-193`.
- Empresa visível sem retrato no par mostrado: frescor diz `nunca lavrado`; aviso da tela a elimina porque `_no_recorte` só contém empresas com retratos. `app/folha/retrato_folha.py:1412-1418`; `app/folha/views_retrato.py:209-212`.
- Lavrador explícito de uma empresa/par não equivale ao universo global do frescor: o filtro `--empresa` e o par explícito pertencem somente ao lavrador, enquanto frescor sempre percorre todas as visíveis e janelas. `app/folha/management/commands/lavrar_retrato_folha.py:51-61`; `app/folha/retrato_folha.py:1410-1412`.

2. **O que os selos de relógio contam**

- **c14 `test_RED_o_que_fazer_sem_hoje_falha_e_nao_le_relogio_proprio`: CHAMADAS interceptadas**, exige zero com `Mock.assert_not_called()`. O mesmo mock substitui `localdate` e `now`; não é contagem de “uma leitura”. `app/chamados/tests/test_e3_r3_c14_sem_banco.py:270-283`.
- **c14 `test_painel_entrega_o_dia_do_instante_do_ato`: não conta chamadas nem nomes de relógio**; somente verifica que o keyword `hoje` de uma chamada encontrada a `o_que_fazer` tem AST textual `hoje.date()`, apesar da docstring dizer “lido UMA vez”. `app/chamados/tests/test_e3_r3_c14_sem_banco.py:292-299`.
- **c15 `_le_relogio`: NOMES distintos**. É `sorted({n.func.attr ...})`; dois `.now()` produzem `['now']`. Reconhece atributos `now`, `today` e `localdate` sem argumentos posicionais; não reconhece `localtime()` sem argumentos nem alias direto `now()`. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:159-163`.
- **c15 `_instante_da_borda`: quantidade de alvos `ast.Name` de `Assign` cujo RHS é atributo `.now()`**, sem deduplicação. Não conta todas as chamadas; um `.now()` solto adicional não entra, e `a = b = tz.now()` produz dois alvos para uma chamada. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:180-184`.
- **c15 selo recorte:** combina `len(_instante_da_borda)==1` com conjunto de nomes `['now']`, e verifica passagem do instante/dia. Não prova total de chamadas. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:203-211`.
- **c15 selo calendário:** mesma combinação e limitação. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:213-219`.
- **c15 leitores sem relógio:** exige conjunto de nomes vazio; qualquer chamada reconhecida torna o conjunto não vazio, mas não segue chamadas transitivas. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:196-201`, `:159-163`.
- **c15 G1/carimbo único:** usa iterador de instantes e `now=lambda: next(relogio)`; compara **conjunto de `lavrado_em` gravados** com o primeiro instante. Não verifica `call_count`. `app/chamados/tests/test_e3_r3_c15_sem_banco.py:35-44`, `:62-63`, `:97-99`.
- **c16 drawer:** importa o `_le_relogio` de c15 e usa só `['now']`: **NOMES distintos**, portanto dois `.now()` continuam aprováveis por essa asserção. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:26`, `:149-152`.
- **c16 relatório:** importa os dois helpers e combina contagem de alvos de atribuição com nomes distintos, mesma limitação dos selos de view c15. `app/chamados/tests/test_e3_r3_c16_sem_banco.py:26`, `:154-159`.

## 15/11 18:36 · sessao 5f5716b33138 · E3-RESTO-R3 · verificador · origem=Code

**Ocorrência dos casos abaixo em produção: [nao verificado]. São contraprovas construídas a partir do código.**

**A. Cura incompleta.**

As curas chegaram aos sítios de produção: seleção das competências, dia do drawer, instante do relatório e documentação de `lavrado_em`. Entretanto, restam falhas de universo, propagação do relógio e documentação, detalhadas em B, D e G. (`app/folha/vigia_retido_calado.py:46–58`; `app/colaboradores/services/drawer.py:21–22`; `app/relatorios/views.py:1519–1543`; `app/folha/models.py:315–321`)

Os oito mutantes registrados do c16 morreram por asserção na execução em memória; eles cobrem V/U/R/N, mas não os contraexemplos abaixo. E5 foi corrigido no instrumento `censo_none_literal`; a API de `dataset_furos` continua aceitando `agora=None`. (`app/chamados/tests/e3_r3_c16_mutantes.py:25–38`; `app/chamados/tests/e3_r3_c15_mutantes.py:65–83`; `app/chamados/tests/test_e3_r3_c16_sem_banco.py:162–185`; `app/relatorios/furos.py:103`)

**B. Dois achados de universo.**

- **Alta — empresa sem nenhum retrato desaparece.** O vigia descobre empresas pelos próprios retratos. Em **25/10/2026**, empresa visível E2, corte 21, sem retratos: deveria nomear `(E2,10,2026)` e `(E2,11,2026)`, mas devolve nenhuma falta para E2. Lavrador e frescor incluem essa empresa. Havendo outra empresa com bloqueados, `cego=0`, ocultando a cobertura ausente. (`app/folha/vigia_retido_calado.py:46–58`; `app/folha/vigia_retido_calado.py:89`; `app/folha/management/commands/lavrar_retrato_folha.py:51–61`; `app/folha/retrato_folha.py:1410–1418`)

- **Média — inclui empresas fora do universo automático.** `censo(None)` inclui retratos de empresas inativas ou de diagnóstico; lavrador e frescor usam `empresas_visiveis()`. Exemplo: E3 inativa, retrato 11/2026, hoje 25/10/2026: o vigia conta esse retrato e declara falta da competência 10; os outros dois excluem E3. (`app/folha/vigia_retido_calado.py:46–58`; `app/colaboradores/queries.py:85–87`; `app/folha/management/commands/lavrar_retrato_folha.py:51`; `app/folha/retrato_folha.py:1410`)

Nos demais casos pedidos, nenhum erro encontrado: em **21/10/2026**, corte 21 produz competências **10/11**, enquanto corte 26 produz **09/10**; competência futura fora dessas duas é excluída. Retrato duplicado por empresa/competência é vedado pela constraint. (`app/ponto/janelas.py:24–33`; `app/ponto/janelas.py:66–72`; `app/folha/retrato_folha.py:1377–1383`; `app/folha/vigia_retido_calado.py:56`; `app/folha/models.py:365–367`)

**C. Escritores, leitores e significado.**

| Campo/lista | Quem escreve | Quem lê |
|---|---|---|
| `competencias_sem_retrato` | `censo`, `app/folha/vigia_retido_calado.py:53–61` | `linhas`, no mesmo arquivo:94–97; command imprime em `app/folha/management/commands/vigiar_retido_calado.py:34–35` |
| Lista de `competencias_do_retrato` | `app/folha/retrato_folha.py:1377–1383` | Lavrador: `app/folha/management/commands/lavrar_retrato_folha.py:61`; frescor: `app/folha/retrato_folha.py:1411`; vigia: `app/folha/vigia_retido_calado.py:55–58` |
| `RetratoFolha.lavrado_em` | Instante capturado e gravado pelo lavrador: `app/folha/management/commands/lavrar_retrato_folha.py:49–50`, `:77–79` | Frescor/rótulo: `app/folha/retrato_folha.py:1412–1445`; mínimo da tela: `app/folha/views_retrato.py:203`; apresentação: `app/templates/folha/_retrato_barra.html:22` |

As competências significam **mês/ano do fim da janela da empresa**, não mês civil. `lavrado_em` significa **instante do início da rodada**; o vigia deixou de usá-lo para selecionar universo. A divergência encontrada é de empresas abrangidas, conforme B. (`app/ponto/janelas.py:10–21`; `app/folha/retrato_folha.py:1380–1382`; `app/folha/retrato_folha.py:1399`; `app/folha/vigia_retido_calado.py:55–58`)

`competencias_sem_retrato` é publicado, mas não participa do alarme: este consulta apenas `cego`, `calados` e `dias_sem_leitura_sistema`. (`app/folha/management/commands/vigiar_retido_calado.py:16–17`; `app/folha/management/commands/vigiar_retido_calado.py:37–41`)

**D. Dois relógios ainda fora da borda.**

- **Média — relatório.** `furos_espelho` chama `escalas_em_limbo(empresa)`, que lê outro `localdate()`. Caso: borda **15/11 às 23:59:59**; limbo executado em **16/11 às 00:00:01**; células de col7 em 15/16/17 com `trabalha=None,None,True`. Pelo dia da borda, col7 entra no limbo; pelo relógio interno, sai: **1 → 0**. (`app/relatorios/views.py:1554`; `app/relatorios/furos.py:59–68`; `app/escala/models.py:1092–1095`)

- **Média — drawer.** `_vivas_de(disp)` não recebe instante; seu wrapper chama `pergunta_viva(p)` sem `hoje/agora`, permitindo `date.today()` e `_tz.now()`. Caso: pergunta materializada de **20/09/2026**, via `completude_turno`, sem resposta/validação/célula, sem tranca, marco sem fato e falta ainda acusada. Em **20/11**, permanece viva; em **21/11**, sai da janela e é considerada materializada: **1 → 0 perguntas**, embora o calendário ainda receba o instante anterior da borda. (`app/colaboradores/services/drawer.py:79–80`; `app/colaboradores/services/drawer.py:123–124`; `app/chamados/juizes.py:1502–1504`; `app/chamados/juizes.py:595–621`; `app/chamados/juizes.py:696–703`; `app/escala/servico_jornada.py:387`)

**E. `padrao=` dos dois commits: [nao verificado].** [nao verificado]

A cópia não disponibiliza o histórico Git para consultar as mensagens ou o diff do c15b. No delta c16 apresentado, a centralização das competências corresponde a **Single Source of Truth**; a propagação do instante corresponde a **Clock Injection**. Não é possível afirmar se os campos declarados coincidem sem lê-los. (`app/folha/retrato_folha.py:1369–1383`; `app/relatorios/views.py:1519–1543`; `app/docs/GLOSSARIO.md:11`; `app/docs/GLOSSARIO.md:19`; [nao verificado])

**F. Nenhum achado estrutural na migration.**

A 0013 altera somente `help_text`, preservando `DateTimeField(db_index=True)` da definição anterior; não acrescenta coluna, índice nem transformação de dados. Depende expressamente da 0012. Ser esta a última migration de folha no `main`: **[nao verificado]**. (`app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:8–18`; `app/folha/migrations/0011_retratofolha.py:22`)

**G. Dois achados adicionais.**

- **Média — selo de “um relógio” aceita dois.** `_le_relogio` usa um conjunto, eliminando chamadas repetidas pelo nome. Substituir a linha do drawer por `_hoje_da_borda = timezone.localtime(timezone.now()).date()` mantém o teste verde, embora introduza uma segunda leitura. Na virada **20/10 23:59:59 → 21/10 00:00:01**, dia e calendário podem divergir. O mutante registrado só testa a introdução de `localdate()`. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162–163`; `app/chamados/tests/test_e3_r3_c16_sem_banco.py:149–152`; `app/colaboradores/services/drawer.py:21–22`; `app/chamados/tests/e3_r3_c16_mutantes.py:34`)

- **Baixa — G3 permanece parcialmente incorreto no texto curado.** A docstring ainda afirma que o legado cai em `sem_causa`/sem saldo. Contraexemplos: legado `sem_codigo_dominio` produz **tipo `cadastro`, saldo `cadastro`**; legado `furo_espelho`, `dias=2`, produz **saldo `2a5`**. A referência “letra → fio em `LETRA_DO_FIO`” também está invertida: essa direção é `FIO_DA_LETRA`. (`app/folha/models.py:308–316`; `app/folha/retrato_folha.py:180–181`; `app/folha/retrato_folha.py:388–389`; `app/folha/retrato_folha.py:424–432`; `app/folha/retrato_folha.py:496–513`)

Nenhum segundo juiz ou escritor fora da porta identificado no delta apresentado: a função nova delega às janelas, e a gravação continua no lavrador de `RetratoFolha`. Alteração histórica de asserções e valores reais de TXT/FechamentoMensal/dinheiro: **[nao verificado]**. (`app/folha/retrato_folha.py:1377–1383`; `app/folha/management/commands/lavrar_retrato_folha.py:77–79`)

**VEREDITO: REPROVA.**

## 15/11 18:35 · sessao d251dcf115c8 · - · - · origem=Ronald (tmux)

Achados D/G:

1. **Média — relatório ainda consulta outro relógio.** `furos_espelho` fixa `_agora` em `app/relatorios/views.py:1519`, mas chama `escalas_em_limbo(empresa)` em `:1554`; esta lê `timezone.localdate()` em `app/relatorios/furos.py:59`. Caso sintético: borda em **15/11/2026 23:59:59**, execução do limbo em **16/11 00:00:01**, células de col7 com `trabalha=None,None,True` em 15,16,17/11. O vínculo devolve esses valores diretamente (`app/escala/models.py:1092-1095`): o teste de dois dias incluiria col7 usando 15/16 e exclui usando 16/17 (`app/relatorios/furos.py:67-68`). Instrumento em memória confirmou contagem **1 → 0**; ocorrência em produção [nao verificado].

2. **Média — drawer perde o instante ao consultar perguntas vivas.** `contexto_drawer` chama `_vivas_de(disp)` sem instante (`app/colaboradores/services/drawer.py:79-80`); o wrapper chama `pergunta_viva(p)` sem `agora`/`hoje` (`app/chamados/juizes.py:1502-1504`). A cadeia pode ler `date.today()` (`:595`) e `_tz.now()` (`app/escala/servico_jornada.py:387`).

   Caso sintético executado com os corpos reais do C6 e janelas reais: pergunta de **20/09/2026**, materializada, via `completude_turno`, sem resposta/validação/célula, tranca falsa, marco sem fato e autoridade de falta acusando `orfao_14h`. Com relógio em **20/11**, wrapper retorna **1 pergunta viva**; em **21/11**, retorna **0**. A janela deixa de alcançar 20/09 (`app/chamados/juizes.py:595-621`) e o lastro absolve fora dela (`:696-697`). Assim, uma borda de 20/11 pode produzir calendário de 20/11 e banner de 21/11; ocorrência em produção [nao verificado].

3. **Lacuna dos selos — cadeia não coberta.** Os testes R examinam apenas o AST de `contexto_drawer` e `furos_espelho` (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:149-159`), permitindo os dois caminhos acima. Os mutantes R alteram somente esses corpos (`app/chamados/tests/e3_r3_c16_mutantes.py:34-37`).

As suítes c15+c16 sem banco passaram **19/19**, executadas com `PYTHONDONTWRITEBYTECODE=1`; saída não persistida [nao verificado]. Na cadeia principal do calendário, o instante recebido segue para pergunta viva, folha e D4 (`app/colaboradores/services/calendario.py:506`, `:614`, `:638`); nenhum outro relógio nessa cadeia foi comprovado [nao verificado].

## 15/11 18:34 · sessao e42efb08fe33 · - · - · origem=Ronald (tmux)

- **G/A — média: selo de relógio aceita regressão equivalente.** `_le_relogio` elimina duplicatas com `set`; duas chamadas `now()` aparecem como `['now']` (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`). O teste do drawer exige justamente esse resultado (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:149-152`). Mutação em memória de `timezone.localtime(agora).date()` para `timezone.localtime(timezone.now()).date()` em `app/colaboradores/services/drawer.py:22` **sobreviveu** ao teste. Caso: primeiro instante 20/10 23:59:59, segundo 21/10 00:00:01; calendário recebe um instante e os recortes recebem outro dia (`app/colaboradores/services/drawer.py:21-22`, `app/colaboradores/services/drawer.py:132`). O mutante registrado cobre `localdate()`, mas essa leitura duplicada de `now()` escapa (`app/chamados/tests/e3_r3_c16_mutantes.py:34`).

- **F — nenhum problema estrutural encontrado na cópia.** A 0013 apenas altera o `help_text`, preservando `DateTimeField(db_index=True)` (`app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:12-18`); a definição anterior tem o mesmo tipo e índice (`app/folha/migrations/0011_retratofolha.py:22`). Depende expressamente da 0012 (`app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:8-10`), que altera somente o `help_text` de `linhas` (`app/folha/migrations/0012_retrato_linhas_help_text_o256.py:12-18`). Ser a última migration do **main**: **[nao verificado]**.

- **G — outros escritores/juízes no delta:** nenhum encontrado. A única gravação identificada nos módulos alterados continua sendo `RetratoFolha.objects.update_or_create`, na porta do lavrador (`app/folha/management/commands/lavrar_retrato_folha.py:77-79`). A lista nova delega às autoridades existentes de janela (`app/folha/retrato_folha.py:1378-1384`).

- Os 65 testes sem banco c4/c5/c6/c14/c15 passaram nesta execução; comparação histórica para determinar se alguma asserção foi enfraquecida: **[nao verificado]**. Os testes do vigia documentam a mudança de expectativa para janelas e preservam a verificação do instante entregue à lista (`app/folha/tests/test_vigia_retido_calado.py:131-141`, `app/folha/tests/test_vigia_retido_calado.py:154-190`).

## 15/11 18:33 · sessao 4e252e0981b3 · - · - · origem=Ronald (tmux)

Dois achados concretos:

1. **Alta — empresa sem qualquer retrato desaparece do universo.** `censo` descobre empresas pelos próprios retratos; sem retrato, não chega a calcular nem registrar competências faltantes (`app/folha/vigia_retido_calado.py:46-58`). Em 25/10/2026, empresa visível E2 com corte 21 deveria produzir faltas `(E2,10,2026)` e `(E2,11,2026)`, pois lavrador e frescor percorrem `empresas_visiveis()` (`app/folha/management/commands/lavrar_retrato_folha.py:51-61`; `app/folha/retrato_folha.py:1410-1418`). Se E1 tiver bloqueados, `cego=0`, deixando E2 silenciosa (`app/folha/vigia_retido_calado.py:89`). Os testes de universo vazio não verificam `competencias_sem_retrato` (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:111-113`; `app/folha/tests/test_vigia_retido_calado.py:31-33`).

2. **Média — universo inclui empresas que lavrador/frescor excluem.** `censo(None)` lê todos os retratos, inclusive empresas inativas ou de diagnóstico (`app/folha/vigia_retido_calado.py:46-52`). Os outros dois usam empresas visíveis, que excluem inativas e CNPJ `11222333*` (`app/colaboradores/queries.py:78-87`). Exemplo: empresa inativa E3 com retrato 11/2026 entra no contador em 25/10/2026 e ainda gera falta de 10/2026; não entra na rodada automática nem no frescor (`app/folha/vigia_retido_calado.py:55-58`; `app/folha/retrato_folha.py:1410-1411`).

Demais verificações:

- Cortes diferentes e virada do dia 21 usam corretamente o mesmo derivador. Em 21/10/2026: corte 21 → competências 10/11; corte 26 → 09/10 (`app/ponto/janelas.py:24-33`; `app/folha/retrato_folha.py:1377-1383`).
- Competência futura fora dessas duas é excluída (`app/folha/vigia_retido_calado.py:56`). Duplicidade por empresa/competência é vedada por constraint (`app/folha/models.py:365-367`; `app/folha/migrations/0011_retratofolha.py:29`).
- `competencias_sem_retrato`: escritor `censo:53-61`; leitor de produção `linhas:94-97`. O alarme não consulta esse campo (`app/folha/management/commands/vigiar_retido_calado.py:16-17`, `:37-41`).
- `competencias_do_retrato`: escritor `app/folha/retrato_folha.py:1377-1383`; leitores lavrador `:61`, frescor `:1411`, vigia `:55-58`, nos respectivos arquivos.
- `lavrado_em`: escritor único encontrado captura o início da rodada e grava esse instante (`app/folha/management/commands/lavrar_retrato_folha.py:49-50`, `:77-79`). Leitores usam comparação de instantes ou apresentação: frescor (`app/folha/retrato_folha.py:1412-1426`), rótulo (`:1441-1445`), mínimo da tela (`app/folha/views_retrato.py:203`) e apresentação (`app/templates/folha/_retrato_barra.html:22`).
- Executados sem banco e sem escrita de bytecode: 10 testes c16 passaram; 8/8 mutantes morreram por asserção, sem erros. Definições dos testes/mutantes: `app/chamados/tests/test_e3_r3_c16_sem_banco.py:77-185`; `app/chamados/tests/e3_r3_c16_mutantes.py:25-38`. Testes com banco: [nao verificado].

## 15/11 18:18 · sessao 132e7f2bb6ed · E3-RESTO-R3 · verificador · origem=Code

**A. As curas existem, mas G1 permanece incompleto.**

- **G1 — média, procede parcialmente.** O escritor grava o mesmo `_agora` em todos os retratos da execução. Os dois mutantes que restauram `timezone.now()` por retrato morrem por asserção. Porém o vigia continua identificando a rodada pelo **dia civil**, contrariando a cura completa pedida na triagem. Os testes c15 criam apenas uma rodada e não distinguem os dois agrupamentos. (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:83`, `app/folha/vigia_retido_calado.py:48`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:35`, `app/chamados/tests/e3_r3_c15_mutantes.py:28`)

- **G2 — cura real nos sítios alterados.** O instante chega ao frescor, à retenção e ao informativo; existem mutantes que retiram essa propagação e morrem. Sobram caminhos de produção com relógios divergentes, detalhados em C. (`app/folha/views_retrato.py:210`, `app/folha/fora_da_folha.py:121`, `app/folha/fora_da_folha.py:301`, `app/chamados/tests/e3_r3_c15_mutantes.py:30`)

- **G3 — baixa, cura documental incompleta e sem mordida própria.** Os textos principais foram corrigidos no código, inclusive em `models.py`; portanto não ficaram apenas na entrega. Contudo os testes e mutantes c15 cobrem exclusivamente G1/G2. Reintroduzir os textos antigos em memória conserva os nove testes verdes. Permanecem contradições documentais, listadas em E6. (`app/chamados/services/tarefa_do_dia.py:32`, `app/folha/fora_da_folha.py:77`, `app/folha/retrato_folha.py:474`, `app/folha/models.py:306`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:185`, `app/chamados/tests/e3_r3_c15_mutantes.py:27`)

Execução local somente em memória: **9 testes c15 OK; 15/15 mutantes mortos**, por asserção e sem erros de infraestrutura. O instrumento rejeita erros como prova de morte. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:185`, `app/chamados/tests/e3_r3_c15_mutantes.py:80`)

**B. G1 ainda permite universo errado — média.**

Os casos abaixo foram reproduzidos com o `censo` real e os leitores reais do retrato, substituindo apenas dependências externas por dados em memória. A ocorrência desses casos em produção é **[nao verificado]**.

| Caso concreto, horários locais | Resultado |
|---|---|
| **Legado:** comp10 em 14/11, 23:59:59, com um bloqueado `ninguem` e um dia sem leitura; comp11 em 15/11, 00:00:01, com um bloqueado `colab` e nenhum dia sem leitura. | Seleciona somente comp11: **calados=0, dias_sem_leitura_sistema=0, cego=0**. Apaga o positivo conhecido da comp10 na seleção anterior à soma. (`app/folha/vigia_retido_calado.py:47`, `app/folha/vigia_retido_calado.py:65`, `app/folha/vigia_retido_calado.py:80`) |
| **Duas rodadas novas no mesmo dia:** às06h, lavra explícita da comp09 com positivo1; às07h, lavra automática das comp10/11, cada uma com bloqueado `colab` e zero dias sem leitura. | Seleciona **09+10+11**, publicando **calados=1, dias_sem_leitura_sistema=1, cego=0**, embora a rodada completa mais recente tenha zero nesses contadores. A competência explícita é permitida e não é apagada pela execução automática. (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/management/commands/lavrar_retrato_folha.py:62`, `app/folha/management/commands/lavrar_retrato_folha.py:81`, `app/folha/vigia_retido_calado.py:49`) |

Uma rodada nova completa que atravessa meia-noite já conserva suas competências juntas. Isso não resolve o legado nem a mistura de rodadas do mesmo dia. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:85`, `app/folha/vigia_retido_calado.py:48`)

O falso positivo acima vem de **outra competência conservada da rodada anterior**. Não há duas versões físicas da mesma chave empresa/competência: existe unicidade e `update_or_create`. (`app/folha/models.py:363`, `app/folha/management/commands/lavrar_retrato_folha.py:81`)

O teste vizinho usado para justificar a conservação do agrupamento cria comp10 em T0 e comp11 em T0+1min e exige ambas como uma lavra. Essa fixture representa o escritor antigo; não demonstra que dia civil identifica corretamente uma rodada após a mudança. (`app/folha/tests/test_vigia_retido_calado.py:42`, `app/folha/tests/test_vigia_retido_calado.py:47`, `app/folha/tests/test_vigia_retido_calado.py:51`)

Leitores de **`RetratoFolha.lavrado_em`**:

| Leitor | Significado assumido |
|---|---|
| `vigia_retido_calado.censo` | Dia civil local como identidade da última lavra. (`app/folha/vigia_retido_calado.py:47`) |
| `frescor_dos_retratos` | Instante comparável ao maior `FechamentoMensal.atualizado_em`; somente `fechamento > lavrado_em` desatualiza. Assume explicitamente início da rodada, conservador para alterações durante sua execução. (`app/folha/retrato_folha.py:1382`, `app/folha/retrato_folha.py:1403`, `app/folha/retrato_folha.py:1408`) |
| `_rotulo_admin` | Instante apresentado no horário local como “lavrado em”. (`app/folha/retrato_folha.py:1430`) |
| `_recorte_da_barra` e barra | Mínimo dos carimbos dos retratos exibidos, apresentado como “atualizado HH:MM”. (`app/folha/views_retrato.py:203`, `app/templates/folha/_retrato_barra.html:22`) |

O escritor localizado é `lavrar_retrato_folha`; homônimos como o de `DiaPago` não pertencem a esse campo. (`app/folha/management/commands/lavrar_retrato_folha.py:81`, `app/folha/porta_export.py:283`)

**C. G2: argumentos presentes não garantem instante preservado.**

Censo atual de chamadas de produção, incluindo aliases:

| Função | Chamadas | Sítios |
|---|---:|---|
| `frescor_dos_retratos` | 2 | `app/folha/views_retrato.py:210`; `app/folha/retrato_folha.py:1443` |
| `retratos_desatualizados` | 0 | Definição em `app/folha/retrato_folha.py:1437`; órfão também documentado em `app/docs/BACKLOG.md:1772` |
| `o_que_segura` | 0 | Definição em `app/folha/fora_da_folha.py:257`; ferramenta humana HAIKU declarada em `app/docs/BACKLOG.md:1772` |
| `dias_que_seguram` | 2 | `app/folha/fora_da_folha.py:200`; `app/chamados/services/vigia_fio.py:537` |
| `_dias_que_retem` | 2 | `app/folha/fora_da_folha.py:165`; `app/folha/fora_da_folha.py:275` |
| `contexto_calendario` | 4 | `app/colaboradores/views.py:1952`; `app/colaboradores/services/calendario.py:157`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160` |

Nenhuma dessas chamadas omite sintaticamente o parâmetro correspondente. Entretanto:

- **Drawer — média:** captura `agora`, depois relê o dia três vezes. Exemplo com corte21: instante inicial **20/11 23:59:59**, leitura posterior em21/11. A janela do drawer passa de **21/09–20/11** para **21/10–20/12**, mas a grade recebe o instante de20/11; o `hoje` devolvido também vem de outro relógio. (`app/colaboradores/services/drawer.py:19`, `app/colaboradores/services/drawer.py:43`, `app/colaboradores/services/drawer.py:111`, `app/colaboradores/services/drawer.py:122`, `app/colaboradores/services/drawer.py:157`, `app/ponto/janelas.py:24`, `app/colaboradores/services/calendario.py:192`)

- **Relatório de furos — média:** a view lê `hoje`, mas não transmite instante ao dataset. O default `None` chega ao D4 e ao calendário, que leem seus relógios. Uma execução atravessando meia-noite pode combinar o recorte do dia anterior com julgamentos de instantes diferentes por colaborador. (`app/relatorios/views.py:1518`, `app/relatorios/views.py:1541`, `app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`, `app/colaboradores/services/calendario.py:192`)

- **Backfill, medição posterior — média:** após os atos, relê o instante mantendo o `hoje` inicial. Se começar em14/11 e medir depois em15/11, entrega dia14 com instante15 ao `censo_alcance`, que não valida essa divergência. O outro `censo` valida. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`, `app/chamados/services/vigia_fio.py:516`, `app/chamados/services/vigia_fio.py:283`)

Esses caminhos não aparecem no delta fornecido; sua situação executável nos hashes anteriores é **[nao verificado]**.

As três bordas nomeadas estão corretas no caminho normal: calendário do perfil, barra e comando do vigia derivam `agora`/`hoje` do mesmo instante. (`app/colaboradores/views.py:1951`, `app/colaboradores/views.py:1965`, `app/folha/views_retrato.py:177`, `app/folha/views_retrato.py:210`, `app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/chamados/management/commands/vigiar_fio_morto.py:181`)

O resíduo financeiro declarado continua existindo: `classificar_export` e `recalcular_fechamento_mes` omitem `hoje`; o wrapper transmite `None` até `_dias_sem_decisao`, que lê `localdate()`. Esses arquivos estão fora do delta fornecido; comparação independente por Git é **[nao verificado]**. (`app/folha/export.py:968`, `app/ponto/services/fechamento.py:1067`, `app/folha/export.py:1307`, `app/folha/export.py:1238`)

**D. `retratos_desatualizados`: zero chamador de produção no hash atual.**

O delta fornecido altera sua assinatura, sem remover chamador. Há registro documental de zero chamador datado de27/10, portanto não cabe atribuir o abandono à c15. A contagem executável nos hashes **57c1c9b94** e **77dfa94fa** permanece **[nao verificado]**. (`app/folha/retrato_folha.py:1437`, `app/docs/BACKLOG.md:1772`)

**E. Checklist.**

**1. Escritores, leitores e unidades pertinentes ao delta:**

| Campo/valor | Origem/escritor | Leitura e significado |
|---|---|---|
| Empresa, mês e ano do retrato | Lavrador escolhe competência explícita ou janelas do juiz; persiste pela chave única. (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/management/commands/lavrar_retrato_folha.py:62`, `app/folha/management/commands/lavrar_retrato_folha.py:81`) | View filtra pela competência; frescor usa anterior/atual; vigia acrescenta o filtro divergente por dia civil descrito em B. (`app/folha/views_retrato.py:192`, `app/folha/retrato_folha.py:1396`, `app/folha/vigia_retido_calado.py:48`) |
| `linhas.status/motivo` | Classificação do export, copiada pelo montador e persistida pelo lavrador. (`app/folha/export.py:969`, `app/folha/retrato_folha.py:1224`, `app/folha/retrato_folha.py:1240`, `app/folha/management/commands/lavrar_retrato_folha.py:83`) | `_balde` distingue entram/bloqueado/módulo próprio; fio legado usa status/motivo. (`app/folha/retrato_folha.py:89`, `app/folha/retrato_folha.py:449`) |
| `dias/datas` | Datas do retentor; contagem e ISO publicados por `fora_da_folha`, copiados pelo montador. (`app/folha/export.py:1261`, `app/folha/fora_da_folha.py:235`, `app/folha/fora_da_folha.py:246`, `app/folha/retrato_folha.py:1242`) | Recorte inclusivo nas duas pontas; balde Hoje usa `data >= hoje`. Datas são dias da célula/turno, não instantes. (`app/folha/retrato_folha.py:1343`, `app/folha/retrato_folha.py:491`, `app/folha/export.py:1180`) |
| `dono`, `dias_sem_leitura`, `dias_sem_porta` | Projeção da porta e agrupamento dos dias; persistidos no retrato. (`app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:231`, `app/folha/retrato_folha.py:1244`, `app/folha/retrato_folha.py:1248`) | `dono` guarda **fio**; o balde vem do mapa. Vigia conta pessoas por retrato e dias sem leitura separadamente; template lê datas sem porta. (`app/folha/retrato_folha.py:194`, `app/folha/retrato_folha.py:447`, `app/folha/vigia_retido_calado.py:58`, `app/folha/vigia_retido_calado.py:65`, `app/templates/folha/_retrato_o_que_fazer.html:46`) |
| `acao/rota/fio/nao_sei` da tarefa | Publicados pelo juiz e projetados pela porta; ações copiadas à linha. (`app/chamados/services/tarefa_do_dia.py:708`, `app/chamados/services/tarefa_do_dia.py:722`, `app/folha/fora_da_folha.py:223`) | Folha lê fio; UI traduz ação em endereço. `resolver_dia` transporta dia do turno. (`app/folha/fora_da_folha.py:86`, `app/folha/retrato_folha.py:1044`, `app/folha/retrato_folha.py:1054`) |
| `tipo/saldo/informativo` | Montador lê causa gravada, aplica os mapas e copia informativo da autoridade. (`app/folha/retrato_folha.py:1217`, `app/folha/retrato_folha.py:1220`, `app/folha/retrato_folha.py:1250`) | Leitores dos eixos e template; informativo de rescisão não entra em `datas/dias` retentores. (`app/folha/retrato_folha.py:496`, `app/folha/retrato_folha.py:506`, `app/folha/retrato_folha.py:1252`, `app/templates/folha/_retrato_o_que_fazer.html:56`) |
| `FechamentoMensal.atualizado_em/causa_espelho/motivos_espelho` | Carimbo `auto_now`; recalculador atribui causa/motivos e salva. (`app/ponto/models.py:479`, `app/ponto/services/fechamento.py:1067`, `app/ponto/services/fechamento.py:1080`, `app/ponto/services/fechamento.py:1144`) | Frescor compara carimbos por instante; montador lê causa; export consulta a autoridade de retenção. (`app/folha/retrato_folha.py:1408`, `app/folha/retrato_folha.py:1170`, `app/folha/export.py:968`) |
| `CelulaDia.veredito/protestos/status_dia` | Porta da célula grava o julgamento e status. (`app/ponto/portas/celula.py:1092`, `app/ponto/portas/celula.py:1096`) | Retentor lê veredito/protestos; calendário lê status pelo D4. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/colaboradores/services/calendario.py:638`, `app/colaboradores/services/calendario.py:695`) |

O retentor percorre `[ini,fim]` inclusivo; ausência de célula passa a reter apenas em `d < hoje`. As conversões locais usam o fuso configurado `America/Sao_Paulo`. A divergência encontrada não é inclusão de extremidade: é **instante versus identidade de rodada/dia civil**. (`app/folha/export.py:1265`, `app/folha/export.py:1271`, `app/config/settings/base.py:93`, `app/folha/vigia_retido_calado.py:48`)

**2. Segundo juiz ou regra manual de status:** nenhum novo identificado no delta. A tarefa continua publicando a porta; calendário consulta D4; a classificação do export permanece na autoridade registrada. O agrupamento errado do vigia é seleção de universo, já apontada em B. (`app/core/juizes.py:299`, `app/core/juizes.py:323`, `app/chamados/services/tarefa_do_dia.py:722`, `app/colaboradores/services/calendario.py:638`)

**3. Segundo escritor:** nenhum introduzido pelo delta identificado para Chamado, Pauta, CelulaDia, DiaPago ou FechamentoMensal. A escrita alterada continua sendo a de RetratoFolha, pelo lavrador. (`app/chamados/services/tarefa_do_dia.py:735`, `app/chamados/services/vigia_fio.py:490`, `app/folha/management/commands/lavrar_retrato_folha.py:81`)

**4. Relógio fora da borda:** procede nos caminhos de C e no resíduo financeiro; não foi eliminado globalmente. (`app/colaboradores/services/drawer.py:111`, `app/relatorios/furos.py:164`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`, `app/folha/export.py:1238`)

**5. Selos/asserções:** G1 não protege o agrupamento do leitor; G3 não possui mutante c15. O censo G2 aceita a presença sintática de `agora=None`, e a lista contra relógios próprios não inclui drawer/dataset/backfill. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:35`, `app/chamados/tests/e3_r3_c15_mutantes.py:27`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:140`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:168`)

Nos vizinhos de desligamento há lei explícita para a inversão, gêmeo ativo preservado e asserção do informativo. A medida atual conserva os critérios9/85 e A1=6/B1=82. Enfraquecimento relativo dos arquivos de apoio e testes entre os hashes é **[nao verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:1`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:102`, `app/folha/tests/test_lei_desligado_nao_trava.py:104`, `app/core/tests/test_medida_turno_do_par.py:100`)

**6. Documentação incorreta — baixa:**

- `lavrado_em` ainda é descrito como momento em que a linha foi escrita; agora contém o início da rodada, anterior à montagem daquela competência. (`app/folha/models.py:356`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:69`)
- Modelo afirma que linha antiga cai em `ninguem`; `{status:'fora', motivo:'sem_codigo_dominio'}` sem `dono` retorna `admin`. (`app/folha/models.py:315`, `app/folha/retrato_folha.py:449`, `app/folha/retrato_folha.py:374`)
- Schema documentado omite saldo `cadastro` e grupoE, ambos produzidos pelo código. (`app/folha/models.py:307`, `app/folha/models.py:308`, `app/folha/retrato_folha.py:424`, `app/folha/fora_da_folha.py:112`)

O contrato dos cinco retornos de `_fio_do_dia` foi corrigido e corresponde à projeção atual. (`app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:89`)

**7. ESMERIL-SEMANTICA:** procede para `lavrado_em`: início da rodada no escritor/frescor, momento da escrita no modelo e dia identificador de rodada no vigia. `dono` guardar fio está explicitamente documentado e possui mapa próprio para o balde; não demonstra outro conflito novo. (`app/folha/management/commands/lavrar_retrato_folha.py:83`, `app/folha/retrato_folha.py:1382`, `app/folha/models.py:356`, `app/folha/vigia_retido_calado.py:48`, `app/folha/retrato_folha.py:170`, `app/folha/retrato_folha.py:194`)

**8. TXT/FechamentoMensal/dinheiro:** nenhuma mudança de cálculo ou escrita financeira identificada no delta fornecido. A alteração persiste o retrato e muda leituras de retenção/apresentação; o caminho financeiro conserva as chamadas sem `hoje`. Igualdade numérica em produção é **[nao verificado]**. (`app/folha/management/commands/lavrar_retrato_folha.py:83`, `app/folha/fora_da_folha.py:121`, `app/folha/export.py:968`, `app/ponto/services/fechamento.py:1067`)

**VEREDITO: REPROVA.**

## 15/11 18:14 · sessao 4b9631d94182 · - · - · origem=Ronald (tmux)

Achados de G2/C/E4:

- **Média — relógios divergentes no drawer, fora do delta fornecido.** `contexto_drawer` captura `agora` em `app/colaboradores/services/drawer.py:19`, mas lê datas novas em `:43` e `:111`; entrega o instante antigo ao calendário em `:121–122`. Caso concreto, corte 21: `agora=20/11/2026 23:59:59`, leitura de `:111=21/11`. A janela apresentada passa de **21/09–20/11** para **21/10–20/12**, enquanto a grade julga com `hoje=20/11` derivado do instante antigo (`app/ponto/janelas.py:24`, `app/colaboradores/services/calendario.py:192`). Pré-existência no hash anterior: [nao verificado].

- **Média — instante não chega ao calendário do relatório.** A borda lê `hoje` em `app/relatorios/views.py:1518`, mas chama `dataset_furos` sem `agora` em `:1541`; o default `None` (`app/relatorios/furos.py:103`) desce ao D4 em `:123` e ao calendário em `:164`, cujo fallback lê o relógio em `app/colaboradores/services/calendario.py:192`. Uma frota atravessando meia-noite pode receber dias de julgamento diferentes, com recorte escolhido pelo dia antigo (`app/relatorios/views.py:1536–1537`). Fora do delta fornecido; pré-existência: [nao verificado].

- **Baixa — proteção dos testes não cobre esses relógios.** O censo exige presença sintática do argumento (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:140–143`); portanto aceita `agora=None`. A proibição de relógio próprio restringe-se às seis funções de `LEITORES_SEM_RELOGIO`, sem drawer/dataset (`:168–170`).

Censo atual de chamadas de produção, incluindo aliases:

| Função | Chamadas | Locais |
|---|---:|---|
| `frescor_dos_retratos` | 2 | `app/folha/views_retrato.py:210`; `app/folha/retrato_folha.py:1443` |
| `retratos_desatualizados` | 0 | Definição em `app/folha/retrato_folha.py:1437`; nenhuma referência executável externa localizada [nao verificado] |
| `o_que_segura` | 0 | Definição em `app/folha/fora_da_folha.py:257`; uso humano HAIKU declarado em `app/docs/BACKLOG.md:1772` |
| `dias_que_seguram` | 2 | `app/chamados/services/vigia_fio.py:537`; `app/folha/fora_da_folha.py:200` |
| `_dias_que_retem` | 2 | `app/folha/fora_da_folha.py:165`, `:275` |
| `contexto_calendario` | 4 | `app/colaboradores/views.py:1952`; `app/colaboradores/services/calendario.py:157`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160` |

Todos esses chamadores presentes entregam o keyword correspondente; o relatório entrega `None` por default (`app/relatorios/furos.py:103`, `:164`).

As curas diretas são reais:

- Barra: único `_agora`, deriva `_hoje` e transmite o instante ao frescor (`app/folha/views_retrato.py:177`, `:178`, `:210`).
- Calendário de perfil: captura `_agora`, passa à grade e deriva `hoje` dele (`app/colaboradores/views.py:1951`, `:1952`, `:1965`).
- Vigia normal: comando captura `agora/hoje` juntos e transmite aos dois censos (`app/chamados/management/commands/vigiar_fio_morto.py:171`, `:172`, `:180`, `:181`); `censo_alcance` deriva o default e passa `hoje` à retenção (`app/chamados/services/vigia_fio.py:516`, `:537`).
- `_dias_que_retem` transmite `hoje` à autoridade (`app/folha/fora_da_folha.py:121`); `o_que_segura` deriva uma data para retenção e informativo (`:270`, `:275`, `:301`).

Nuance do vigia: `censo_alcance` aceita `hoje` explícito divergente sem validar (`app/chamados/services/vigia_fio.py:516`), ao contrário de `censo` (`:283–284`). Backfill permite `--hoje` histórico em DRY/LISTA (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218–221`, `:237`); após o ato relê `timezone.localtime()` preservando o `hoje` antigo (`:352`). É caminho externo ao delta fornecido.

Resíduo dinheiro:

- Exatamente duas chamadas a `motivos_retencao_espelho`: `app/folha/export.py:968`; `app/ponto/services/fechamento.py:1067`.
- Ambas omitem `hoje`; o wrapper transmite `None` (`app/folha/export.py:1307`), chegando ao relógio interno (`:1238`).
- `motivos_retencao_celula` tem quatro chamadas: export `:1307`, fora_da_folha `:121`, previa `app/folha/previa.py:170`, vigia_pausa `app/folha/services/vigia_pausa.py:125`. As duas últimas já fornecem `hoje`.
- Export/fechamento estão ausentes do delta de produção fornecido; comparação por git indisponível nesta cópia [nao verificado].

D/histórico: `app/docs/BACKLOG.md:1772` já documenta zero chamador de `retratos_desatualizados` em 27/10. Isso é evidência documental anterior; não prova contagem executável nos hashes `57c1c9b94`/`77dfa94fa` [nao verificado]. O delta fornecido não remove chamada dessa função; muda sua assinatura (`app/folha/retrato_folha.py:1437`).

Testes: executei em memória, com `python3 -B`: **9/9 OK e 15/15 mutantes mortos**, sem escrita; resultado transitório sem log arquivado [nao verificado]. O instrumento efetivamente troca fontes em memória (`app/chamados/tests/e3_r3_c15_mutantes.py:73–78`), exige falha de asserção e rejeita erro de infraestrutura (`:80–83`). G2 possui mutantes das bordas, propagação e defaults (`:30–47`); não é cura só textual.

E2: no calendário, status/rotulo vêm do D4 (`app/colaboradores/services/calendario.py:638`, `:695–696`); precedência vem de `fatos_do_periodo` (`:538`). Não localizei clone novo de juiz no delta [nao verificado].

## 15/11 18:13 · sessao e3b4179907c7 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **G3 sem selo que morda a regressão textual — baixa.** O teste c15 contém apenas G1 e G2 (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `:185`); os 15 mutantes também cobrem exclusivamente essas duas curas (`app/chamados/tests/e3_r3_c15_mutantes.py:27`). Reintroduzi, apenas em memória, textos antigos contraditórios em tarefa_do_dia, fora_da_folha, retrato_folha e models: os nove testes c15 continuam verdes. O selo textual c14 cobre outros sítios explicitamente enumerados (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:409`). A cura documental existe nos arquivos vivos, portanto não foi feita só no teste/texto de entrega; falta a mordida pedida para G3 (`app/chamados/services/tarefa_do_dia.py:32`; `app/folha/fora_da_folha.py:77`; `app/folha/retrato_folha.py:474`; `app/folha/models.py:306`).

2. **Contrato documental de RetratoFolha ainda contraditório — baixa.**
   - Afirma que linha antiga sem as três chaves cai em fio `ninguem` (`app/folha/models.py:315`), mas `{status:'fora', motivo:'sem_codigo_dominio'}` sem `dono` cai em `admin`: `fio_da_linha` usa `_fio_cru` (`app/folha/retrato_folha.py:449`), que retorna admin para motivo diferente de furo (`:374`).
   - Enumeração de grupos ainda diz A/B/C/D (`app/folha/models.py:308`), embora `_grupo(['nao_sei'])` retorne E (`app/folha/fora_da_folha.py:112`).
   - Schema de saldo ainda enumera somente `1|2a5|6mais|None` (`app/folha/models.py:307`), embora motivo cadastral produza `cadastro` (`app/folha/retrato_folha.py:424`).

Resultados executados sem gravação:

- c15: nove testes verdes, quinze mutantes mortos por asserção, sem erro de infraestrutura; o runner rejeita morte por erro (`app/chamados/tests/e3_r3_c15_mutantes.py:80`).
- c5+c6+nome_indefinido: 34 testes verdes (`app/chamados/tests/test_e3_r3_c5_sem_banco.py:24`; `app/chamados/tests/test_e3_r3_c6_sem_banco.py:87`; `app/chamados/tests/test_e3_r3_c6_nome_indefinido.py:29`).
- Vizinhos de desligamento têm lei explícita justificando inversão, preservam gêmeo ativo e verificam informativo (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:1`, `:102`, `:116`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`, `:103`).
- Medida mantém números registrados 9/85 e critérios A1=6/B1=82 (`app/core/tests/test_medida_turno_do_par.py:100`); os três sítios novos aparecem na classificação informativa (`:391`, `:415`).
- Enfraquecimento relativo dos testes entre hashes anteriores: **[nao verificado]**, porque esta cópia não disponibiliza histórico Git acessível. [nao verificado]

## 15/11 18:13 · sessao 091be4eebcf9 · - · - · origem=Ronald (tmux)

- **G1, média: cura parcial.** O escritor grava `_agora` em todos os retratos (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:83`), mas o vigia ainda identifica rodada por dia civil local (`app/folha/vigia_retido_calado.py:47-50`). O próprio comentário promete ler a última lavra (`:20-25`).

  Casos concretos reproduzidos executando `censo` real com dependências em memória:
  1. Legado: comp10 carimbada em **14/11 23:59:59**, com um dia sem leitura; comp11 em **15/11 00:00:01**, sem linhas bloqueadas. Retorna apenas comp11, `dias_sem_leitura_sistema=0`, `calados=0`, `cego=1`; o positivo de comp10 desaparece pela seleção em `:48-50`, antes das somas em `:65-80`.
  2. Duas rodadas novas no mesmo dia: às06h, execução explícita de comp09 grava positivo1; às07h, execução completa grava comp10 e comp11, ambas zero. O vigia retorna **09+10+11 e positivo1**, embora a rodada completa mais recente contenha apenas10/11. Execução explícita é permitida em `lavrar_retrato_folha.py:57-58`; universo automático vem de `:62-69`; competências antigas permanecem porque `update_or_create` só substitui sua própria chave (`:81-83`). Não é versão física antiga da mesma competência: a unicidade está em `app/folha/models.py:363-364`.
  3. Rodada completa comp10/11 às06h, comp10 positivo1; relavra parcial de comp11 às07h, zero: continuam ambas e positivo1. Isso confirma que o resultado depende da interpretação “união do dia”, não “última rodada” (`vigia_retido_calado.py:47-50`). A entrega assume deliberadamente essa interpretação, apesar da cura pedida na triagem. [nao verificado: ocorrência desses casos em produção]

- **Mordida G1:** os dois mutantes registrados em `app/chamados/tests/e3_r3_c15_mutantes.py:28-29` foram executados e morreram com falha de asserção, sem erro de infraestrutura; sustentam a cura do escritor nos testes `app/chamados/tests/test_e3_r3_c15_sem_banco.py:85-92`. O leitor não é protegido: substituir em memória `vigia_retido_calado.py:48` o dia pelo instante manteve os três testes G1 verdes. A fixture só produz uma rodada (`test_e3_r3_c15_sem_banco.py:35-78`) e não distingue agrupamento por dia de agrupamento por instante. O teste vizinho exige dois carimbos diferentes, T0 e T0+1min, como uma lavra (`app/folha/tests/test_vigia_retido_calado.py:42-52`); essa fixture representa o escritor antigo e não justifica conservar a ambiguidade do leitor após a mudança.

- **Leitores de `RetratoFolha.lavrado_em`:**
  - Vigia: dia civil local identifica a última lavra (`app/folha/vigia_retido_calado.py:47-50`).
  - Frescor: instante comparado ao maior `FechamentoMensal.atualizado_em`; fechamento estritamente posterior significa desatualizado, igualdade significa atualizado (`app/folha/retrato_folha.py:1401-1411`). Assume início da rodada, explicitamente conservador (`:1382`).
  - Rótulo: converte o instante para horário local e apresenta “retrato lavrado em” (`app/folha/retrato_folha.py:1430-1434`).
  - View: usa o mínimo dos instantes dos retratos exibidos (`app/folha/views_retrato.py:201-203`), repassa como `atualizado` (`:241`, `:319`), e a barra mostra “atualizado HH:MM” (`app/templates/folha/_retrato_barra.html:22`).
  - Escritor único encontrado: `app/folha/management/commands/lavrar_retrato_folha.py:81-83`. Outros campos homônimos encontrados pertencem a DiaPago/Placar/Pauta, não a RetratoFolha (`app/folha/porta_export.py:283-287`, `app/chamados/views.py:843-848`).

- **G3/semântica, baixa:** o help_text ainda define o campo como “Quando o lavrador escreveu esta linha” (`app/folha/models.py:354-357`), mas ele passou a conter o início da rodada, possivelmente anterior à montagem e à gravação daquela competência (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:69`, `:83`). Há três sentidos concorrentes: início da rodada, momento da escrita e dia identificador de rodada; os locais acima demonstram a divergência.

**Recomendação do recorte B/G1: REPROVA**, pela seleção incorreta do universo ainda em `app/folha/vigia_retido_calado.py:47-50`.

## 15/11 18:03 · sessao 6740547b38e3 · JUIZ-RESPONDIDA · censo · origem=Code

Há critérios diferentes para perguntas diferentes: a fila global usa **instante E texto**, a retenção sem veredito usa **instante OU texto**, e a entrada de `validar_pergunta` verifica **só instante**. Não escolhi entre eles. (`app/chamados/juizes.py:1725`; `app/chamados/models.py:1907`; `app/chamados/services/validacao.py:418`.)

**Árvore:** examinei a cópia fornecida. Correspondência com `HEAD` de `main` e com o hash informado: **[nao verificado]**, pois não há metadados Git acessíveis. Execução dos testes: **[nao verificado]**; abaixo descrevo suas asserções estaticamente. [nao verificado]

**Convenções do censo:** `I` = instante presente; `T` = texto que sobra de `(texto or '').strip()`. Onde o código usa verdade crua, igualdade com `''`, comparação de conteúdo ou intervalo temporal, isso está explicitado. (`app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1531`.)

Classes: **A** = `instante E texto`; **B** = `instante OU texto`; **C** = `so instante`; **D** = `so texto`; **E** = `outro`, detalhado na célula; **F** = `exibicao (so mostra)`. A classificação considera o uso dos dois campos naquele sítio; os demais filtros continuam indicados.

**(a) Leitores**

Os números R identificam usos separados por função/finalidade, inclusive quando reunidos numa linha. Q identifica a finalidade normalizada para a contagem final. Os delegados explícitos aparecem além do corpo que consultam.

| R | Arquivo:linha — função | Campos | Classe / predicado aplicado | Pergunta/finalidade | [nao verificado]
|---|---|---|---|---|
| 1 | `app/api/views.py:1163`, `:1165` — `api_chamados_v2` | I,T | **F** — serializa resposta e data | Q01 Payload do app |
| 2 | `app/api/views.py:1179` — `api_chamados_v2` | I,T, delegados | **E** — permite correção quando `not aguarda_o_colaborador(...)`, além dos outros limites | Q02 Correção/reenvio |
| 3 | `app/api/views.py:2561`, `:2563` — `api_disputa_perguntas._item` | I,T | **F** — serialização | Q01 Payload do app |
| 4 | `app/api/views.py:2577` — `api_disputa_perguntas` | I | **C** — `I` seleciona `respondidas` dentro do histórico | Q03 Partição pendente/histórico |
| 5 | `app/api/views.py:2620` — `api_disputa_responder` | I | **C** — `ja_respondida = I` | Q02 Correção/reenvio |
| 6 | `app/api/views.py:2637` — `api_disputa_responder` | I | **F** — devolve instante gravado | Q01 Payload do app |
| 7 | `app/api/views_mensageria.py:567`, `:569`, `:570` — `_item_validar` | I,T | **F** — resposta, data e dias de espera | Q04 Fila de validação |
| 8 | `app/chamados/canal.py:65` — `perguntas_mudas_com_resposta` | T | **D** — SQL `resposta_colab=''`, sem trim | Q05 Resposta encontrada no fio |
| 9 | `app/chamados/juizes.py:351`, `:363` — `_slot_esperado` | I,T | **E** — rejeita resposta posterior ao instante consultado; interpreta `data_hora` ou delega ao plantio | Q06 Slot/materialização |
| 10 | `app/chamados/juizes.py:855`, `:856` — `_chave_memo_pv` | I,T | **E** — valores integram chave de memo; `getattr` literal | Q07 Cache/invalidação — técnica |
| 11 | `app/chamados/juizes.py:1359`, `:1371`, `:1376` — `_resposta_concorda_com_a_luz` | T | **E** — parse da hora e concordância classe A com luz do marco | Q08 Vitalidade |
| 12 | `app/chamados/juizes.py:1388`, `:1393` — `projecao_temporal_da_pergunta` | I,T | **E** — se instante posterior ao teto, projeta localmente `(None, '')` | Q09 Projeção temporal — técnica |
| 13 | `app/chamados/juizes.py:1403`, `:1420` — `_pergunta_viva_calc` | I,T, via projeção | **A** — degrau da resposta: `I and T`; a vitalidade completa segue os demais degraus | Q08 Vitalidade |
| 14 | `app/chamados/juizes.py:1531` — `respondida_sem_texto` | I,T | **E** — `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 15 | `app/chamados/juizes.py:1551` — `q_respondida_sem_texto` | I,T | **E** — face SQL de `I ∧ vazio(strip)`; contempla NULL | Q10 Integridade do par — técnica |
| 16 | `app/chamados/juizes.py:1559` — `q_respondida_com_texto` | I,T | **A** — instante não nulo e regex de texto após trim | Q10 Integridade do par — técnica |
| 17 | `app/chamados/juizes.py:1706` — `porta_alcancavel_do_dia` | I,T, delegados | **E** — exclui `respondida_sem_texto`; exige fila e presença no app | Q11 Alcance/porta |
| 18 | `app/chamados/juizes.py:1718` — `perguntas_corrigiveis` | I | **C** — `I`, mais `pergunta_sem_lastro_ou_viva` | Q02 Correção/reenvio |
| 19 | `app/chamados/juizes.py:1725`, `:1728` — `perguntas_respondidas_a_validar` | I,T | **A** — `q_respondida_com_texto`, `q_vivas` e confirmação Python de vitalidade | Q04 Fila de validação |
| 20 | `app/chamados/juizes.py:1753`, `:1756` — `declarou_hora` | T | **E** — texto não vazio que não seja maiúsculo nem `[descartado...` | Q06 Slot/materialização |
| 21–23 | `app/chamados/juizes.py:1900` — `delta_da_resposta`; `:2004` — `classificar_resposta_validacao`; `:2026` — `motivo_implausivel_validacao` | T | **E**, em cada função — interpreta conteúdo/hora, classe ou plausibilidade | Q12 Classificação da resposta |
| 24 | `app/chamados/juizes.py:2079` — `irmas_respondidas_sem_consumo` | I,T, delegados | **B** — `sem_veredito_entre(..., ignorar_tranca=True)`; universo do crédito | Q13 Retenção por resposta sem veredito |
| 25–26 | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` — `_contraprova_i21a`; `:592` — `Command.handle`, I2 | I | **C**, em ambos — falta/presença do instante, com outros filtros | Q14 Auditoria de invariantes |
| 27 | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` — `Command.handle`, I12 | I,T, delegados | **B** — consulta `irmas_respondidas_sem_consumo` | Q14 Auditoria de invariantes |
| 28 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `:1106` — `Command.handle`, I20 | I,T | **E** — instante nulo e SQL `texto != ''`; sem trim | Q14 Auditoria de invariantes |
| 29 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `:1575` — `Command.handle`, I21 | I,T | **A** — instante presente e SQL `texto != ''`; sem trim; não validada/materializada | Q14 Auditoria de invariantes |
| 30 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1603` — `Command.handle`, C2 | I | **C** — instante nulo | Q14 Auditoria de invariantes |
| 31 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1655` — `Command.handle`, F1 | T | **D** — existência de texto após strip interrompe o ramo | Q14 Auditoria de invariantes |
| 32 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `:60` — `linhas_a_criar` | I,T | **E** — `I OU validada_em`; carrega o par por `.only()` e delega reconstrução | Q15 Preservação/recomposição da trilha |
| 33 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:326` — `aplicar` | T | **E** — copia texto para ato `SILENCIO` | Q15 Preservação/recomposição da trilha |
| 34 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`, `:506` — `Command.handle` | T | **D** — `.only()` fornece texto à eleição de canônica | Q16 Deduplicação/mérito |
| 35 | `app/chamados/management/commands/curar_perguntas_em_limbo.py:32` — `nota_da_resposta` | T | **E** — procura no followup o conteúdo literal da resposta, após strip | Q17 Diagnóstico/cura de limbo |
| 36 | `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `:69` — `Command.handle` | I,T | **E** — instante nulo e SQL `texto != ''`; limpeza mantém filtro de instante nulo | Q17 Diagnóstico/cura de limbo |
| 37 | `app/chamados/management/commands/dedup_perguntas_fato.py:75` — `Command.handle` | T | **D** — texto após strip | Q16 Deduplicação/mérito |
| 38 | `app/chamados/management/commands/validar_classe_a.py:54` — `Command.handle` | I,T, delegados | **B** — `respostas_sem_veredito()` | Q04 Fila de validação |
| 39 | `app/chamados/management/commands/validar_classe_a.py:80` — `Command.handle` | T | **F** — amostra truncada no relatório do comando | Q20 Exibição da resposta |
| 40 | `app/chamados/models.py:1533` — `ChamadoColaborador._universo_da_ressuscitacao` | T | **D** — SQL texto NULL ou `''`, sem trim | Q18 Ressuscitação |
| 41 | `app/chamados/models.py:1610`, `:1611` — `ChamadoColaborador.ressuscitar_perguntas` | I,T | **E** — carrega par por `.only()` para preservar projeções sem linha | Q15 Preservação/recomposição da trilha |
| 42 | `app/chamados/models.py:1907`, `:1908` — `DisputaSupervisao.sem_veredito_entre` | I,T | **B** — `I or T`; sem validação/materialização; tranca conforme parâmetro | Q13 Retenção por resposta sem veredito |
| 43–45 | `app/chamados/models.py:1889` — `respostas_sem_veredito`; `:1917` — `pode_fechar`; `:1950` — `fechar` | I,T, delegados | **B**, nos três — consultam a mesma lista; `pode_fechar` também exige ausência de fio vivo | Q13 Retenção por resposta sem veredito |
| 46 | `app/chamados/models.py:1990` — `DisputaSupervisao.fechar` | T | **E** — copia texto para ato `SILENCIO` | Q15 Preservação/recomposição da trilha |
| 47 | `app/chamados/models.py:2122` — `DisputaSupervisao.remover_motivo` | T | **E** — copia texto para ato `RETRATACAO` | Q15 Preservação/recomposição da trilha |
| 48 | `app/chamados/models.py:2922`, `:2923` — `PerguntaDisputa._nascer` | I,T | **E** — recusa nascimento com `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 49 | `app/chamados/models.py:3128`, `:3131` — `PerguntaDisputa.estado` | I | **C** — pendente/respondida pelo instante, após precedências de validação/vitalidade/materialização | Q19 Estado apresentado |
| 50 | `app/chamados/models.py:3210`, `:3213`, `:3214` — `PerguntaDisputa.carimbar` | I,T | **E** — guarda foto anterior; só completa instante se houver texto e faltar instante | Q10 Integridade do par — técnica |
| 51 | `app/chamados/models.py:3343`, `:3351`, `:3354` — `PerguntaDisputa.descarimbar_em_lote` | I,T | **E** — consulta metade não recebida por `.values_list()` e recusa par resultante ilegal | Q10 Integridade do par — técnica |
| 52 | `app/chamados/models.py:3454`, `:3468` — `PerguntaDisputa.q_vivas` | I,T, delegados | **A** — degrau de resposta usa `q_respondida_com_texto`; queryset completo combina outros degraus | Q08 Vitalidade |
| 53 | `app/chamados/models.py:3534` — `PerguntaDisputa.save` | I,T | **E** — guarda par carregado depois do save | Q10 Integridade do par — técnica |
| 54 | `app/chamados/models.py:3555`, `:3557` — `PerguntaDisputa._par_lido` | I,T | **E** — lê par de `__dict__` somente se ambas as colunas estiverem carregadas | Q10 Integridade do par — técnica |
| 55 | `app/chamados/models.py:3562`, `:3566` — `PerguntaDisputa._recusa_par_ilegal` | I,T | **E** — compara foto e recusa transição para `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 56 | `app/chamados/models.py:3641` — `PerguntaDisputa.respondida` | I | **C** — `respondida_em is not None` | Q19 Estado apresentado |
| 57 | `app/chamados/models.py:3664`, `:3669`, `:3677`, `:3684`, `:3686` — `PerguntaDisputa.resposta_display` | T | **F** — resolve rótulo de posto/escala/opção ou devolve texto | Q20 Exibição da resposta |
| 58 | `app/chamados/models.py:4067`, `:4074`, `:4077` — `DecisaoPergunta.linhas_da_projecao_sem_linha` | I,T | **C** — ato de resposta exige instante; texto é copiado, sem exigir presença; validação tem ramo próprio | Q15 Preservação/recomposição da trilha |
| 59 | `app/chamados/reconciliador.py:365` — `lavrar_sem_turno_aberto` | T | **D** — texto após strip impede lavratura | Q21 Reconciliação de órfãs |
| 60 | `app/chamados/reconciliador.py:448` — `retencao_da_retratacao` | I,T, delegados | **B** — irmãs sem consumo | Q13 Retenção por resposta sem veredito |
| 61 | `app/chamados/reconciliador.py:1083` — `veredito_orfa` | T | **D** — texto após strip impede o ramo | Q21 Reconciliação de órfãs |
| 62 | `app/chamados/reconciliador.py:1164` — `encerrar_orfas_pela_lampada` | I,T, delegados | **B** — lista sem veredito | Q13 Retenção por resposta sem veredito |
| 63 | `app/chamados/relatorios_disputa.py:24`, `:26` — `classificar` | I,T, delegados | **E** — C7 primeiro; depois existência de viva com `¬I ∨ ¬T` distingue colaborador/supervisão | Q22 Cobrança/dono da espera |
| 64 | `app/chamados/services/acoes_disputa.py:95` — `acao_reabrir_disputa` | I | **C** — conta instante nulo | Q02 Correção/reenvio |
| 65 | `app/chamados/services/acoes_disputa.py:130` — `acao_regularizar_disputa` | I | **C** — alguma viva sem instante impede regularizar | Q23 Entrada da regularização |
| 66 | `app/chamados/services/acoes_disputa.py:353`, `:359` — `acao_corrigir_resposta` | T | **E** — preserva texto anterior em nota de correção | Q15 Preservação/recomposição da trilha |
| 67 | `app/chamados/services/becos.py:30` — `classificar` | I | **C** — instante nulo e destino não resolvido | Q21 Reconciliação de órfãs |
| 68 | `app/chamados/services/cobranca.py:237`, `:241` — `_calados` | I,T, delegados | **E** — viva que espera texto: `¬I ∨ ¬T`; disputa aberta | Q22 Cobrança/dono da espera |
| 69 | `app/chamados/services/competencia_trancada.py:126` — `medir` | I,T, delegados | **B** — sem veredito ignorando tranca | Q13 Retenção por resposta sem veredito |
| 70 | `app/chamados/services/declaracao_texto.py:417` — `declarar_hora_colab` | I,T | **E** — instante verdadeiro e texto exatamente igual à hora solicitada | Q06 Slot/materialização |
| 71 | `app/chamados/services/declaracao_texto.py:419` — `declarar_hora_colab` | I | **C** — `bool(instante)` informa reenvio ao escritor | Q02 Correção/reenvio |
| 72 | `app/chamados/services/declaracao_texto.py:687` — `aplicar_declaracao` | I | **C** — ausência de instante dispara gravação de resposta | Q23 Entrada da regularização |
| 73 | `app/chamados/services/disputa_emissao.py:1635`, `:1646` — `reabrir_para_correcao` | I,T | **E** — cria foto se texto OU instante OU validação OU materialização; exclui fora do fato | Q15 Preservação/recomposição da trilha |
| 74 | `app/chamados/services/disputa_emissao.py:1663` — `responder_automaticamente` | T | **D** — texto após strip já existente impede resposta automática | Q24 Resposta automática |
| 75 | `app/chamados/services/disputa_emissao.py:1691` — `carimbar_resposta_pela_trilha` | I,T | **E** — só age com `¬I ∧ T` | Q17 Diagnóstico/cura de limbo |
| 76 | `app/chamados/services/disputa_emissao.py:1734` — `recobrar_questionario` | I | **C** — conta instante nulo | Q22 Cobrança/dono da espera |
| 77 | `app/chamados/services/disputa_emissao.py:1842` — `reabrir_credor_de_pergunta_viva` | I | **C** — instante presente exclui esta porta; também exige viva | Q11 Alcance/porta |
| 78 | `app/chamados/services/disputa_emissao.py:1860` — `reabrir_credor_de_pergunta_viva` | I,T | **E** — carga contém limbo se `¬I ∧ T` | Q17 Diagnóstico/cura de limbo |
| 79 | `app/chamados/services/disputa_emissao.py:2000` — `_resolver_perguntas_por_chamado_resolvido` | I,T | **A** — par com texto não materializado de fato é preservado | Q13 Retenção por resposta sem veredito |
| 80 | `app/chamados/services/disputa_emissao.py:2030` — mesma função | I,T, delegados | **B** — irmãs sem consumo protegem pai | Q13 Retenção por resposta sem veredito |
| 81 | `app/chamados/services/disputa_emissao.py:2078` — mesma função | T | **D** — `_tem_resp = bool(strip(texto))` | Q13 Retenção por resposta sem veredito |
| 82 | `app/chamados/services/disputa_emissao.py:2073`, `:2090` — mesma função | I | **E** — fotos antes/depois para auditoria | Q15 Preservação/recomposição da trilha |
| 83–84 | `app/chamados/services/explicador.py:32` — `_explicar_validada`; `:79`, `:80` — `explicar_pergunta` | T | **F**, em ambos — texto compõe explicação ao usuário | Q20 Exibição da resposta |
| 85 | `app/chamados/services/fila_por_causa.py:226` — `_gaveta_de` | I,T, delegados | **B** — existência de resposta sem veredito seleciona gaveta | Q04 Fila de validação |
| 86 | `app/chamados/services/fio.py:174` — `aguarda_o_colaborador` | I,T, delegados | **E** — existe viva dentro do recorte `¬I ∨ ¬T` | Q22 Cobrança/dono da espera |
| 87 | `app/chamados/services/fio.py:187` — `espera_o_texto_do_colaborador` | I,T | **E** — `¬I or respondida_sem_texto(p)`, equivalente ao recorte `¬I ∨ ¬T` | Q22 Cobrança/dono da espera |
| 88 | `app/chamados/services/fio.py:357`, `:378` — `montar_chamados_fio` | I,T, delegados | **E** — deriva `resposta_enviada` e `todas_resp` pelos dois leitores anteriores | Q22 Cobrança/dono da espera |
| 89 | `app/chamados/services/fio.py:698` — `processar_post_fio` | I | **C** — alguma viva sem instante impede regularização | Q23 Entrada da regularização |
| 90–91 | `app/chamados/services/fio.py:1271` — `canonica_do_grupo`; `:1283` — `merito_perdedor` | T | **D**, em ambos — texto após strip; segundo também exige via admin | Q16 Deduplicação/mérito |
| 92 | `app/chamados/services/fio.py:1602` — `recusa_respondida_sem_texto` | I,T | **E** — recusa `I ∧ ¬T` pelo predicado delegado | Q10 Integridade do par — técnica |
| 93 | `app/chamados/services/foto_e4b.py:179`, `:180` — `nascimentos` | I | **E** — transição antes não nulo/depois nulo em qualquer assinatura listada | Q18 Ressuscitação |
| 94 | `app/chamados/services/historico.py:99`, `:105`, `:106` — `_eventos_dos_chamados` | I | **C** — `getattr(p, campo)` seleciona evento pelo instante e marco `desde` | Q25 Linha do tempo |
| 95 | `app/chamados/services/historico.py:109` — mesma função | T | **F** — texto truncado ou “sem resposta” no evento | Q25 Linha do tempo |
| 96 | `app/chamados/services/janela_colab.py:63` — `chamados_visiveis_colab` | I,T, delegados | **A** — degrau de resposta herdado de `q_vivas`, dentro da consulta abrangente | Q11 Alcance/porta |
| 97 | `app/chamados/services/materializacao.py:280` — `_tipo_pelo_marco` | T | **E** — converte resposta em minutos para identificar marco/tipo | Q06 Slot/materialização |
| 98 | `app/chamados/services/materializacao.py:359`, `:373`, `:412` — `_ts_tipo_do_plantio` | T | **E** — escape/default do relógio, régua e parse da hora | Q06 Slot/materialização |
| 99 | `app/chamados/services/materializacao.py:438` — `estado_aplicacao_pergunta` | T | **D** — exige texto após strip, além de tipo hora | Q06 Slot/materialização |
| 100 | `app/chamados/services/materializacao.py:474` — `_declarar_ausencia_de_pergunta` | T | **E** — conteúdo da resposta determina ausência/opção | Q26 Escape/ausência/folga |
| 101 | `app/chamados/services/materializacao.py:571` — `_materializar_ausencia_de_resposta` | T | **E** — copia texto para ato `ABSORCAO` | Q15 Preservação/recomposição da trilha |
| 102 | `app/chamados/services/materializacao.py:633` — `perguntas_absorvidas_por` | T | **E** — SQL `resposta_colab__in=escapes` | Q26 Escape/ausência/folga |
| 103 | `app/chamados/services/materializacao.py:674`, `:675` — `devolver_absorvidas_da_ausencia_rejeitada` | I,T | **E** — foto do estado anterior à devolução | Q15 Preservação/recomposição da trilha |
| 104 | `app/chamados/services/materializacao.py:768` — `_reconciliar_destino_e_clones._fecha` | I,T, delegados | **B** — irmãs sem consumo impedem fecho | Q13 Retenção por resposta sem veredito |
| 105 | `app/chamados/services/materializacao.py:835` — `_materializar_resposta_hora` | T | **D** — sem texto após strip recusa plantio | Q06 Slot/materialização |
| 106 | `app/chamados/services/materializacao.py:1001`, `:1010` — `_materializar_resposta_data_hora` | T | **E** — parse de data/hora; conteúdo também entra no erro/auditoria | Q06 Slot/materialização |
| 107 | `app/chamados/services/materializacao.py:1376` — `materializar_perguntas_validadas_da_disputa` | I | **C** — instante presente, validada e não materializada | Q06 Slot/materialização |
| 108 | `app/chamados/services/painel.py:67` — `marcador_do_placar` | I | **E** — `Max('respondida_em')` para marcador | Q07 Cache/invalidação — técnica |
| 109 | `app/chamados/services/passivo_validado.py:66`, `:83` — `_reversao_de` | I | **E** — `getattr` dirigido por `CAMPOS_REVERSAO`, serializado em ISO | Q15 Preservação/recomposição da trilha |
| 110 | `app/chamados/services/pergunta_no_app.py:53`, `:61` — `perguntas_do_app` | I,T, delegad

[truncado em 20000 de 54902 caracteres]

_5729 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
