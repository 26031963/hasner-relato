# CODEX-ESTADO

> Ultima resposta processada: **24/11/2026 11:10** · publicado em **24/11/2026 11:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 24/11 11:10 · sessao d5304cd15d9e · CHAMADO-PRESO-NA-REGENERACAO · sonda · origem=Code

```text
sonda_leitura: script.py [6efc9f20be43d662] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

CH 40589 col 549 emp 2 dia 2026-11-22 ini_comp_atual 2026-11-21 oferecido {datetime.date(2026, 11, 22): False}
CH 40588 col 549 emp 2 dia 2026-11-20 ini_comp_atual 2026-11-21 oferecido {datetime.date(2026, 11, 20): False}
CH 37336 col 932 emp 2 dia 2026-11-06 ini_comp_atual 2026-11-21 oferecido {datetime.date(2026, 11, 6): False}
CH 36687 col 899 emp 2 dia 2026-11-03 ini_comp_atual 2026-11-21 oferecido {datetime.date(2026, 11, 3): True}
CH 35243 col 872 emp 2 dia 2026-10-27 ini_comp_atual 2026-11-21 oferecido {datetime.date(2026, 10, 27): False}
CH 34070 col 941 emp 2 dia 2026-10-23 ini_comp_atual 2026-11-21 oferecido {datetime.date(2026, 10, 23): False}
FIM
```

O bloco é a transcrição verbatim do dado raspado fornecido. Não encontrei `script.py` na cópia e não reexecutei a sonda. O `HEAD` não pôde ser confirmado por Git localmente [nao verificado]. As referências abaixo são da cópia fornecida, que o wrapper prepara por hash com `git archive`. `bin/codex_pergunta.sh:304`, `bin/codex_pergunta.sh:315`, `bin/codex_pergunta.sh:360`.

`resolver_dia_oferecido` exige as três condições juntas. O dia deve ser anterior ao início da competência atual, não pode ter pergunta viva com resposta e deve ser anterior a `hoje` com status D4 em `falta`, `irregular`, `aberto` ou `sem_certidao`. A pergunta só bloqueia se tiver `respondida_em`, texto não branco e passar em `pergunta_viva`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:122`, `app/colaboradores/services/calendario.py:503`, `app/colaboradores/services/calendario.py:697`, `app/chamados/juizes.py:1676`.

| CH | Dia | Oferecido | O que o dado permite concluir |
|---|---|---|---|
| 40589 | 2026-11-22 | `False` | **O corte da competência basta para bloquear.** `22/11 >= 21/11`. Status e pergunta viva com resposta [nao verificado]. `app/colaboradores/services/calendario.py:122` |
| 40588 | 2026-11-20 | `False` | `20/11 < 21/11`, passa no corte. Se bloqueou por status, por pergunta viva com resposta ou por ambos [nao verificado]. `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:122` |
| 37336 | 2026-11-06 | `False` | `06/11 < 21/11`, passa no corte. Se bloqueou por status, por pergunta viva com resposta ou por ambos [nao verificado]. `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:122` |
| 36687 | 2026-11-03 | `True` | `03/11 < 21/11`, passa no corte. O `True` implica dia anterior a `hoje`, nenhuma pergunta viva com resposta que bloqueie e status D4 entre os quatro aceitos. Status exato [nao verificado]. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:122` |
| 35243 | 2026-10-27 | `False` | `27/10 < 21/11`, passa no corte. Se bloqueou por status, por pergunta viva com resposta ou por ambos [nao verificado]. `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:122` |
| 34070 | 2026-10-23 | `False` | `23/10 < 21/11`, passa no corte. Se bloqueou por status, por pergunta viva com resposta ou por ambos [nao verificado]. `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:122` |

A janela atual contém o `hoje` usado para calculá-la, então uma data anterior ao início dessa mesma janela também é anterior a `hoje`. Na consulta em lote, se a chamada recebeu `agora`, o corte já devolve `False` para 22/11 antes de montar o calendário. Se a sonda passou `agora` [nao verificado]. `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:150`, `app/colaboradores/services/calendario.py:193`, `app/colaboradores/services/calendario.py:493`.

## 24/11 11:07 · sessao de2a5d3ea4f8 · CHAMADO-PRESO-NA-REGENERACAO · sonda · origem=Code

Não localizei o `script.py` na cópia e não executei a sonda em PROD. Abaixo está a transcrição literal do recorte fornecido. A cópia não tem metadados Git para confirmar o hash do HEAD. As referências apontam para as linhas da cópia recebida. [nao verificado] [nao verificado]

```text
sonda_leitura: script.py [53d2a9b534707c7a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

U657 JSP02 True True
U657_ACOES [(('EscalaColaborador', 'regenerar_celulas_vinculo'), 159), (('CelulaDia', 'veredito_celula'), 50), (('ChamadoColaborador', 'estado_chamado'), 48), (('', 'pos_vinculo'), 13), (('PerguntaDisputa', 'pergunta_emitida'), 4), (('EscalaColaborador', 'editar'), 4), (('ChamadoColaborador', 'nascimento_chamado'), 3), (('Batida', 'retratar_batida_admin'), 3), (('EscalaColaborador', 'criar'), 2), (('CelulaDia', 'editar_dia'), 2), (('Colaborador', 'editar'), 2), (('User', 'toggle_ativo_adm'), 2), (('PerguntaDisputa', 'carimbo_desfeito_por_lastro'), 1), (('ChamadoColaborador', 'fechamento_recusado'), 1)]
  A 08:53:52 ChamadoColaborador estado_chamado 37755 status aberto -> resolvido via sistema
  A 08:53:52 CelulaDia veredito_celula 129577 veredito cobrado -> concorde via cartorio
  A 08:53:52 CelulaDia veredito_celula 129577 veredito cobrado -> trabalhou via admin
  A 08:53:52 CelulaDia veredito_celula 129577 veredito trabalhou do colab #872 em 2026-11-08: o dia ja estava completo, nada a corrigir; pergunta #55294 fechada (admin decidiu); pergunta #55342 fe
  A 08:53:53  pos_vinculo None [nome] col872 2026-11-08..2026-11-08: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 08:53:58 ChamadoColaborador estado_chamado 36950 status aberto -> resolvido via sistema
  A 08:53:59 CelulaDia veredito_celula 129573 veredito cobrado -> concorde via cartorio
  A 08:53:59 CelulaDia veredito_celula 129573 veredito cobrado -> trabalhou via admin
  A 08:53:59 CelulaDia veredito_celula 129573 veredito trabalhou do colab #872 em 2026-11-04: o dia ja estava completo, nada a corrigir; pergunta #53867 fechada (admin decidiu); pergunta #53896 fe
  A 08:53:59  pos_vinculo None [nome] col872 2026-11-04..2026-11-04: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 08:54:08 ChamadoColaborador estado_chamado 36509 status em_analise -> resolvido via sistema
  A 08:54:08 CelulaDia veredito_celula 129571 veredito cobrado -> concorde via cartorio
  A 08:54:08 CelulaDia veredito_celula 129571 veredito cobrado -> trabalhou via admin
  A 08:54:08 CelulaDia veredito_celula 129571 veredito trabalhou do colab #872 em 2026-11-02: o dia ja estava completo, nada a corrigir; pergunta #53079 fechada (admin decidiu); pergunta #53137 fe
  A 08:54:09  pos_vinculo None [nome] col872 2026-11-02..2026-11-02: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 08:59:31 ChamadoColaborador estado_chamado 36125 status aberto -> resolvido via sistema
  A 08:59:31 CelulaDia veredito_celula 129569 veredito cobrado -> concorde via cartorio
  A 08:59:31 CelulaDia veredito_celula 129569 veredito cobrado -> trabalhou via admin
  A 08:59:31 CelulaDia veredito_celula 129569 veredito trabalhou do colab #872 em 2026-10-31: o dia ja estava completo, nada a corrigir; pergunta #52208 fechada (admin decidiu); pergunta #52237 fe
  A 08:59:32  pos_vinculo None [nome] col872 2026-10-31..2026-10-31: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 08:59:39 ChamadoColaborador estado_chamado 35721 status aberto -> resolvido via sistema
  A 08:59:39 CelulaDia veredito_celula 129567 veredito cobrado -> concorde via cartorio
  A 08:59:39 CelulaDia veredito_celula 129567 veredito cobrado -> trabalhou via admin
  A 08:59:39 CelulaDia veredito_celula 129567 veredito trabalhou do colab #872 em 2026-10-29: o dia ja estava completo, nada a corrigir; pergunta #51589 fechada (admin decidiu); pergunta #51616 fe
  A 08:59:40  pos_vinculo None [nome] col872 2026-10-29..2026-10-29: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 08:59:48 ChamadoColaborador estado_chamado 34898 status aberto -> resolvido via sistema
  A 08:59:49 ChamadoColaborador estado_chamado 34719 status em_analise -> resolvido via sistema
  A 08:59:49 CelulaDia veredito_celula 129563 veredito cobrado -> concorde via cartorio
  A 08:59:49 CelulaDia veredito_celula 129563 veredito cobrado -> trabalhou via admin
  A 08:59:49 CelulaDia veredito_celula 129563 veredito trabalhou do colab #872 em 2026-10-25: o dia ja estava completo, nada a corrigir; pergunta #50083 fechada (admin decidiu); pergunta #50084 fe
  A 08:59:49  pos_vinculo None [nome] col872 2026-10-25..2026-10-25: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 08:59:55 ChamadoColaborador estado_chamado 34239 status em_analise -> resolvido via sistema
  A 08:59:55 CelulaDia veredito_celula 129561 veredito cobrado -> concorde via cartorio
  A 08:59:55 CelulaDia veredito_celula 129561 veredito cobrado -> trabalhou via admin
  A 08:59:55 CelulaDia veredito_celula 129561 veredito trabalhou do colab #872 em 2026-10-23: o dia ja estava completo, nada a corrigir; pergunta #49222 fechada (admin decidiu); pergunta #49223 fe
  A 08:59:55  pos_vinculo None [nome] col872 2026-10-23..2026-10-23: {'julgadas': 0, 'puladas_humanas': 1, 'reconciliados': 0, 'furos_antes': 0, 'furos_
  A 09:00:01 ChamadoColaborador estado_chamado 33274 status em_analise -> resolvido via sistema
  A 09:00:01 CelulaDia veredito_celula 129559 veredito cobrado -> concorde via cartorio
  A 09:00:01 CelulaDia veredito_celula 129559 veredito cobrado -> trabalhou via admin
  A 09:00:01 CelulaDia veredito_celula 129559 veredito trabalhou do colab #872 em 2026-10-21: o dia ja estava completo, nada a corrigir; pergunta #46499 fechada (admin decidiu); pergunta #46500 fe
CH 40589 aberto perguntas 2 fio_vivo True
  P 61522 intervalo_saida 2026-11-22 marco int_ida tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da saída do intervalo no dia 22/11/2026'
  P 61523 intervalo_volta 2026-11-22 marco int_volta tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da volta do intervalo no dia 22/11/2026'
CH 40588 aberto perguntas 2 fio_vivo True
  P 61520 intervalo_saida 2026-11-20 marco int_ida tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da saída do intervalo no dia 20/11/2026'
  P 61521 intervalo_volta 2026-11-20 marco int_volta tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da volta do intervalo no dia 20/11/2026'
CH 37336 aberto perguntas 1 fio_vivo True
  P 54538 saida_sem_entrada 2026-11-06 marco inicio tipo hora resp '07:00' respondida True validada False retratada False perg 'Confirme o horário da entrada no dia 06/11/2026'
CH 36687 aberto perguntas 4 fio_vivo True
  P 53408 saida_sem_entrada 2026-11-03 marco inicio tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da entrada no dia 03/11/2026'
  P 53534 intervalo_volta 2026-11-03 marco int_volta tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da volta do intervalo no dia 03/11/2026'
  P 53580 orfao_14h 2026-11-03 marco fim tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da saída no dia 03/11/2026'
  P 53584 intervalo_saida 2026-11-03 marco int_ida tipo hora resp '' respondida False validada False retratada False perg 'Confirme o horário da saída do intervalo no dia 03/11/2026'
CH 35243 aberto perguntas 2 fio_vivo True
  P 50720 intervalo_saida 2026-10-27 marco int_ida tipo hora resp 'FOLGA_CONTESTA' respondida True validada False retratada False perg 'Confirme o horário da saída do intervalo no dia 27/10/2026'
  P 50749 intervalo_volta 2026-10-27 marco int_volta tipo hora resp 'FOLGA_CONTESTA' respondida True validada False retratada False perg 'Confirme o horário da volta do intervalo no dia 27/10/2026'
CH 34070 em_analise perguntas 1 fio_vivo True
  P 49602 intervalo_volta 2026-10-23 marco int_volta tipo hora resp '13:14' respondida True validada False retratada False perg 'Confirme o horário da volta do intervalo no dia 23/10/2026'
FIM
```

### 1. Porta que explica as ações de 657

- **“Resolver dia” → “Trabalhou no horário da escala”** é a ação que corresponde às linhas A. O formulário faz `POST /ponto/celula/veredito/`, atendido por `ponto.views.veredito_celula_view`. A view passa `request.user` e a própria `request` para o serviço. O serviço gera o texto “o dia ja estava completo, nada a corrigir”, fecha o fio do dia e lavra `trabalhou` via `admin`. Depois de um sucesso, a view chama `reconciliar_apos_vinculo` com `motivo='resolver_dia'`, que grava a linha `pos_vinculo`. `app/templates/ponto/partials/_veredito_dia.html:6`, `app/templates/ponto/partials/_veredito_dia.html:17`, `app/templates/ponto/partials/_veredito_dia.html:21`, `app/config/urls.py:29`, `app/ponto/urls.py:58`, `app/ponto/views.py:3519`, `app/ponto/views.py:3530`, `app/ponto/services/veredito_celula.py:252`, `app/ponto/services/veredito_celula.py:352`, `app/ponto/services/veredito_celula.py:361`, `app/colaboradores/services/pos_vinculo.py:92`.
- **`via sistema` e usuário 657 cabem na mesma linha.** `fechar_o_fio_do_dia` chama `c.reconciliar`, que grava a transição com a via `sistema`. `_gravar_estado` emite `estado_chamado` sem autor explícito nesse caminho. O middleware guarda a request, e `core.observ.evento` usa o usuário autenticado dela para preencher `LogAuditoria.usuario`. O `save` do chamado também pode disparar o julgamento do Cartório antes da lavra administrativa. O ramo exato que fechou cada chamado da lista A fica [nao verificado]. `app/ponto/services/veredito_celula.py:207`, `app/chamados/models.py:674`, `app/chamados/catalogo/motor.py:88`, `app/chamados/catalogo/motor.py:95`, `app/chamados/models.py:304`, `app/core/middleware.py:78`, `app/core/observ.py:108`, `app/core/observ.py:134`, `app/chamados/signals.py:189`, `app/chamados/signals.py:218`.
- O controle aparece no **calendário de ponto do perfil** e no **painel de diagnóstico**, com formulário individual e em lote. A página de origem e o formulário usados por 657 ficam [nao verificado]. `app/templates/colaboradores/detalhe.html:215`, `app/templates/colaboradores/calendario_partial.html:67`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`, `app/templates/colaboradores/partials/_calendario_grade.html:135`, `app/templates/ponto/partials/_veredito_lote_barra.html:2`.

### 2. Marcos das perguntas e presença na ata atual

O código mapeia o motivo da pergunta para a posição e o tipo do marco. Para conferir um horário cobrado, o lastro procura o par **hora + tipo** em `ata.lampadas`. O recorte traz os pares cobrados, mas a presença deles na ata atual fica [nao verificado]. `app/chamados/services/declaracao_texto.py:43`, `app/chamados/juizes.py:971`, `app/ponto/services/lastro.py:75`, `app/ponto/services/lastro.py:82`.

| Chamado e dia | Marco pedido nas linhas P | Hora e tipo cobrados no dado | Existe na ata atual? |
|---|---|---|---|
| 40589 · 22/11 | 61522 saída para intervalo (`int_ida`, S). 61523 volta do intervalo (`int_volta`, E). `app/chamados/services/declaracao_texto.py:46` | `11:00S`, `12:00E` | [nao verificado] |
| 40588 · 20/11 | 61520 saída para intervalo (`int_ida`, S). 61521 volta do intervalo (`int_volta`, E). `app/chamados/services/declaracao_texto.py:46` | `11:00S`, `12:00E` | [nao verificado] |
| 37336 · 06/11 | 54538 entrada do turno (`inicio`, E). A resposta registrada é `07:00`. `app/chamados/services/declaracao_texto.py:44` | `10:00E` | [nao verificado] |
| 36687 · 03/11 | 53408 entrada (`inicio`, E). 53534 volta do intervalo (`int_volta`, E). 53580 saída final (`fim`, S). 53584 saída para intervalo (`int_ida`, S). `app/chamados/services/declaracao_texto.py:44` | `08:00E`, `14:00E`. A associação de cada E à sua pergunta fica [nao verificado]. A lista fornecida não traz horário S. | [nao verificado] |
| 35243 · 27/10 | 50720 saída para intervalo (`int_ida`, S). 50749 volta do intervalo (`int_volta`, E). Ambas registram `FOLGA_CONTESTA`. `app/chamados/services/declaracao_texto.py:46` | `12:00S`, `13:00E` | [nao verificado] |
| 34070 · 23/10 | 49602 volta do intervalo (`int_volta`, E). A resposta registrada é `13:14`. `app/chamados/services/declaracao_texto.py:47` | `12:00E` | [nao verificado] |

`fio_vivo=True` indica que pelo menos uma pergunta do conjunto passado ao juiz está viva. Nos chamados com várias linhas P, a vitalidade individual de cada pergunta fica [nao verificado]. `app/chamados/juizes.py:1604`.

## 24/11 11:05 · sessao 8416cdb68151 · PONTA-PEQUENA-NA-BORDA · leitura · origem=Code

### 1. O que grava e efeito no dinheiro

Com `--apply`, o comando chama `ponto.portas.he.recusar_ponta_pequena`, que usa o escritor `_gravar`. Ele cria uma `DecisaoHE` por `(colaborador, data)` com `estado='nao'`, `origem='sistema'`, `decidida_por=None`, `minutos_na_decisao` e um `motivo` com os minutos e o limite. O campo que identifica a via é `origem`. Se já há decisão vigente, a porta pula o dia. Se há uma linha revogada e o dia voltou a caber no limite, atualiza a mesma linha e limpa as três marcas de revogação. `app/ponto/management/commands/recusar_ponta_pequena.py:168–183`, `app/ponto/portas/he.py:65–86`, `app/ponto/portas/he.py:346–372`, `app/ponto/models.py:1297–1317`, `app/ponto/models.py:1354–1357`.

A trilha é `LogAuditoria`, com `acao='recusar_ponta_pequena'`, `modelo='DecisaoHE'`, o PK da decisão e `valor_antes`/`valor_depois`. Em execução pelo `manage.py`, o ator da trilha é `sistema:recusar_ponta_pequena`. A trilha é *best effort*. `app/ponto/portas/he.py:89–106`, `app/ponto/portas/he.py:369–371`, `app/core/auditoria.py:17–29`, `app/core/observ.py:22–32`, `app/core/observ.py:108–137`.

**Pelo código, a recusa não altera valores.** Sem decisão e com `estado='nao'`, a apuração segue o mesmo ramo. Só `autorizado` libera os minutos. A porta do sistema não chama recálculo nem escreve `FechamentoMensal`, `DiaPago` ou holerite. O comando só lê o hash de `FechamentoMensal` antes e depois. Se divergir, para **depois** das escritas já feitas, sem revertê-las. Resultado em produção [não verificado]. `app/ponto/motor_calculo_v2.py:1455–1457`, `app/ponto/motor_calculo_v2.py:1647–1653`, `app/ponto/motor_calculo_v2.py:1691–1706`, `app/ponto/motor_calculo_v2.py:1733–1744`, `app/ponto/portas/he.py:360–372`, `app/ponto/management/commands/recusar_ponta_pequena.py:166–202`, `app/ponto/management/commands/lavrar_dias_pagos.py:54–78`.

### 2. Universo, competência e guardas

- Por padrão, percorre as empresas ativas e escolhe **uma competência por empresa**, a que contém `timezone.localdate()`, pelo corte da empresa. Aceita `--mes` e `--ano` juntos e `--empresas` com IDs separados por vírgula. Se chega apenas mês ou apenas ano, usa a competência de hoje. Sem `--apply`, faz ensaio pela mesma porta. `app/ponto/management/commands/recusar_ponta_pequena.py:55–59`, `app/ponto/management/commands/recusar_ponta_pequena.py:71–96`, `app/ponto/management/commands/recusar_ponta_pequena.py:168–170`, `app/ponto/janelas.py:36–63`.
- Lê os dias do retrato `he_pendente` da competência escolhida. Exige retrato calculado **hoje**, colaborador existente, ausência de decisão vigente, `CelulaDia.veredito` preenchido e lista vazia de marcos ainda não vencidos em `cartorio.marcos_vencidos(...)[1]`. Esse teste usa o instante do marco **mais 30 min**, inclusive em turno que cruza a meia-noite. `app/ponto/management/commands/recusar_ponta_pequena.py:98–156`, `app/ponto/services/he_pendente_lavrado.py:193–206`, `app/ponto/services/cartorio.py:880–911`.
- **Exportada ou trancada.** O comando e a porta de recusa não consultam `competencia_entregue` nem `periodo_trancado`. A guarda de exportação que existe está na seleção padrão de **`lavrar_he_pendente`**, que inclui a anterior só se `competencia_entregue` for falso. A corrente entra sempre nessa seleção, e `--mes`/`--ano` explícitos no lavrador pulam essa seleção. Assim, um retrato de hoje pode satisfazer a guarda do recusador mesmo para competência exportada ou trancada. O estado da competência 11/2026 em produção é [não verificado]. `app/ponto/management/commands/recusar_ponta_pequena.py:78–170`, `app/ponto/portas/he.py:330–372`, `app/ponto/management/commands/lavrar_he_pendente.py:34–43`, `app/ponto/services/he_pendente_lavrado.py:209–225`, `app/folha/export.py:580–594`, `app/ponto/services/fechamento.py:1464–1470`.
- **15 min é o default** de `Empresa.limite_decisao_he_min`. Vale o valor cadastrado por empresa, sobre o total do dia somado no retrato. A porta aceita `0 < minutos <= limite`, portanto 15 exatos cabem quando o cadastro é 15. Limite zero desliga a recusa automática. Valores cadastrados em produção [não verificado]. `app/colaboradores/models.py:82–89`, `app/ponto/services/he_pendente_lavrado.py:55–68`, `app/ponto/management/commands/recusar_ponta_pequena.py:92–96`, `app/ponto/portas/he.py:277–293`, `app/ponto/portas/he.py:332–344`.

### 3. Reversão

O comando **gera arquivo de reversão, mas não tem flag para consumi-lo**. Comando irmão que consuma esse JSON [não verificado]. Com `--apply` e candidatos, grava **antes das escritas** um JSON UTF-8 em `/app/logs/ponta_pequena/emp<id>_<MM><AAAA>_<AAAAMMDD_HHMMSS>.json`. Campos: `empresa`, `mes`, `ano`, `limite_decisao_he_min`, `escrito_em`, `como_reverter` e `pares`, cada um com `colab`, `data` ISO e `minutos`. Falha ao gravar o arquivo aborta antes de chamar a porta para aquela empresa. `app/ponto/management/commands/recusar_ponta_pequena.py:49–59`, `app/ponto/management/commands/recusar_ponta_pequena.py:156–170`, `app/ponto/management/commands/recusar_ponta_pequena.py:208–229`.

**O JSON lista candidatos, não as escritas confirmadas.** A instrução de apagar por `origem='sistema'` usa filtros independentes de colaborador e data, que podem cruzar pares, e não distingue inserção de reativação. Para desfazer com precisão, o snapshot teria de guardar, por par exato, a ausência ou a linha completa **antes** e a linha **depois** da escrita confirmada, com PK, estado, origem, autor, motivo, minutos, carimbos e os três campos de revogação. Assim é possível remover só a inserção da rodada ou restaurar a linha revogada anterior, conferindo o estado posterior para preservar decisões e alterações subsequentes. A trilha atual não guarda o motivo nem os minutos **da revogação anterior**. `app/ponto/management/commands/recusar_ponta_pequena.py:163–183`, `app/ponto/management/commands/recusar_ponta_pequena.py:215–223`, `app/ponto/portas/he.py:61–99`, `app/ponto/models.py:1300–1346`.

Há uma revogação de negócio em `lavrar_he_pendente`: ele chama `revogar_recusa_do_sistema` quando os minutos passam do limite. A porta marca a linha e a retira das vigentes. Esse fluxo não consome o JSON. `app/ponto/management/commands/lavrar_he_pendente.py:48–62`, `app/ponto/portas/he.py:473–505`, `app/ponto/models.py:1265–1267`.

### 4. Quem chama e quem lê

- **Agenda.** `recusar_ponta_pequena --apply` é encadeado ao término de `lavrar_he_pendente`, com log `ponta_pequena.log` e até 900 s de espera pelo gatilho. Não tem horário próprio. A raiz diária `processar_cartorio --apply` está configurada para **06:27, 06:29 e 06:31** nas empresas 2, 3 e 4. Horário efetivo da recusa em produção [não verificado]. `app/config/crons.py:17–33`, `app/config/crons.py:614–616`, `app/config/crons.py:1531–1578`, `app/config/crons.py:1580–1591`, `app/config/crons.py:1903–1924`, `bin/encadeado.sh:84–102`.
- **Tela e fila.** Gestão de HE, seu PDF, espelho e contador da Central leem a decisão vigente. O dia segue visível como `HE recusada` e sai da contagem de `sem_decisao`. O lavrador também lê as vigentes ao montar o retrato. `app/ponto/services/gestao_he.py:42`, `app/ponto/services/gestao_he.py:69–95`, `app/ponto/services/gestao_he.py:295–310`, `app/ponto/services/gestao_he.py:488–505`, `app/ponto/views.py:823`, `app/ponto/views.py:969`, `app/ponto/services/espelho.py:939–948`, `app/ponto/management/commands/lavrar_he_pendente.py:43–62`.
- **Folha e cartório.** A folha lê a decisão no motor e no portão do TXT. O portão de HE, quando ligado, só barra dia **acima** do limite e **sem decisão vigente**, portanto a ponta pequena já fica fora dessa trava antes da recusa. No caminho do cartório, a dependência observada vai da recusa para o cartório: o comando lê o veredito da célula e pergunta pelos marcos. Leitura de `DecisaoHE` pelo julgamento do cartório [não verificado]. `app/ponto/motor_calculo_v2.py:1397–1418`, `app/folha/porta_export.py:223–236`, `app/folha/porta_export.py:676–691`, `app/ponto/management/commands/recusar_ponta_pequena.py:120–156`, `app/ponto/services/cartorio.py:1898–1923`.

### 5. Onde a mesma decisão poderia entrar por evento

**O evento já existe.** O cartório chama `lavrar_veredito` e persiste pelo `carimbar`. Quando a lavra escreve `veredito` ou `ata`, `salvar_e_emitir` grava um `EventoCelula` na mesma transação. `recompute_turnos` também emite esse evento quando um turno materializado deixa de estar aberto. O consumidor atual roda a cada minuto e trata chamados e perguntas. Ele ainda não chama a recusa de HE. `app/ponto/services/cartorio.py:1898–1923`, `app/ponto/portas/celula.py:1217–1238`, `app/ponto/portas/celula.py:977–1001`, `app/ponto/nucleo.py:57–66`, `app/ponto/nucleo.py:86–93`, `app/chamados/reconciliador.py:1492–1523`, `app/config/crons.py:393–394`.

O ponto de encaixe é o **consumo desse evento**, reaproveitando `ponto/portas/he.py::recusar_ponta_pequena` para decidir e gravar, e `acima_do_limite` como predicado único do limiar. O juiz geral de **fato encerrado** é `ponto/turnos.py::dia_encerrado`, que o cartório consulta em lote por `dias_encerrados`. Para reproduzir o comportamento atual do comando, o consumidor teria de conferir também as guardas que o comando realmente usa: veredito presente, nenhum marco pendente e os minutos do dia apurados pela fonte de HE. O evento carrega só a identidade da célula e do dia. Cobertura de todos os casos que se tornam elegíveis só pela passagem do tempo [não verificado]. `app/ponto/portas/he.py:277–297`, `app/ponto/portas/he.py:330–372`, `app/ponto/turnos.py:2038–2083`, `app/ponto/services/cartorio.py:1179–1183`, `app/ponto/management/commands/recusar_ponta_pequena.py:141–170`, `app/ponto/services/he_pendente_lavrado.py:132–152`, `app/escala/models.py:1856–1874`.

## 24/11 11:03 · sessao d51b367827f3 · CHAMADO-PRESO-NA-REGENERACAO · sonda · origem=Code

## Saída da sonda

Transcrição literal do dado raspado fornecido. Uma nova execução em produção e a correspondência da cópia local com o hash informado ficaram [nao verificado].

```text
sonda_leitura: script.py [0f08b205cc56c7ae] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

IRMAO 37755 batida_ausente resolvido sistema 2026-11-24 08:53:52.546186-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:53:52.550116-03:00 estado_chamado 657 None status aberto -> resolvido via sistema
IRMAO 36950 batida_ausente resolvido sistema 2026-11-24 08:53:58.983017-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:53:58.986773-03:00 estado_chamado 657 None status aberto -> resolvido via sistema
IRMAO 36509 batida_ausente resolvido sistema 2026-11-24 08:54:08.627422-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:54:08.631020-03:00 estado_chamado 657 None status em_analise -> resolvido via sistema
IRMAO 36125 batida_ausente resolvido sistema 2026-11-24 08:59:31.737533-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:59:31.741836-03:00 estado_chamado 657 None status aberto -> resolvido via sistema
IRMAO 35721 batida_ausente resolvido sistema 2026-11-24 08:59:39.516072-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:59:39.519572-03:00 estado_chamado 657 None status aberto -> resolvido via sistema
IRMAO 34898 par_relampago resolvido sistema 2026-11-24 08:59:48.979320-03:00 ['data_turno', 'duracao_min', 'par']
   LOG 2026-11-24 08:59:48.983994-03:00 estado_chamado 657 None status aberto -> resolvido via sistema
IRMAO 34719 batida_ausente resolvido sistema 2026-11-24 08:59:49.167854-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:59:49.172127-03:00 estado_chamado 657 None status em_analise -> resolvido via sistema
IRMAO 34239 batida_ausente resolvido sistema 2026-11-24 08:59:55.152586-03:00 ['atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 08:59:55.156144-03:00 estado_chamado 657 None status em_analise -> resolvido via sistema
IRMAO 33274 batida_ausente resolvido sistema 2026-11-24 09:00:01.255964-03:00 ['atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
   LOG 2026-11-24 09:00:01.260303-03:00 estado_chamado 657 None status em_analise -> resolvido via sistema
IRMAO 34248 batida_ausente resolvido sistema 2026-11-24 10:39:25.481156-03:00 ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes']
   LOG 2026-11-24 10:39:25.484783-03:00 estado_chamado 657 None status em_analise -> resolvido via sistema
JANELA_TOTAL 12
JANELA_ACOES [('estado_chamado', 7), ('urgencia_escalada', 3), ('fechamento_recusado', 1), ('nascimento_chamado', 1)]
   J 2026-11-24 08:59:31.741836-03:00 estado_chamado 36125 657 None status aberto -> resolvido via sistema
   J 2026-11-24 08:59:39.519572-03:00 estado_chamado 35721 657 None status aberto -> resolvido via sistema
   J 2026-11-24 08:59:48.983994-03:00 estado_chamado 34898 657 None status aberto -> resolvido via sistema
   J 2026-11-24 08:59:49.172127-03:00 estado_chamado 34719 657 None status em_analise -> resolvido via sistema
   J 2026-11-24 08:59:55.156144-03:00 estado_chamado 34239 657 None status em_analise -> resolvido via sistema
   J 2026-11-24 09:00:01.260303-03:00 estado_chamado 33274 657 None status em_analise -> resolvido via sistema
   J 2026-11-24 09:00:12.470016-03:00 estado_chamado 40763 None sistema:vigia_de_hora status aberto -> resolvido via sistema
   J 2026-11-24 09:00:15.875421-03:00 urgencia_escalada 19356 None sistema:processar_alertas_avancados urgencia normal -> urgente (turno aberto 25h)
   J 2026-11-24 09:00:18.282034-03:00 nascimento_chamado 40799 None sistema:vigia_de_hora nasceu aberto (batida_ausente) via abrir
   J 2026-11-24 09:00:27.277151-03:00 urgencia_escalada 4923 None sistema:processar_alertas_avancados urgencia normal -> urgente (turno aberto 26h)
   J 2026-11-24 09:00:27.936259-03:00 urgencia_escalada 7133 None sistema:processar_alertas_avancados urgencia normal -> urgente (turno aberto 24h)
   J 2026-11-24 09:00:31.946015-03:00 fechamento_recusado 40780 None sistema:vigia_de_hora fechamento recusado: 1 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#61899) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 40589 2026-11-23 09:42:46.953007-03:00 fechamento_recusado fechamento recusado: 2 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#61522, #61523) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 40589 2026-11-23 09:36:16.839841-03:00 nascimento_chamado nasceu aberto (batida_ausente) via abrir
SEIS 40588 2026-11-23 09:42:48.084008-03:00 fechamento_recusado fechamento recusado: 2 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#61520, #61521) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 40588 2026-11-23 09:36:14.980104-03:00 nascimento_chamado nasceu aberto (batida_ausente) via abrir
SEIS 37336 2026-11-16 14:05:31.053330-03:00 fechamento_recusado fechamento recusado: 1 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#54538) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 37336 2026-11-06 10:30:24.379918-03:00 nascimento_chamado nasceu aberto (batida_ausente) via abrir
SEIS 36687 2026-11-04 07:50:15.712731-03:00 fechamento_recusado fechamento recusado: 2 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#53534, #53584) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 36687 2026-11-04 07:41:08.545113-03:00 fechamento_recusado fechamento recusado: 3 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#53408, #53534, #53584) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 36687 2026-11-04 07:41:05.886969-03:00 fechamento_recusado fechamento recusado: 4 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#53408, #53534, #53580, #53584) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 36687 2026-11-03 08:30:26.192757-03:00 nascimento_chamado nasceu aberto (batida_ausente) via abrir
SEIS 35243 2026-11-09 11:04:00.123771-03:00 fechamento_recusado fechamento recusado: 2 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#50720, #50749) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 35243 2026-10-27 12:30:32.934892-03:00 nascimento_chamado nasceu aberto (batida_ausente) via abrir
SEIS 34070 2026-11-10 13:27:13.672651-03:00 fechamento_recusado fechamento recusado: 1 pergunta(s) VIVA(s) da competencia aberta ancorada(s) neste chamado (#49602) e nenhuma cobranca viva do mesmo colab-dia que as receba
SEIS 34070 2026-10-24 08:04:24.866248-03:00 estado_chamado status resolvido -> em_analise via -
SEIS 34070 2026-10-23 16:46:57.184404-03:00 estado_chamado status em_analise -> resolvido via sistema
SEIS 34070 2026-10-23 12:30:17.614677-03:00 estado_chamado status resolvido -> em_analise via -
SEIS 34070 2026-10-23 07:55:22.637748-03:00 estado_chamado status aberto -> resolvido via sistema
FIM
```

## 1. Quem fechou os irmãos entre 08:55 e 09:05?

**A função de transição é `ChamadoColaborador.reconciliar()`; o chamador é [nao verificado].** Os seis irmãos da janela têm `estado_chamado`, `usuario_id=657` e `ator=None`. No código da cópia, `reconciliar()` chama `_gravar_estado` sem passar um usuário e com a via `sistema`. Nesse caminho, o `657` vem do usuário autenticado da requisição. A descrição registra a via e o campo `ator` recebe a assinatura `sistema:<comando>` quando a escrita ocorre fora de uma requisição em `manage.py`. `app/chamados/models.py:674` `app/chamados/models.py:304` `app/chamados/catalogo/motor.py:95` `app/core/observ.py:22` `app/core/observ.py:108` `app/core/observ.py:134`

O fechamento que traz `ator=sistema:vigia_de_hora` é o do **40763**, às 09:00:12. A configuração agenda `vigia_de_hora --apply` a cada 10 minutos. O comando chama `rodar()`, que chama `cartorio.julgar_celula()`. A origem exata da invocação em produção e o ramo que fechou o 40763 ficam [nao verificado]. `app/config/crons.py:367` `app/ponto/management/commands/vigia_de_hora.py:23` `app/ponto/services/vigia_de_hora.py:134`

## 2. Qual filtro deixou de fora os seis chamados?

**O filtro que atuou nessa janela é [nao verificado].** Para 40589, 40588, 37336, 36687, 35243 e 34070, a saída mostra recusas anteriores com a mensagem de `_gate_pergunta_viva()`. Esse gate retorna `False` quando há pergunta viva ancorada e pelo menos uma delas fica sem cobrança viva válida do mesmo colaborador-dia para recebê-la. Se essa condição ainda valia na janela, fica [nao verificado]. `app/chamados/models.py:408` `app/chamados/models.py:438` `app/chamados/models.py:708`

Com os valores medidos na sombra, o filtro exato do fechador por lastro seria `if cel is None or marcos_acesos(ch, cel) is not True: continue`. `None` satisfaz essa condição. No 34070, a busca da lâmpada confere hora e tipo, então `12:00 S` não satisfaz `12:00 E`. A execução desse fechador para os seis na janela fica [nao verificado]. `app/ponto/services/lastro.py:395` `app/ponto/services/lastro.py:89` `app/ponto/services/lastro.py:174`

O cartório tem um ramo que tenta `reconciliar()` para os chamados em `vivos` com `marcos_inexistentes(x, _ata_agora)`, usando a ata daquele julgamento. A própria guarda de premissa aceita `marcos_inexistentes(...) is True` e a guarda de pergunta viva ainda pode recusar o fechamento. Qual ata e quais perguntas foram avaliadas para esses seis na janela fica [nao verificado]. `app/ponto/services/cartorio.py:1464` `app/ponto/services/cartorio.py:1596` `app/chamados/models.py:1363` `app/chamados/models.py:708`

## 3. Os irmãos fechados tinham `orfa_regeneracao`?

Sim, em **cinco dos seis** irmãos fechados na janela, segundo as chaves impressas em `IRMAO`. A chave aparece em **36125, 35721, 34719, 34239 e 33274**, todos `batida_ausente`. Está ausente em **34898**, de `par_relampago`. No bloco inteiro, os três irmãos das 08:53–08:54 também trazem a chave, e o **34248**, das 10:39, não a traz. O contexto no instante exato de cada fechamento fica [nao verificado].

## 24/11 10:52 · sessao ea99ce1fa9b6 · CHAMADO-PRESO-NA-REGENERACAO · sonda · origem=Code

Não localizei o `script.py` na cópia nem executei a sonda na sombra [nao verificado]. O lançador exige o arquivo e cria uma cópia temporária, o que viola a regra de só leitura. [bin/sonda_leitura.sh:71](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/bin/sonda_leitura.sh:71) [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/bin/sonda_leitura.sh:216)

**Saída raspada fornecida, verbatim**

```text
sonda_leitura: script.py [bf7bfbe2913109f7] no cpuset 4-7, lendo sombra como leitor (LEITURA)

CH 40589 status aberto cat Ausencia sub Batida nao realizada celfk 122470 col 549 data 2026-11-22 modulo batida_ausente criado 2026-11-23 09:36:16.838601-03:00
  cobrados [('11:00', 'S'), ('12:00', 'E')] chaves_ctx ['atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
  cel 122470 veredito cobrado via cartorio regen 2026-11-23 12:42:46.737979+00:00 nregen 3 julgada 2026-11-23 12:42:46.839854+00:00
  lampadas [('07:00', 'E', True, None), ('19:00', 'S', True, None)]
  apagados [] inexist True acesos None
CH 40588 status aberto cat Ausencia sub Batida nao realizada celfk 122468 col 549 data 2026-11-20 modulo batida_ausente criado 2026-11-23 09:36:14.978587-03:00
  cobrados [('11:00', 'S'), ('12:00', 'E')] chaves_ctx ['atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
  cel 122468 veredito cobrado via cartorio regen 2026-11-23 12:42:47.819535+00:00 nregen 3 julgada 2026-11-23 12:42:47.988730+00:00
  lampadas [('07:00', 'E', True, None), ('19:00', 'S', True, None)]
  apagados [] inexist True acesos None
CH 37336 status aberto cat Ausencia sub Batida nao realizada celfk 129885 col 932 data 2026-11-06 modulo batida_ausente criado 2026-11-06 10:30:24.378583-03:00
  cobrados [('10:00', 'E')] chaves_ctx ['atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
  cel 129885 veredito cobrado via cartorio regen 2026-11-16 17:05:26.602427+00:00 nregen 1 julgada 2026-11-16 17:05:30.948640+00:00
  lampadas [('07:00', 'E', True, None), ('19:00', 'S', True, None)]
  apagados [] inexist True acesos None
CH 36687 status aberto cat Ausencia sub Batida nao realizada celfk 130781 col 899 data 2026-11-03 modulo batida_ausente criado 2026-11-03 08:30:26.191567-03:00
  cobrados [('08:00', 'E'), ('14:00', 'E')] chaves_ctx ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
  cel 130781 veredito cobrado via cartorio regen 2026-11-04 10:50:15.360931+00:00 nregen 2 julgada 2026-11-05 10:12:09.623972+00:00
  lampadas [('13:00', 'E', True, None), ('17:00', 'S', True, None), ('18:00', 'E', True, None), ('21:00', 'S', True, None)]
  apagados [] inexist True acesos None
CH 35243 status aberto cat Ausencia sub Batida nao realizada celfk 129565 col 872 data 2026-10-27 modulo batida_ausente criado 2026-10-27 12:30:32.933634-03:00
  cobrados [('12:00', 'S'), ('13:00', 'E')] chaves_ctx ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
  cel 129565 veredito cobrado via cartorio regen 2026-11-09 14:03:59.290694+00:00 nregen 1 julgada 2026-11-09 14:04:00.023146+00:00
  lampadas [('07:00', 'E', True, None), ('19:00', 'S', True, None)]
  apagados [] inexist True acesos None
CH 34070 status em_analise cat Ausencia sub Batida nao realizada celfk 130026 col 941 data 2026-10-23 modulo batida_ausente criado 2026-10-23 07:15:11.389386-03:00
  cobrados [('12:00', 'E')] chaves_ctx ['_marco_por', 'atraso_min', 'data_turno', 'horario_previsto', 'marcos_faltantes', 'orfa_regeneracao']
  cel 130026 veredito cobrado via cartorio regen 2026-11-10 16:27:13.281724+00:00 nregen 1 julgada 2026-11-14 10:12:14.056259+00:00
  lampadas [('08:00', 'E', True, None), ('12:00', 'S', True, None), ('13:00', 'E', True, None), ('17:00', 'S', True, None)]
  apagados [] inexist True acesos None
FIM
```

O cartório seleciona candidatos com `marcos_inexistentes=True` na ata do julgamento. Se a porta ler a mesma ata exibida, esse `True` satisfaz a guarda da premissa. Uma pergunta viva sem destino válido ainda pode impedir o fecho. A saída fornecida não traz o resultado de `reconciliaria()` nem os dados dessa guarda. [app/ponto/services/cartorio.py:1597](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/cartorio.py:1597) [app/chamados/models.py:804](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/chamados/models.py:804) [app/chamados/models.py:1363](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/chamados/models.py:1363) [app/chamados/models.py:429](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/chamados/models.py:429) [nao verificado]

- **40589** — `marcos_inexistentes=True` porque `11:00 S` e `12:00 E` estão ausentes da ata (saída, linhas 4, 6 e 7). Recusa de `reconciliaria()` e condição que impediu o fecho [nao verificado].
- **40588** — `marcos_inexistentes=True` porque `11:00 S` e `12:00 E` estão ausentes da ata (saída, linhas 9, 11 e 12). Recusa de `reconciliaria()` e condição que impediu o fecho [nao verificado].
- **37336** — `marcos_inexistentes=True` porque `10:00 E` está ausente da ata (saída, linhas 14, 16 e 17). Recusa de `reconciliaria()` e condição que impediu o fecho [nao verificado].
- **36687** — `marcos_inexistentes=True` porque `08:00 E` e `14:00 E` estão ausentes da ata (saída, linhas 19, 21 e 22). Recusa de `reconciliaria()` e condição que impediu o fecho [nao verificado].
- **35243** — `marcos_inexistentes=True` porque `12:00 S` e `13:00 E` estão ausentes da ata (saída, linhas 24, 26 e 27). Recusa de `reconciliaria()` e condição que impediu o fecho [nao verificado].
- **34070** — `marcos_inexistentes=True` porque o cobrado é `12:00 E` e a ata tem `12:00 S` (saída, linhas 29, 31 e 32). Recusa de `reconciliaria()` e condição que impediu o fecho [nao verificado].

## 24/11 10:48 · sessao 6cc13324a081 · CHAMADO-PRESO-NA-REGENERACAO · leitura · origem=Code

O fechador de **marco aceso** pula o chamado quando o marco sumiu da ata. O **cartório** tem um ramo próprio que tenta encerrar por marco inexistente e passa pela mesma porta de estado. Qual ramo rodou e qual guarda decidiu o caso de 27/10, após a regeneração relatada em 09/11, fica [nao verificado]. [app/ponto/services/lastro.py:174–181,388–403] [app/ponto/services/cartorio.py:1593–1610] [nao verificado]

## 1. Porta canônica e chamadores

A porta de estado é `ChamadoColaborador.reconciliar()`. O executor de lastro é `fechar_pelo_marco_colab(colab, falhas=None)`. Para cada candidato, ele chama `transicionar_com_pauta(ch, 'resolvido', None, ato=ch.reconciliar)` e, no sucesso, grava no mesmo `atomic` a nota `lastro_encontrado`, com o dia e o que acendeu os marcos. [app/chamados/models.py:661–675] [app/ponto/services/lastro.py:343–361,364–403]

A trilha de estado é `LogAuditoria.acao='estado_chamado'`, com estado anterior, destino e via. O chamado ganha `resolvido_em` e `via_encerramento='sistema'`. Sem ator explícito, a auditoria usa o usuário autenticado da requisição ou o ator do processo. A auditoria é *best effort*. O motivo específico do lastro fica no texto da nota do fio. [app/chamados/models.py:240–258,304–308,670–675] [app/chamados/catalogo/motor.py:88–96] [app/core/observ.py:94–118,133–139] [app/core/models.py:295–307] [app/ponto/services/lastro.py:343–361]

| Função que tenta fechar | Quem a aciona |
|---|---|
| `lastro.fechar_pelo_marco_colab` | `reconciliador.consumir_eventos_celula`, pelo comando agendado a cada minuto. [app/chamados/reconciliador.py:1515–1519] [app/chamados/management/commands/consumir_eventos_celula.py:25–31] [app/config/crons.py:393–394] |
| `cartorio._julgar_colab_corpo` e `cartorio.julgar_colab` | O corpo tenta o fecho imediato e o de marco inexistente. `julgar_colab` tenta os adiados depois de lavrar e retratar as perguntas absolvidas. Entram pelo cron `processar_cartorio --apply` e, via `julgar_celula`, por `vigia_de_hora --apply`, pela regeneração, pelos signals de batida, ausência, justificativa e criação ou mudança de estado do chamado, e pela view `cobrar_dia_view`. [app/ponto/services/cartorio.py:1015–1048,1568–1610,2087–2090] [app/config/crons.py:367,616] [app/ponto/management/commands/processar_cartorio.py:59–65] [app/ponto/services/vigia_de_hora.py:134] [app/ponto/portas/celula.py:467–479] [app/ponto/signals.py:85–93,131–165] [app/chamados/signals.py:189–218] [app/chamados/views_cobrar.py:323–343] [app/chamados/services/cobrar_dia.py:39–41] |
| `detectar_ausencias.Command._fechar_chamado` | O `handle` do cron de cinco minutos chama após encontrar entrada e obter `marcos_acesos_do_dia(...) is True`. [app/config/crons.py:364] [app/ponto/management/commands/detectar_ausencias.py:396–413,845–854] |
| `entrada_sincronizada.resolver_cobranca_da_entrada` | As duas implementações de `api_bater_ponto` chamam para batida `E`. O serviço seleciona a cobrança viva do dia, exige os marcos acesos e passa pela porta de sistema. [app/api/views.py:790–795] [app/api/views_core.py:784–789] [app/ponto/services/entrada_sincronizada.py:19–37] |
| `curar_residuo_bug115.Command._ata_sem_furo` | Comando avulso com `--apply`, para o acervo sem `marcos_faltantes` declarado e com ata que tem lâmpadas, todas fora da lista de apagadas. Usa a mesma porta e anota o motivo no fio. [app/config/crons.py:1477–1478] [app/ponto/management/commands/curar_residuo_bug115.py:39–48,63–109,153–162] |

`fechar_cobranca_com_lastro --alarme` hoje é o vigia horário. O comando mede e alarma, e a escrita por marco aceso do outbox fica com `fechar_pelo_marco_colab`. [app/config/crons.py:729–732] [app/ponto/management/commands/fechar_cobranca_com_lastro.py:40–83]

## 2. Quando o marco deixou de existir

- **No lastro por marco aceso, ignora.** A procura é por hora e tipo na ata. Se a lâmpada procurada não existe, `marcos_acesos` retorna `None` em `lastro.py:176–177`. O `is not True` seguido de `continue` em `lastro.py:395–396` pula o chamado antes da transição. Esse pulo não gera `fechamento_recusado`. [app/ponto/services/lastro.py:82–95,170–181,391–403]
- **No cartório, tenta fechar.** `marcos_inexistentes` exige uma ata com lâmpadas, pelo menos um marco cobrado, tipo em todos os marcos cobrados e ausência de **todos** eles na ata. A decisão está em `lastro.py:110–115`. O cartório chama `reconciliar()` em `cartorio.py:1598`. A premissa aceita a inexistência em `models.py:1363–1364`, e a guarda de pergunta viva ainda pode recusar. Só o sucesso gera a nota `marco_inexistente` e põe a célula na segunda passada. [app/ponto/services/lastro.py:98–115] [app/ponto/services/cartorio.py:1596–1610] [app/chamados/models.py:429–450,701–712,1345–1367]

`marcos_apagados(cel) == []` também ocorre com ata sem lâmpadas ou com lâmpadas pendentes. A lista vazia, sozinha, é insuficiente para decidir se `marcos_inexistentes` daria `True`. [app/ponto/services/lastro.py:110–135]

## 3. O que sai dos escritores de DNA

| Escritor | Efeito após a escrita |
|---|---|
| `regenerar_celulas_vinculo`, também usado por `regenerar_celulas_dia` | A implementação está na porta da célula. Os saves de DNA não inserem `EventoCelula` diretamente. A porta registra a regeneração e marca no contexto as cobranças vivas elegíveis cujo DNA foi reescrito depois da emissão. A marca preserva os marcos da emissão e, por si, não muda o estado. Em célula futura reescrita, limpar um julgamento anterior pode emitir evento. [app/ponto/portas/celula.py:112,251–327,369–394,796–800,1181–1207] [app/ponto/management/commands/gerar_celulas.py:232–265] [app/escala/services/regeneracao.py:141–155] |
| `editar_dia`, também usado por `trocar_plantao` | Grava DNA e trilha, marca órfãs e chama a mesma rotina posterior da regeneração. [app/ponto/portas/celula.py:654–684,729–737] |
| `reverter_regeneracao_dia` | Repõe o DNA anterior, insere `EventoCelula` explicitamente no `atomic` e chama a mesma rotina posterior. [app/ponto/portas/celula.py:865–895] |
| `gerar_celulas_periodo` | Cria célula ou preenche DNA nulo no passado. Nessa função, o fluxo termina sem lavra ou outbox. O comando `gerar_celulas` também chama a porta de regeneração para células já existentes de hoje, com `recalculo=False`. [app/ponto/portas/celula.py:948–972] [app/ponto/management/commands/gerar_celulas.py:303–325,345–350] |

A rotina posterior chama `julgar_celula(..., apply_=True, forcar=True)` para dias tocados até hoje sem veredito humano, quando `recalculo=True`. O caminho de frota passa `recalculo=False`. Em ambos, `on_commit` rejunta chamados vivos de escopo dia com FK nula e reavalia as faltas dos dias tocados, tentando encerrar as de feriado ou folga. O gatilho é a chamada da porta. Os signals de vigência e âncora recebem `EscalaColaborador` e chamam a regeneração. [app/ponto/portas/celula.py:439–441,467–503,535–547] [app/escala/services/cadastro_tipo.py:353–364] [app/chamados/services/feriado_reavaliar.py:29–57,65–85] [app/escala/signals.py:34–83,86–134]

Quando a lavra grava `ata` ou `veredito`, `salvar_e_emitir` insere o evento na mesma transação. O consumidor agrupa eventos **por colaborador** e chama, entre outras rotinas, `fechar_pelo_marco_colab`. A regeneração chega ao lastro por essa lavra e pelo consumidor, ou à tentativa por marco inexistente dentro do próprio cartório. [app/ponto/services/cartorio.py:1898–1922] [app/ponto/portas/celula.py:977–1001,1217–1237] [app/chamados/reconciliador.py:1484–1523]

No julgamento, um chamado em dia de trabalho ainda protesta quando `marcos_acesos` devolve `None`. Se não houver códigos e houver chamado vivo com fio vivo que protesta, o cartório pode lavrar `cobrado`. Um chamado vivo do dia também cala a emissão ordinária de outra cobrança. Se foi essa a sequência no caso relatado, [nao verificado]. [app/ponto/services/cartorio.py:1412–1417,1492–1501,1514–1538,1775–1796]

## 4. Vigia de vivo sem marco apagado

**Não localizei um contador geral com esse predicado** [nao verificado]. Os contadores mais próximos têm estes recortes.

- `curar_residuo_bug115` conta `ata_sem_furo`. `nomear_marco_por_lampada` conta `celula sem lampada apagada`. Ambos começam na fila de `batida_ausente` **sem** `contexto_json.marcos_faltantes`. Um chamado com o marco antigo ainda declarado fica fora desse recorte. [app/ponto/management/commands/curar_residuo_bug115.py:39–48,75–90] [app/chamados/management/commands/nomear_marco_por_lampada.py:47–64]
- `supra_juiz` conta `COBRANCA_SEM_FURO` com dia de trabalho, previsto positivo, realizado suficiente e todas as células casadas. O contador `COBRANCA_ORFA_REGENERACAO` fica no ramo de lavra `cobranca_indevida` com código. O invariante `C3` de `auditar_invariantes_chamados` exige `concorde`. [app/ponto/supra_juiz.py:476–484] [app/ponto/management/commands/supra_juiz.py:246–278,297–310] [app/chamados/management/commands/auditar_invariantes_chamados.py:1635–1638]
- `fechar_cobranca_com_lastro` mede lastro encontrado, retenção pelo gate e casos que escapam ao evento. Já `out['marco_inexistente']` do cartório conta **fechos bem-sucedidos** por esse motivo. [app/ponto/services/lastro.py:284–323,412–424] [app/ponto/management/commands/fechar_cobranca_com_lastro.py:53–83] [app/ponto/services/cartorio.py:1596–1609]

## 5. Argumentos e guardas para um chamado específico

- **Por chamado:** a porta recebe a instância `ch`. O padrão interno é `transicionar_com_pauta(ch, 'resolvido', None, ato=ch.reconciliar)`. Embora `reconciliar` declare `novo_status='resolvido'`, o corpo usa o destino do motor, que leva `aberto`, `em_analise` e `registrado` a `resolvido`. A assinatura não recebe `staff`, ator, motivo ou via. A via é fixada em `sistema`. A nota com o motivo é responsabilidade do chamador. O executor `fechar_pelo_marco_colab` recebe um colaborador, sem filtro por ID de chamado, e usa a fila que exclui silenciados e `registrado`. [app/chamados/models.py:661–675,690–695] [app/chamados/catalogo/motor.py:88–111] [app/chamados/services/fecho_runtime.py:146–169] [app/ponto/services/lastro.py:364–403] [app/chamados/services/painel.py:158–186]
- **Guardas efetivas:** estado atual relido, transição permitida, premissa regularizada ou exceção de agrupamento ou deduplicação, guarda de conversa quando é chamado-pai e guarda de pergunta viva ancorada. Se as perguntas vivas puderem ser reatadas a outra cobrança válida do mesmo dia, a porta reata. Caso contrário, recusa e registra o motivo. `reconciliaria()` consulta a mesma decisão em leitura. [app/chamados/models.py:393–427,429–464,677–716,718–737,783–837]
- **Exportada e trancada:** `reconciliar()` não tem veto geral pela competência do chamado. Na guarda de perguntas, a coleta pula perguntas de competência entregue ou de dia indecidível. O juiz de pergunta viva também considera morta por leitura a pergunta de dia trancado sem toque posterior de admin. A retratação por `retratar_com_motivo` tem uma cerca adicional sobre escritas de perguntas entregues. A exigência explícita de `staff` está na porta de **regenerar célula em competência exportada**, `regenerar_em_exportada`. [app/chamados/models.py:368–388,683–716] [app/chamados/services/vigia_fio.py:789–811] [app/chamados/juizes.py:1023–1042,1494–1503] [app/chamados/reconciliador.py:558–599] [app/ponto/portas/regen_exportada.py:107–117]

## 24/11 10:48 · sessao 80a94dcdbfad · - · - · origem=Ronald (tmux)

### 1. Porta e chamadores

A porta de estado para um chamado é `ChamadoColaborador.reconciliar()`, normalmente via `transicionar_com_pauta(ch, 'resolvido', None, ato=ch.reconciliar)`. O fecho especificamente por lastro é `fechar_pelo_marco_colab(colab, falhas=None)`. Ele exige todos os marcos cobrados acesos e grava a ocorrência `lastro_encontrado` no mesmo `atomic` do fechamento. `reconciliar` carimba `resolvido_em`, via `sistema` e auditoria `estado_chamado`. `core.observ.evento` atribui o ator à requisição autenticada ou ao processo. A nota aponta a batida e o dia ou discrimina dispensa/isenção. `app/chamados/models.py:661-675,240-258,301-308`, `app/chamados/catalogo/motor.py:88-96`, `app/chamados/services/fecho_runtime.py:146-169`, `app/ponto/services/lastro.py:343-361,364-403`, `app/core/observ.py:94-117,133-138`, `app/core/models.py:295-307`.

- `fechar_pelo_marco_colab` é chamado pelo consumidor de `EventoCelula`, agendado a cada minuto. O comando `fechar_cobranca_com_lastro` roda no minuto 40 como vigia de lastro escapado. `app/chamados/reconciliador.py:1515-1523`, `app/chamados/management/commands/consumir_eventos_celula.py:26-31`, `app/config/crons.py:393-394,729-732`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:40-83`.
- Cartório fecha por marco em `_julgar_colab_corpo` para quem não protesta, incluindo todos os marcos acesos ou dia sem trabalho. Com fio no lote, `julgar_colab` faz lavra, E2, tentativa de fechamento e segunda lavra. O cartório registra `[RECONCILIA-MARCO]` e só grava a ocorrência específica do fecho por não protesto se houve justificativa rejeitada. Existe um ramo próprio para marcos inexistentes. `app/ponto/services/cartorio.py:48-56,1015-1048,1492-1495,1568-1610`.
- Chamam o cartório os signals de `Batida` e `Ausencia` no `post_save`/`post_delete`, de `Justificativa` no `post_save` e de nascimento/mudança de status de `ChamadoColaborador`; os crons `processar_cartorio --apply` / `vigia_de_hora --apply`; e a view `cobrar_dia_view` por `cobrar_dia`. Também há chamadas na mutação de célula, pós-vínculo e operações específicas. `app/ponto/signals.py:85-93,131-165`, `app/chamados/signals.py:189-218`, `app/config/crons.py:365-367,616`, `app/ponto/management/commands/processar_cartorio.py:59-65`, `app/ponto/services/vigia_de_hora.py:134`, `app/chamados/views_cobrar.py:323-343`, `app/chamados/services/cobrar_dia.py:27-41`, `app/ponto/portas/celula.py:467-485`, `app/colaboradores/services/pos_vinculo.py:51-77`, `app/escala/regua_defesa.py:680-697`, `app/ponto/management/commands/celula_veredito_velho.py:133-162`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`, `app/ponto/services/furos_sem_cobranca.py:117-128`, `app/chamados/reconciliador.py:811-827`.
- `detectar_ausencias.Command.handle` chama `_fechar_chamado` se encontrou entrada e `marcos_acesos_do_dia(...) is True`, com ocorrência `fechado_por_batida`. A view autenticada `api_bater_ponto` chama `resolver_cobranca_da_entrada` para `E`, com a mesma exigência e ocorrência `entrada_sincronizada_resolveu`. `app/config/crons.py:364`, `app/ponto/management/commands/detectar_ausencias.py:396-413,845-854`, `app/api/urls.py:67`, `app/api/views.py:386-392,790-795`, `app/api/views_core.py:372-378,784-789`, `app/ponto/services/entrada_sincronizada.py:19-37`.
- Comando avulso `curar_residuo_bug115 --apply` chama `_ata_sem_furo` quando há ata com lâmpadas e nenhuma apagada para `batida_ausente` da fila sem `contexto_json.marcos_faltantes`. Usa o mesmo gate e ocorrência `lastro_ata_sem_furo`. `app/ponto/management/commands/curar_residuo_bug115.py:39-48,63-93,108-109,153-162`, `app/config/crons.py:1477-1478`.
- Pela retratação de dia/premissa, `supra_juiz --executar` usa `reconciliador.retratar` sob `cobranca_indevida` lavrada com código, guarda de conversa e exclusão de órfã por reescrita. `reconciliar_grade --apply` também usa `retratar` por grade limpa, cobertura ou premissa morta, com cron em dry-run. Essas rotas culminam em `reconciliar`. `app/ponto/management/commands/supra_juiz.py:245-290`, `app/ponto/management/commands/reconciliar_grade.py:261-316`, `app/config/crons.py:460-462,599-604`, `app/chamados/reconciliador.py:558-626`.

### 2. Marco inexistente

O fecho estrito do lastro **ignora**. A procura se faz por hora e tipo, `marcos_acesos` devolve `None` ao encontrar uma lâmpada ausente, e o `continue` de `fechar_pelo_marco_colab` evita a própria chamada ao gate, portanto não há nota de sucesso nem trilha `fechamento_recusado` desse caminho. `app/ponto/services/lastro.py:82-95,170-181,391-403`.

O cartório **tenta fechar** na hipótese distinta `marcos_inexistentes`: ata do julgamento com lâmpadas, cobrança por marcos nomeados e com tipo, todos ausentes na ata. O gate de premissa reconhece essa situação como regularizada e a guarda de pergunta viva ainda pode recusar ou reatar. Sucesso gera ocorrência `marco_inexistente`, contador, log e segunda passada. `app/ponto/services/lastro.py:98-115`, `app/ponto/services/cartorio.py:1593-1610,1044-1048`, `app/chamados/models.py:1345-1367,429-458,683-716`.

### 5. Chamada singular e guardas

`reconciliar(self, novo_status='resolvido')` atua no objeto. Embora aceite `novo_status`, o corpo escolhe o destino pela matriz `RECONCILIAR`, que mapeia `aberto`, `em_analise` e `registrado` para `resolvido`. A via é fixa `sistema`, sem argumento staff, usuário, motivo ou via. O gate relê o estado, exige premissa regularizada ou uma exceção prevista, consulta conversa do chamado-pai quando aplicável e protege pergunta viva ancorada. Para reproduzir a prova por lastro singular, o chamador também precisa verificar todos os marcos cobrados acesos e anexar a nota explicativa após sucesso, como faz o wrapper por colaborador. `app/chamados/models.py:661-739,761-839`, `app/chamados/catalogo/motor.py:88-111`, `app/ponto/services/lastro.py:388-403`.

Exportada ou trancada não é veto direto ao estado do chamado no gate de sistema. O filtro de perguntas ancoradas pula dia entregue ou indecidível por K6, e `pergunta_viva` cala pergunta de dia trancado sem toque posterior. O resolver S143 tem cerca de escrita na pergunta entregue, em seu próprio ato. A retratação S128 separada cerca a unidade inteira ao tentar escrever pergunta protegida. `app/chamados/models.py:368-388,661-716`, `app/chamados/services/vigia_fio.py:789-811`, `app/chamados/juizes.py:1023-1042,1494-1503`, `app/chamados/signals.py:91-119`, `app/chamados/services/disputa_emissao.py:1985-2012`, `app/chamados/reconciliador.py:558-620`, `app/chamados/services/fio.py:1531-1556,1559-1569`.

### Complemento do vigia

`nomear_marco_por_lampada` tem a linha literal `celula sem lampada apagada`, restrita à fila `batida_ausente` sem `marcos_faltantes` e incluindo célula ausente. `curar_residuo_bug115` separa célula ausente e ata sem lâmpadas no mesmo subconjunto. `COBRANCA_SEM_FURO` exige dia de trabalho com previsto, realizado suficiente, células de grade todas casadas e chamado vivo. `COBRANCA_ORFA_REGENERACAO` é contado apenas no ramo de `cobranca_indevida` lavrada com código e sem retenção por conversa. O `marco_inexistente` do cartório conta fechos que de fato ocorreram. Um contador do conjunto inteiro descrito na pergunta [nao verificado]. `app/chamados/management/commands/nomear_marco_por_lampada.py:47-64,89-91`, `app/ponto/management/commands/curar_residuo_bug115.py:39-48,75-93`, `app/ponto/supra_juiz.py:476-484`, `app/ponto/management/commands/supra_juiz.py:245-278,304-308`, `app/ponto/services/cartorio.py:1597-1609`.

## 24/11 10:48 · sessao 37db1e494598 · - · - · origem=Ronald (tmux)

Na cópia, não localizei contador geral de todos os `batida_ausente` em `VIVOS` com célula existente e `marcos_apagados(cel) == []`. [nao verificado]

- `curar_residuo_bug115` conta a classe `ata_sem_furo` quando há lâmpadas na ata e nenhuma apagada, mas seleciona apenas chamados da fila cujo `contexto_json.marcos_faltantes` está vazio. `nomear_marco_por_lampada` imprime `celula sem lampada apagada` no mesmo recorte de chamados sem marco declarado e pode incluir célula ausente. São comandos de acervo. [curar_residuo_bug115.py:45](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/curar_residuo_bug115.py:45) [curar_residuo_bug115.py:75](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/curar_residuo_bug115.py:75) [nomear_marco_por_lampada.py:47](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/chamados/management/commands/nomear_marco_por_lampada.py:47) [crons.py:1372](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/config/crons.py:1372) [crons.py:1477](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/config/crons.py:1477)
- `supra_juiz` conta `COBRANCA_SEM_FURO` por colaborador-dia de trabalho, com previsto positivo, realizado ≥ previsto, todas as células casadas e chamado vivo de furo. `COBRANCA_ORFA_REGENERACAO` aparece no outro ramo, condicionado a lavra `cobranca_indevida` com código correspondente e à guarda de conversa. `fila_de_orfas` lista chamados de furo vivos com marco nomeado e reescrita posterior à criação, sem conferir as lâmpadas. [ponto/supra_juiz.py:476](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/supra_juiz.py:476) [commands/supra_juiz.py:223](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/supra_juiz.py:223) [commands/supra_juiz.py:246](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/supra_juiz.py:246) [commands/supra_juiz.py:275](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/supra_juiz.py:275) [regeneracao.py:141](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/escala/services/regeneracao.py:141) [regeneracao.py:158](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/escala/services/regeneracao.py:158)
- A auditoria C3 mede `VIVOS` em `celula__veredito='concorde'` para quatro módulos, inclusive `batida_ausente`. O vigia horário `fechar_cobranca_com_lastro --alarme` mede quem já tem lastro e fecharia pela porta, mas escapou do evento. Seu `sem_marco` é ausência de marco declarado no chamado. O contador `marco_inexistente` do cartório mede fechamentos aceitos. [auditar_invariantes_chamados.py:1635](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/chamados/management/commands/auditar_invariantes_chamados.py:1635) [lastro.py:273](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/lastro.py:273) [lastro.py:412](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/lastro.py:412) [crons.py:731](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/config/crons.py:731) [cartorio.py:1596](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/cartorio.py:1596)
- `reconciliar_grade` imprime `divergencia_grade_cartorio` e `orfa_dry` e roda no cron sem `--apply`. `orfa_dry` é a proposta de relavra de cobrança com `orfa_regeneracao` e furo detectado na grade. [reconciliar_grade.py:220](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/reconciliar_grade.py:220) [reconciliar_grade.py:329](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/reconciliar_grade.py:329) [reconciliar_grade.py:430](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/management/commands/reconciliar_grade.py:430) [crons.py:460](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/config/crons.py:460)

O lastro por lâmpada acesa devolve `None` quando a lâmpada do marco cobrado está ausente, e o fechador do evento pula o chamado. O cartório tem um ramo próprio para todos os marcos nomeados e tipados ausentes de uma ata com lâmpadas. O ramo tenta `reconciliar()` e registra `marco_inexistente` após aceitação. O gate pode recusar por pergunta viva sem reata na competência aberta. O código permite calcular `cobrado` quando não há códigos e há fio vivo de um chamado cujo dia segue de trabalho e cujos marcos não estão todos acesos. Qual caminho incidiu no caso de produção permanece [nao verificado]. [lastro.py:110](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/lastro.py:110) [lastro.py:175](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/lastro.py:175) [lastro.py:395](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/lastro.py:395) [cartorio.py:1492](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/cartorio.py:1492) [cartorio.py:1514](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/cartorio.py:1514) [cartorio.py:1537](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/cartorio.py:1537) [cartorio.py:1593](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/ponto/services/cartorio.py:1593) [chamados/models.py:429](/home/ronald/codex_ws/7a4e79eefb7bd2613e4826449b0b22a43401cfb5/app/chamados/models.py:429)

## 24/11 10:47 · sessao 055c866779be · - · - · origem=Ronald (tmux)

- **Escritores de `CelulaDia.dna.marcos`.** `montar_dna` produz o blob, e a porta concentra `regenerar_celulas_vinculo`, `editar_dia`, `reverter_regeneracao_dia` e `gerar_celulas_periodo`. `regenerar_celulas_dia` delega à regeneração de vínculo, e `trocar_plantao` chama `editar_dia` nos dois lados. `app/core/contratos_estruturais.py:113-125`, `app/ponto/portas/celula.py:74-85,292-315,641-664,729-737,796-800,865-878,948-972`.
- **Regeneração e edição.** O save de DNA não insere `EventoCelula`. A regeneração marca cobranças órfãs dos dias reescritos e audita o ato. A edição também marca órfãs e usa o mesmo processamento posterior. O marcador grava `orfa_regeneracao` com os marcos da emissão no contexto, condicionado à reescrita posterior à criação e a marcos nomeados. Esse save altera contexto e data de atualização, então o receiver do chamado retorna se o status for igual. `app/ponto/portas/celula.py:261-279,292-327,369-394,654-684`, `app/ponto/management/commands/gerar_celulas.py:232-265`, `app/escala/services/regeneracao.py:141-155`, `app/chamados/services/contexto.py:52,97-105`, `app/chamados/signals.py:189-195`.
- **Processamento posterior.** Por padrão, a porta força o cartório para dias tocados até hoje sem veredito humano e recalcula uma vez por competência. A frota passa `recalculo=False`, que pula essas duas etapas, e invalida fechamento. Em ambos os casos, `on_commit` rejunta a FK nula dos vivos de escopo dia do colaborador e reavalia nos dias tocados `batida_ausente` cuja premissa morreu por feriado/folga. O próprio cartório mantém a guarda da jornada de hoje. `app/ponto/portas/celula.py:439-441,467-503,535-547`, `app/escala/services/cadastro_tipo.py:353-364,375-378`, `app/chamados/services/feriado_reavaliar.py:24-56,65-86`, `app/ponto/services/cartorio.py:2059-2089`.
- **Outbox e outros escritores.** `salvar_e_emitir` insere `EventoCelula` quando a lavra grava `veredito` ou `ata`, inclusive mudança de precedência. Nos dois ramos de regeneração futura, apagar julgamento prévio pode emitir via `desfazer_julgamento_da_futura`, que preserva decisão de admin e retorna quando nada foi julgado. A reversão insere o evento explicitamente no atomic e depois usa o mesmo processamento posterior. O gerador de período só cria célula ou preenche DNA nulo passado, sem julgamento ou evento nessa função. `app/ponto/portas/celula.py:267-268,312-315,865-895,948-1001,1181-1207,1241-1252`, `app/ponto/services/cartorio.py:1898-1922,2034-2055,2094-2096`.
- **Chamado vivo.** O cron de minuto consome o outbox agrupado por colaborador, roda rejunção, retrata perguntas e chamados absolvidos, verifica premissa e órfãs, chama `fechar_pelo_marco_colab` e aciona a fábrica. A retratação por absolvição ignora veredito acusatório, classe que contém `cobrado`, e rejeita ata com marcos pendentes. No julgamento síncrono, o cartório também tem ramo explícito para marcos inexistentes. Verifica todos ausentes numa ata com lâmpadas e todos os marcos tipados, tenta `reconciliar`, só anota `marco_inexistente` e rejulga no sucesso. Um chamado de dia de trabalho com `marcos_acesos is not True` protesta, e sem códigos um vivo com fio que protesta pode produzir `cobrado`. `app/config/crons.py:393-394`, `app/chamados/reconciliador.py:1140-1153,1484-1523`, `app/escala/models.py:1778-1779`, `app/ponto/services/lastro.py:98-115`, `app/ponto/services/cartorio.py:1492-1495,1514-1538,1593-1610`.
- **Distinção entre porta e executor.** `lastro.fechar_pelo_marco_colab` pula ausência de lâmpada antes do gate. O gate `reconciliar` considera a premissa regularizada por `marcos_inexistentes` mediante o mesmo juiz do cartório, mas ainda passa pelas demais guardas, inclusive pergunta viva. A causa do chamado de 27/10 ter continuado vivo após o julgamento de 09/11 fica [nao verificado]. `app/ponto/services/lastro.py:170-181,391-403`, `app/chamados/models.py:683-712,1345-1367`.

## 24/11 10:28 · sessao 8540913dc18e · R1-REMEDE-2411 · sonda · origem=Code

No dado fornecido, `rc_sombra=0` é a única linha `rc_sombra`. A transcrição abaixo reproduz o dado fornecido linha a linha. A execução em produção está [nao verificado].

```text
sonda_leitura: script.py [e6495196abed8215] no cpuset 4-7, lendo sombra como leitor (LEITURA)

AUTORIDADE {'dia_sem_lavratura_no_espelho': 8808, 'dia_batida_impar': 435, 'dias_comparados': 7763, 'diverge_acima_60': 300, 'dono_BATIDA': 275, 'impar_que_DIVERGE': 280, 'impar_no_conjunto_do_dia': 276, 'impar_dentro_da_faixa': 240, 'bate_ate_10min': 7372, 'impar_FORA_da_faixa': 40, 'dono_ESTRUTURA': 37, 'dia_sem_trabalho_ambos': 151, 'dia_isento_ou_ausencia': 1683, 'e6_zero_esp_trabalho': 26, 'dono_CADASTRO': 79, 'esp_zero_e6_trabalho': 42, 'impar_que_BATE': 155, 'esp_desconta_janela_declarada': 288, 'diverge_10_60': 23, 'impar_SO_POR_CORTE_do_oraculo': 4}
AUTORIDADE erros no espelho: 0
AUTORIDADE BATE (ate 10 min): 95.0% de 7763 dias comparados
AUTORIDADE dos que divergem: 240 dentro da faixa piso..teto (indecidivel sem DNA), 40 FORA da faixa (contraditorio)
AUTORIDADE paridade de quem: 276 com contagem impar no conjunto do dia (falta OU sobra batida), 4 partidos SO pelo corte do proprio oraculo
AUTORIDADE ESTRUTURA    37 dia-colab     152.9 h   25 colabs   o sistema se contradiz ou calcula errado DADO o cadastro -- fila 1
AUTORIDADE CADASTRO     79 dia-colab     315.3 h   37 colabs   o DNA nao descreve as batidas -- lista do admin, nao codigo
AUTORIDADE BATIDA      275 dia-colab    1473.0 h  129 colabs   falta ou sobra batida -- lista do admin, nao codigo
AUTORIDADE SOMA        391   <- tem de ser igual a soma das classes (391)
AUTORIDADE colabs com divergencia: 164
CSV_POR_DONO {'BATIDA': 275, 'ESTRUTURA': 37, 'CADASTRO': 79} TOTAL 391
CSV_PARIDADE {('IMPAR', 'BATIDA'): 226, ('par', 'ESTRUTURA'): 36, ('par', 'CADASTRO'): 26, ('IMPAR', 'CADASTRO'): 53, ('par', 'BATIDA'): 49, ('IMPAR', 'ESTRUTURA'): 1}
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 4|37|2026-11-14|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|110|2026-11-12|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|112|2026-10-21|ESTRUTURA|546|838|838|292|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|112|2026-10-22|ESTRUTURA|541|835|835|294|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|113|2026-10-30|ESTRUTURA|242|0|0|-242|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|113|2026-11-01|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|114|2026-11-08|ESTRUTURA|439|429|499|-10|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|114|2026-11-15|ESTRUTURA|499|440|499|-59|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|130|2026-10-29|ESTRUTURA|335|672|730|337|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|200|2026-10-29|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|200|2026-11-02|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|218|2026-11-14|ESTRUTURA|103|0|0|-103|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|220|2026-11-10|ESTRUTURA|337|430|486|93|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-09|ESTRUTURA|551|597|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-10|ESTRUTURA|551|598|611|47|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-11|ESTRUTURA|550|598|611|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-13|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-17|ESTRUTURA|553|598|612|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-18|ESTRUTURA|550|596|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-19|ESTRUTURA|554|599|613|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|296|2026-11-02|ESTRUTURA|345|0|0|-345|e6_zero_esp_trabalho|par|0|trabalhou |n_batidas=0
ESTRUTURA 2|302|2026-11-20|ESTRUTURA|296|0|0|-296|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|334|2026-11-16|ESTRUTURA|209|591|683|382|diverge_acima_60|par|3|folga |n_batidas=4
ESTRUTURA 2|382|2026-11-10|ESTRUTURA|418|0|0|-418|e6_zero_esp_trabalho|par|3|trabalhou |n_batidas=0
ESTRUTURA 2|516|2026-10-23|ESTRUTURA|361|0|0|-361|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|727|2026-11-10|ESTRUTURA|307|0|0|-307|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|841|2026-11-16|ESTRUTURA|178|0|0|-178|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|874|2026-10-22|ESTRUTURA|667|383|383|-284|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 2|884|2026-11-08|ESTRUTURA|679|699|739|20|diverge_10_60|par|4|rejeitado |n_batidas=4
ESTRUTURA 3|909|2026-11-11|ESTRUTURA|831|245|374|-586|diverge_acima_60/FORA|IMPAR|CORTE_do_oraculo|em_aberto |n_batidas=4
ESTRUTURA 2|922|2026-11-01|ESTRUTURA|1382|68|68|-1314|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|922|2026-11-04|ESTRUTURA|676|458|458|-218|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 1|950|2026-11-11|ESTRUTURA|480|240|240|-240|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|989|2026-10-23|ESTRUTURA|721|567|567|-154|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|996|2026-10-28|ESTRUTURA|0|580|580|580|esp_zero_e6_trabalho|par|1|folga |n_batidas=2
ESTRUTURA 3|996|2026-11-10|ESTRUTURA|443|1026|1116|583|diverge_acima_60|par|5|em_aberto |n_batidas=6
ESTRUTURA 2|1044|2026-11-13|ESTRUTURA|204|135|339|-69|diverge_acima_60|par|4|em_aberto |n_batidas=4
FIM_LISTA
rc_sombra=0
```

## 24/11 10:21 · sessao 0bcf3a448c61 · R1-REMEDE-2411 · sonda · origem=Code

No dado fornecido, não há `rc_sombra` diferente de `0`. A execução original está [nao verificado].

```text
sonda_leitura: script.py [0d4e6e058a9c5469] no cpuset 4-7, lendo sombra como leitor (LEITURA)

AUTORIDADE {'dia_sem_lavratura_no_espelho': 8912, 'dias_comparados': 7521, 'bate_ate_10min': 7203, 'dia_isento_ou_ausencia': 1878, 'dia_batida_impar': 361, 'impar_que_BATE': 148, 'esp_zero_e6_trabalho': 35, 'dono_BATIDA': 217, 'diverge_acima_60': 235, 'impar_que_DIVERGE': 213, 'impar_no_conjunto_do_dia': 212, 'impar_FORA_da_faixa': 32, 'impar_dentro_da_faixa': 181, 'dono_CADASTRO': 75, 'e6_zero_esp_trabalho': 19, 'dia_sem_trabalho_ambos': 151, 'esp_desconta_janela_declarada': 258, 'dono_ESTRUTURA': 26, 'diverge_10_60': 29, 'impar_SO_POR_CORTE_do_oraculo': 1}
AUTORIDADE erros no espelho: 0
AUTORIDADE BATE (ate 10 min): 95.8% de 7521 dias comparados
AUTORIDADE dos que divergem: 181 dentro da faixa piso..teto (indecidivel sem DNA), 32 FORA da faixa (contraditorio)
AUTORIDADE paridade de quem: 212 com contagem impar no conjunto do dia (falta OU sobra batida), 1 partidos SO pelo corte do proprio oraculo
AUTORIDADE ESTRUTURA    26 dia-colab      88.4 h   16 colabs   o sistema se contradiz ou calcula errado DADO o cadastro -- fila 1
AUTORIDADE CADASTRO     75 dia-colab     295.0 h   31 colabs   o DNA nao descreve as batidas -- lista do admin, nao codigo
AUTORIDADE BATIDA      217 dia-colab    1021.0 h  117 colabs   falta ou sobra batida -- lista do admin, nao codigo
AUTORIDADE SOMA        318   <- tem de ser igual a soma das classes (318)
AUTORIDADE colabs com divergencia: 146
CSV_POR_DONO {'BATIDA': 217, 'CADASTRO': 75, 'ESTRUTURA': 26} TOTAL 318
CSV_PARIDADE {('par', 'BATIDA'): 43, ('IMPAR', 'BATIDA'): 174, ('IMPAR', 'CADASTRO'): 39, ('par', 'CADASTRO'): 36, ('par', 'ESTRUTURA'): 26}
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 4|37|2026-10-05|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 4|37|2026-10-09|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|129|2026-10-12|ESTRUTURA|0|779|779|779|esp_zero_e6_trabalho|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|218|2026-10-15|ESTRUTURA|105|0|0|-105|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|252|2026-09-23|ESTRUTURA|548|594|608|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-24|ESTRUTURA|552|598|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-25|ESTRUTURA|556|604|616|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-28|ESTRUTURA|535|581|595|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-01|ESTRUTURA|545|590|605|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-02|ESTRUTURA|545|593|606|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-06|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-09|ESTRUTURA|544|592|604|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|369|2026-10-12|ESTRUTURA|476|416|476|-60|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 2|382|2026-09-30|ESTRUTURA|364|0|0|-364|e6_zero_esp_trabalho|par|2|trabalhou |n_batidas=0
ESTRUTURA 2|451|2026-09-23|ESTRUTURA|707|718|718|11|diverge_10_60|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|451|2026-09-25|ESTRUTURA|705|718|718|13|diverge_10_60|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|478|2026-09-25|ESTRUTURA|719|8|8|-711|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|515|2026-10-09|ESTRUTURA|244|0|0|-244|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|515|2026-10-11|ESTRUTURA|245|0|0|-245|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|723|2026-10-08|ESTRUTURA|440|345|500|-95|diverge_acima_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|746|2026-09-24|ESTRUTURA|289|193|350|-96|diverge_acima_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|841|2026-10-20|ESTRUTURA|681|522|701|-159|diverge_acima_60|par|4|em_aberto |n_batidas=4
ESTRUTURA 2|854|2026-10-04|ESTRUTURA|718|7|7|-711|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 2|864|2026-10-12|ESTRUTURA|509|449|509|-60|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|868|2026-09-22|ESTRUTURA|0|679|733|679|esp_zero_e6_trabalho|par|1|em_aberto |n_batidas=4
ESTRUTURA 3|876|2026-09-21|ESTRUTURA|550|665|722|115|diverge_acima_60|par|1|em_aberto |n_batidas=4
FIM_LISTA
rc_sombra=0
```

## 24/11 09:45 · sessao 01fb331cff27 · RESOLVER-PASSA-NA-FRENTE · verificador · origem=Code

**Pré-verificação reprovada.** O calendário preserva o veto da pergunta viva respondida, mas o POST pode resolver o mesmo dia. A recusa por tranca funciona por dia na porta, mas um lote misto ainda encaminha os dias recusados à reconciliação. O histórico novo deixa de fora resoluções de competências que o calendário ainda permite resolver. `app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:506`, `app/ponto/views.py:3520`, `app/ponto/views.py:3533`, `app/chamados/services/pergunta_no_app.py:91`

Usei o diff fornecido e a cópia somente para leitura. Hash e base Git [nao verificado], pois a cópia não tem metadados Git. Execução dos testes, renderização no APK, sonda das 1.346 respondidas e impacto em produção [nao verificado]. O corte pede a sonda e o RED 3/3. `app/docs/CORTES.md:642`

## A. Escopo do aval

1. **Parcial.** O calendário ainda combina a face SQL de C21 com C6 e veta a oferta quando há pergunta viva respondida. A tarefa consulta essa mesma oferta. Já o POST valida permissão, data e veredito, e a porta acrescenta motivo e C20, sem consultar a oferta. Assim, aceita em princípio o dia vetado, inclusive o dia que os próprios testes novos resolvem com uma respondida viva. Também inexiste um veto geral para `nao_sei`: a tarefa pode publicar `resolver_dia` quando a oferta é verdadeira mesmo com a flag ligada, e a Folha prioriza a rota nesse caso. Os três testes **do 4b de 15/11** estão em `:132`, `:136` e `:143` e esperam ausência de porta e fio `nada`; verde [nao verificado]. `app/colaboradores/services/calendario.py:117`, `app/colaboradores/services/calendario.py:501`, `app/chamados/services/tarefa_do_dia.py:702`, `app/chamados/services/tarefa_do_dia.py:781`, `app/folha/fora_da_folha.py:86`, `app/ponto/views.py:3491`, `app/ponto/services/veredito_celula.py:251`, `app/ponto/tests/test_resolver_passa_na_frente.py:53`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:132`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:136`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:143`
2. **Parcial.** A API entrega `rotulo` no item histórico, deixa a encerrada fora de `respondidas` no fluxo normal e publica a nota quando o chamado transiciona. O calendário admite qualquer data anterior à competência corrente, enquanto o histórico só busca a anterior e a corrente. Uma resolução elegível mais antiga já nasce fora desse histórico. Também há um caminho em que a pergunta perde a via e o rótulo antes de recebê-los: a falta ou a batida aciona o cartório antes de `fechar_o_fio_do_dia`, e o fechamento só carimba quem **ainda** passa no C6 depois desses efeitos. Uma respondida cujo marco a própria batida satisfez pode cair nessa ordem. `app/api/views.py:2591`, `app/api/views.py:2595`, `app/api/views.py:1123`, `app/chamados/services/pergunta_no_app.py:91`, `app/colaboradores/services/calendario.py:117`, `app/ponto/services/veredito_celula.py:198`, `app/ponto/services/veredito_celula.py:365`, `app/ponto/services/veredito_celula.py:381`, `app/ponto/signals.py:132`, `app/ponto/signals.py:143`, `app/chamados/juizes.py:1516`
3. **Parcial.** Para um dia que C20 dá por trancado, com motivo preenchido, a porta recusa antes de escrever o veredito e devolve a causa ao envelope. Em lote misto, um único sucesso dispara `reconciliar_apos_vinculo(min(datas), max(datas))` sobre o intervalo original. Esse caminho reavalia a faixa inteira e pode atualizar ou apagar `FuroDiario` no dia recusado. O envelope registra cada recusa, mas o texto do lote mostra apenas as quatro primeiras. `app/ponto/services/veredito_celula.py:254`, `app/ponto/services/veredito_celula.py:258`, `app/ponto/views.py:3526`, `app/ponto/views.py:3533`, `app/ponto/views.py:3548`, `app/colaboradores/services/pos_vinculo.py:62`, `app/colaboradores/services/pos_vinculo.py:79`, `app/ponto/portas/celula.py:1292`, `app/core/respostas.py:54`
4. **Sim, nos dois sítios nomeados.** Ressuscitar exclui `resolver_dia` antes de simular. `merito_perdedor` reconhece a via em perdedora com resposta pelo C21, e o plano a retém como mérito. `materializacao.py:879` ficou fora do diff fornecido. `app/chamados/models.py:1553`, `app/chamados/services/fio.py:1394`, `app/chamados/services/fio.py:1453`, `app/chamados/services/materializacao.py:879`

**Além do literal dos quatro itens.** Motivo obrigatório e trilha assinada vêm da lei de 23/11. O diff também publica o **texto livre integral** do supervisor na nota do chamado, libera qualquer nota de sistema que contenha a frase fixa e acrescenta `"motivo": null` ao log dos chamadores internos, inclusive `declarar_dia` falta. O limite de 200 caracteres é só do HTML. Na porta, basta texto após `strip()`. A trilha da pergunta corta o texto para reservar espaço para a frase. `app/docs/CORTES.md:628`, `app/ponto/services/veredito_celula.py:166`, `app/ponto/services/veredito_celula.py:254`, `app/ponto/services/veredito_celula.py:396`, `app/ponto/services/veredito_celula.py:404`, `app/api/views.py:62`, `app/api/views.py:1128`, `app/ponto/services/declarar_dia.py:213`, `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_lote_barra.html:7`

## B. Escritores, leitores e semântica

| Campo ou valor | Quem escreve, quem lê e o que significa |
|---|---|
| `via_resolucao` e `VIA_RESOLVER_DIA` | O modelo declara a via. O Resolver seleciona perguntas do fato, chama `fechar_pelo_resolver` e persiste por `carimbar_identidade` → `_escrever_carimbo`. `admin_decidiu` continua com `fechar_por_admin`. O campo também tem portas preexistentes de carimbo comum, descarimbo individual/em lote, relavra de vias derivadas e troca pelo fato no espelho. O único produtor de `resolver_dia` localizado em produção passa pela porta de identidade. Os leitores diretos novos são a guarda de idempotência, a ressuscitação, `merito_perdedor`, o histórico e o rótulo. `app/ponto/services/veredito_celula.py:198`, `app/chamados/models.py:2786`, `app/chamados/models.py:3349`, `app/chamados/models.py:3365`, `app/chamados/models.py:3415`, `app/chamados/models.py:3517`, `app/chamados/models.py:3559`, `app/chamados/models.py:3645`, `app/chamados/models.py:3649`, `app/chamados/services/relavra_via.py:106`, `app/chamados/models.py:1559`, `app/chamados/services/fio.py:1406`, `app/chamados/services/pergunta_no_app.py:95` |
| `VIAS_QUE_VENCEM_A_RESPOSTA`, C6 | A classe define o conjunto. C6 lê via **com** carimbo, na projeção temporal, antes de olhar a resposta. `q_vivas` lê a mesma constante na face SQL. A via permanece na identidade do fato e C21 continua decidindo se existe resposta. Histórico e rótulo leem a via crua, sem conferir carimbo ou instante, embora a rota já tenha um `agora` da borda. Uma gravação entre as duas consultas pode deixar a mesma pergunta no retrato anterior e no histórico novo. `app/chamados/models.py:2794`, `app/chamados/models.py:2811`, `app/chamados/models.py:3761`, `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1494`, `app/chamados/services/pergunta_no_app.py:91`, `app/api/views.py:2555`, `app/api/views.py:2595` |
| `FRASE_RESOLVIDO_ANTES` e `rotulo` | A constante nasce no despachante e entra em `DecisaoPergunta.ato_motivo`, na nota do chamado e no JSON por `rotulo_do_app`. A nota é gravada por `nota.anexar_ocorrencia`. A API usa a frase também como autorização de visibilidade por substring. Há dois sentidos concretos: a frase afirma **antes da resposta**, mas o escritor a aplica à pergunta já respondida e conserva a resposta. A renderização pelo APK é [nao verificado]. `app/ponto/services/veredito_celula.py:162`, `app/ponto/services/veredito_celula.py:219`, `app/ponto/services/veredito_celula.py:229`, `app/ponto/services/veredito_celula.py:396`, `app/chamados/services/nota.py:75`, `app/chamados/services/pergunta_no_app.py:101`, `app/api/views.py:62`, `app/api/views.py:2591`, `app/chamados/models.py:4440`, `app/ponto/tests/test_resolver_passa_na_frente.py:142` |
| `dias_trancados` / C20 | C20 lê `PeriodoFechado` por K7 e a trilha posterior de Cobrar, Resolver ou desarquivar. Consome `(colaborador, dia do turno)`. O leitor avulso deriva esse dia da pergunta e exclui conversa. C6, a carga da tarefa, modelos, reconciliador, emissão e agora Resolver leem o mesmo juiz. Os escritores da tranca são `trancar_periodo` e `reabrir_periodo`. **Dia trancado por C20** pode ser falso em competência ainda trancada, quando houve toque administrativo anterior. `app/chamados/juizes.py:339`, `app/chamados/juizes.py:1023`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1130`, `app/chamados/juizes.py:1330`, `app/chamados/juizes.py:1502`, `app/chamados/models.py:2050`, `app/chamados/reconciliador.py:789`, `app/chamados/services/disputa_emissao.py:2102`, `app/chamados/services/tarefa_do_dia.py:550`, `app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1432` |
| `motivo` dos forms | O supervisor escreve texto de decisão. A view o passa a todos os dias selecionados, e o serviço usa a presença do parâmetro também para **identificar o ato**. `None` seleciona o caminho interno, string vazia produz recusa e texto aciona a via nova. O `motivo` de `recusas` é outro dado: a causa de erro devolvida pela porta. Cobrar usa outra rota e ignora o campo. `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_dia.html:28`, `app/templates/ponto/partials/_veredito_lote_barra.html:7`, `app/ponto/views.py:3523`, `app/ponto/views.py:3530`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/views_cobrar.py:337` |

A via é terminal para C6 enquanto carimbada. **Há uma porta preexistente que pode apagar esse carimbo.** Em disputa aberta, a recusa administrativa aceita uma pergunta respondida, não validada e ainda no fato. `resolver_dia` satisfaz essas condições e o descarimbo em lote só protege `VIAS_FORA_DO_FATO`. Assim, corrigir por um novo Resolver fica como regra incompleta. Esse caminho usa a porta existente, sem um segundo escritor novo da via. `app/docs/CORTES.md:628`, `app/chamados/models.py:2809`, `app/chamados/services/validacao.py:948`, `app/chamados/services/validacao.py:979`, `app/chamados/services/validacao.py:999`, `app/chamados/models.py:3645`

## C. Classes de reprovação e mordida dos testes [nao verificado]

- **Juiz, escritor e fonte única.** C6, C20 e C21 são reutilizados. A via entra nas listas com propósitos distintos e preserva a FK. As catracas verificam a lista de identidade, o chamador e a via que ele pode gravar. A divergência de autoridade é entre a oferta `_TT11` e a execução. A derivação paralela adicionada é usar texto livre da nota como identidade de evento para decidir visibilidade. `app/core/juizes.py:290`, `app/core/juizes.py:304`, `app/core/juizes.py:751`, `app/ponto/views.py:3520`, `app/chamados/tests/test_o261e_f2_fila.py:137`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:500`, `app/api/views.py:62`
- **Relógio.** A API injeta `agora`, mas o helper novo aceita omiti-lo e cai em `localdate()`. Mais importante, `fechar_pelo_resolver` lê `_tz.now()` por pergunta, embora o serviço já tenha `_agora_do_ato`. `SILENCIO` herda o instante do carimbo de cada pergunta. O teste congela o relógio e não distingue um instante do ato de várias leituras. `app/api/views.py:2556`, `app/chamados/services/pergunta_no_app.py:80`, `app/chamados/services/pergunta_no_app.py:91`, `app/ponto/services/veredito_celula.py:251`, `app/chamados/models.py:3373`, `app/ponto/services/veredito_celula.py:219`, `app/ponto/tests/test_resolver_passa_na_frente.py:24`
- **Trava e exceções.** A ordem do fio está correta no trecho novo: colaborador primeiro, C20 sob a trava, depois disputa e chamado antes do carimbo das perguntas. K7 faz leitura simples e a porta que grava `PeriodoFechado` não compartilha essa trava, portanto a leitura não serializa o Resolver contra um trancamento concorrente. A guarda também herda uma escrita lateral: em base sem usuário `sistema`, C20 pode criá-lo por `get_or_create`. A preparação do teste já chama C20 antes da fotografia. O diff não adiciona `except`. `app/ponto/services/veredito_celula.py:192`, `app/ponto/services/veredito_celula.py:258`, `app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1464`, `app/chamados/juizes.py:1316`, `app/core/utils.py:6`, `app/ponto/tests/test_resolver_passa_na_frente.py:216`
- **Mutantes, por inspeção.** **(1)** Os testes novos não mordem retirar o veto do calendário nem aceitar um POST de dia vetado. Os três cenários de FK mordem retirar a precedência da nova via da respondida, e o espelho cobre `declarar_dia` falta. **(2)** As asserções mordem sumir com o item, com `rotulo` ou com a nota no JSON. Não cobrem competência mais antiga, renderização, nem pergunta que perde vitalidade durante o veredito. **(3)** Mordem retirar C20 e omitir o motivo no avulso. O lote do teste é todo trancado, não confere mensagem de tela e não fotografa `FuroDiario`. **(4)** Mordem retirar a exclusão da ressuscitação e retirar a nova via do mérito. Execução dos mutantes [nao verificado]. `app/ponto/tests/test_resolver_passa_na_frente.py:60`, `app/ponto/tests/test_resolver_passa_na_frente.py:76`, `app/ponto/tests/test_resolver_passa_na_frente.py:91`, `app/ponto/tests/test_resolver_passa_na_frente.py:199`, `app/ponto/tests/test_resolver_passa_na_frente.py:223`, `app/ponto/tests/test_resolver_passa_na_frente.py:251`, `app/ponto/tests/test_resolver_passa_na_frente.py:273`, `app/ponto/tests/test_resolver_passa_na_frente.py:290`, `app/ponto/tests/test_resolver_passa_na_frente.py:309`, `app/ponto/tests/test_resolver_passa_na_frente.py:328`

## D. Migration e leitores fora do diff

**Sim, só de `choices`.** A comparação estática com a última definição anterior confirmou os mesmos atributos e todas as escolhas anteriores, com apenas `resolver_dia` ao final. A 0057 só declara `AlterField`; sua dependência 0056 contém um backfill próprio de outro campo. `app/chamados/migrations/0051_decisao_pergunta.py:109`, `app/chamados/migrations/0057_via_resolver_dia.py:9`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12`

**Há leitores com resultado diferente.** O juiz de lastro aceita a via nova como decisão suficiente, a relavra exclui o carimbo por identidade, e o card e o explicador usam o display da choice. O backfill `admin_decidiu_sem_ato` só audita a via antiga. Já `estado_aplicacao_pergunta` cai em `aplicada` para pergunta de hora respondida e carimbada pela nova via, mesmo sem aplicação da hora. Esse é o achado da obra separada citada no aval. `app/chamados/juizes.py:729`, `app/chamados/services/relavra_via.py:174`, `app/templates/chamados/partials/_card_atendimento.html:195`, `app/chamados/services/explicador.py:122`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:121`, `app/chamados/services/materializacao.py:879`, `app/chamados/services/materializacao.py:889`, `app/chamados/services/materializacao.py:900`

## E. Templates

**Avulso.** O campo obrigatório corresponde à porta. O texto promete encerramento imediato inclusive da respondida, mas a tela esconde o botão para a viva respondida e pode exibi-lo para dia que C20 vai recusar. “Cobrar este dia” está corretamente ligado à outra rota e dispensa a validação do motivo. Essa rota ainda pode registrar toque em dia trancado. `app/templates/ponto/partials/_veredito_dia.html:15`, `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_dia.html:28`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/colaboradores/services/calendario.py:777`, `app/ponto/services/veredito_celula.py:265`, `app/chamados/services/cobrar_dia.py:31`

**Lote.** Os três verbos e o motivo correspondem ao POST. A barra dá a entender que os dias marcados são acionáveis, mas a seleção usa a mesma oferta sem C20. Um dia selecionável pode ser recusado, e o intervalo da reconciliação ainda pode incluí-lo. `app/templates/ponto/partials/_veredito_lote_barra.html:2`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/ponto/views.py:3520`, `app/ponto/views.py:3533`

## F. Leis dos sítios

- **Viola LEIS-UI L2 e L4.** A oferta e a ação não compartilham a elegibilidade, e a tela oferece ação onde a tranca a impede. **Viola L7** na ajuda do avulso: a promessa de encerramento foi escrita no template e diverge das condições da porta. `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:39`, `app/docs/LEIS-UI.md:77`, `app/templates/ponto/partials/_veredito_dia.html:8`, `app/colaboradores/services/calendario.py:97`, `app/ponto/services/veredito_celula.py:265`
- **Viola a disciplina de relógio da L-128** na leitura nova por pergunta. **Cumpre C21/L-130** ao reconhecer resposta no mérito e preservar o par no ato de silêncio. **Cumpre o desenho de identidade** do corte de 01:10 ao usar a via no degrau existente, sem escrever a FK. A garantia de correção posterior pelo próprio Resolver fica incompleta pela recusa administrativa descrita em B. `app/docs/PLANO-PADROES.md:97`, `app/chamados/models.py:3373`, `app/docs/LEIS.md:151`, `app/chamados/services/fio.py:1400`, `app/chamados/models.py:4440`, `app/docs/CORTES.md:628`, `app/docs/CORTES.md:642`, `app/chamados/juizes.py:1509`, `app/chamados/services/validacao.py:999`
- **L-050, L-051, L-102 e L-104.** A guarda nova usa C20 e preserva a exceção de toque anterior da L-051, sem restaurar fronteira de exportação da L-050. O hunk do calendário só comenta o veto, sem alterar a palavra do dia sem par da L-102 ou o recorte civil do holerite da L-104. O corte posterior da tela pela lavra segue como outra obra. Pouso dos pré-requisitos F0 e E4C-F5 e efeito sobre exportadas em produção [nao verificado]. `app/docs/LEIS.md:91`, `app/docs/LEIS.md:92`, `app/docs/LEIS.md:128`, `app/docs/LEIS.md:130`, `app/chamados/juizes.py:1130`, `app/colaboradores/services/calendario.py:484`, `app/docs/CORTES.md:628`, `app/docs/CORTES.md:639`, `app/docs/CORTES.md:642`

## 24/11 09:22 · sessao e77d23685b46 · A-TELA-DO-DIA-LE-UM-JUIZ · censo · origem=Code

### 1. Escritores do chamado de furo

**Há quatro caminhos que podem gravar `modulo_origem='batida_ausente'` na cópia.** Três emitem a cobrança automática do dia com categoria `Ausencia`, subcategoria `Batida nao realizada` e área `supervisao`. O quarto é a abertura web pelo colaborador, que pode gravar o mesmo módulo sem associar a solicitação ao dia do furo. O hash `32e4bd52a` é o informado no pedido. A cópia disponível veio sem metadados Git para confirmá-lo. `[nao verificado]` `app/ponto/management/commands/detectar_ausencias.py:694` `app/chamados/reconciliador.py:957` `app/chamados/reconciliador.py:1032` `app/ponto/management/commands/reconciliar_grade.py:507` `app/chamados/views.py:167` `app/chamados/services/abertura.py:47`

| Caminho de criação | Quem chama e o que grava |
|---|---|
| `detectar_ausencias.Command._abrir_chamado` | O `handle` do detector chama quando falta entrada. `emitir_furo_retroativo --apply` chama para `FURO_SEM_COBRANCA`. Ambos usam a entrada prevista `E`, com `{tipo, hora, motivo}` em `contexto_json.marcos_faltantes` quando o motivo é mapeável. `contexto_json.horario_previsto` recebe o instante ISO da âncora. O cartório também chama o mesmo método em dois ramos. No `FURO_PARCIAL`, passa todos os marcos apagados que conseguiu nomear, em ordem temporal, e usa o primeiro como âncora. No ramo de furo integral, cobrança morta ou limbo de hoje, pode cair no padrão de entrada. `app/ponto/management/commands/detectar_ausencias.py:424` `app/ponto/management/commands/detectar_ausencias.py:476` `app/ponto/management/commands/detectar_ausencias.py:694` `app/ponto/management/commands/emitir_furo_retroativo.py:116` `app/ponto/management/commands/emitir_furo_retroativo.py:158` `app/ponto/services/cartorio.py:1850` `app/ponto/services/cartorio.py:1879` `app/ponto/services/cartorio.py:1931` `app/ponto/services/cartorio.py:2032` |
| `chamados.reconciliador.reemitir_chamado_do_dia`, ramo **c2** | `reatar_pergunta_orfa` pode chegar ao c2 pelo signal de batida ou pela fábrica, quando esta cria uma pergunta do dia sem destino. O c2 grava `data_turno`, `origem='reconciliador_c2'` e, se houver, todos os apagados em `contexto_json.marcos_faltantes`, com `motivo='lampada_apagada'` e `_marco_por='lampada_da_celula'`. Lista vazia ou falha ao nomear permite que o contexto saia sem o campo. `app/ponto/signals.py:207` `app/chamados/reconciliador.py:1568` `app/chamados/reconciliador.py:1662` `app/chamados/reconciliador.py:1755` `app/chamados/services/disputa_emissao.py:468` `app/chamados/reconciliador.py:811` `app/chamados/reconciliador.py:1032` `app/ponto/services/lastro.py:127` |
| `reconciliar_grade.Command._emitir_furo` | O `handle` chama com `--apply` e sem `--so-retratar`. Grava os marcos `missing` da grade que venceram e têm motivo mapeável em `contexto_json.marcos_faltantes`, com `{tipo, hora, motivo}`, `data_turno` e `origem='grade'`. A entrada do cron na cópia roda em sombra, sem `--apply`. `app/ponto/management/commands/reconciliar_grade.py:66` `app/ponto/management/commands/reconciliar_grade.py:151` `app/ponto/management/commands/reconciliar_grade.py:250` `app/ponto/management/commands/reconciliar_grade.py:507` `app/config/crons.py:462` |
| `chamados.views.abrir_chamado` → `abrir_chamado_local` | O POST web aceita `batida_ausente` por ele constar em `MOTIVOS`. A categoria vem da opção válida do formulário ou do classificador, que usa o texto. Se o módulo for preservado, `ChamadoColaborador.criar` grava a solicitação sem `marcos_faltantes`, `horario_previsto` ou `data_turno`. A abertura nativa da API usa `app_colaborador`. `app/chamados/views.py:167` `app/chamados/views.py:196` `app/chamados/catalogo/modulos.py:105` `app/chamados/services/abertura.py:28` `app/chamados/services/abertura.py:47` `app/chamados/classificador.py:277` `app/api/views.py:2189` |

Os três caminhos automáticos convergem em `ChamadoColaborador.abrir`, com deduplicação por colaborador, módulo e `data_turno`. Um existente encerrado pode passar pela porta de renascimento. O lote do cartório, o julgamento pontual, **Cobrar este dia** e o passe `cobrar_furos_sem_cobranca` chegam ao mesmo emissor do cartório. `app/chamados/models.py:968` `app/chamados/models.py:1037` `app/ponto/management/commands/processar_cartorio.py:36` `app/ponto/management/commands/processar_cartorio.py:65` `app/ponto/services/cartorio.py:2143` `app/chamados/services/cobrar_dia.py:41` `app/ponto/services/furos_sem_cobranca.py:117`

No vocabulário mais amplo de furo, `intervalo_pendente` tem emissão automática desativada no detector de intervalo. `turno_aberto` nasce em `processar_alertas_turno`, com categoria `turno_aberto`, subcategoria `saida_pendente` e `contexto_json.entrada_timestamp`. `entrada_bloqueada_turno_aberto` nasce no POST de batida web, com categoria `turno_aberto`, subcategoria `entrada_bloqueada` e `contexto_json.{pendentes,total}`. Os passes de saída de 24h usam módulo e categoria `turno_aberto_24h`, com `data_turno` e `entrada_timestamp`. Esses são fluxos de turno ou intervalo distintos da cobrança diária da tabela. `app/chamados/catalogo/furo_modulos.py:15` `app/ponto/management/commands/detectar_intervalo_ausente.py:150` `app/ponto/management/commands/processar_alertas_turno.py:335` `app/ponto/views.py:294` `app/ponto/views.py:310` `app/ponto/management/commands/processar_alertas_avancados.py:120` `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:128` `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`

### 2. Leitores do marco gravado e efeito de uma lâmpada acesa

| Leitor | Efeito |
|---|---|
| **Lastro** | `_marcos_cobrados` lê todos os pares hora/tipo de `contexto_json.marcos_faltantes`. `marcos_acesos` dá `True` quando todos acenderam, `False` ao encontrar uma apagada e `None` quando falta identificação suficiente. `fechar_pelo_marco_colab` só tenta fechar com `True`, via `reconciliar`, e só grava a nota se fechar. `fechar_cobranca_com_lastro` mede e alarma. `app/ponto/services/lastro.py:62` `app/ponto/services/lastro.py:166` `app/ponto/services/lastro.py:401` `app/ponto/management/commands/fechar_cobranca_com_lastro.py:49` |
| **`reconciliar` e reabertura** | `premissa_regularizada` considera regularizado o chamado quando todos os marcos nomeados acenderam, mesmo que outro marco do dia continue apagado. `reconciliar` ainda passa pelo motor e pela guarda da pergunta viva. Na reabertura, premissa regularizada barra o ato. Se o emissor pediu renascimento por outros marcos, a porta examina os marcos **do pedido**, e a cobrança pode reabrir pelo marco novo ainda apagado. `app/chamados/models.py:683` `app/chamados/models.py:804` `app/chamados/models.py:988` `app/chamados/models.py:1352` `app/chamados/models.py:1414` `app/ponto/management/commands/detectar_ausencias.py:744` |
| **Cartório** | `_protesta` exclui da acusação de cobrança o chamado cujos marcos já acenderam na ata em construção. Em `apply`, tenta reconciliá-lo. Se houver fio vivo entre os chamados que deixaram de protestar, adia o lote até lavrar e rodar o carimbo das perguntas absolvidas. Um fecho pode provocar nova passada e cobrança pelo marco que continua apagado. `app/ponto/services/cartorio.py:1546` `app/ponto/services/cartorio.py:1623` `app/ponto/services/cartorio.py:1047` `app/ponto/services/cartorio.py:1073` `app/ponto/services/cartorio.py:1931` |
| **Signals e registro de batida** | Os signals de batida acionam cartório e reconciliador. O signal da pergunta alterada acorda o evento da célula, cujo consumidor chama `fechar_pelo_marco_colab`. São gatilhos. O caminho rápido do detector e as duas versões da API de batida sincronizada só tentam o fecho se `marcos_acesos_do_dia` der `True`. A batida web também pode tentar reconciliar pela guarda de premissa do chamado. `app/ponto/signals.py:131` `app/ponto/signals.py:206` `app/chamados/signals.py:252` `app/chamados/reconciliador.py:1515` `app/ponto/management/commands/detectar_ausencias.py:396` `app/api/views.py:794` `app/api/views_core.py:788` `app/ponto/services/entrada_sincronizada.py:19` `app/ponto/services/triagem_batida.py:589` |
| **Perguntas, telas e API de chamados** | `entrada_de_marco` lê o campo para identificar o marco da pergunta por motivo, com fallback por tipo só no legado. Materialização e validação o usam para tipo, hora ou comparação de resposta. O fio web usa a hora para exibir a previsão. Já o formulário de declaração do card web e `api_chamados_v2` projetam os faltantes atuais da célula. Um marco aceso sai dessa projeção, sem transição de chamado pela leitura. O espelho de gestor usa o campo gravado para o badge de chamado vivo, e o calendário indexa chamado vivo por célula ou dia. `app/chamados/juizes.py:588` `app/chamados/juizes.py:2039` `app/chamados/juizes.py:2166` `app/chamados/services/materializacao.py:567` `app/chamados/services/materializacao.py:706` `app/chamados/services/fio.py:451` `app/chamados/services/fio.py:495` `app/chamados/services/declaracao_texto.py:129` `app/api/views.py:1078` `app/escala/utils.py:1058` `app/ponto/services/espelho.py:615` `app/colaboradores/services/calendario.py:456` |
| **Pauta** | A Pauta de discordantes lê `veredito='discordante'` e a porta publicada pela tarefa do dia. Se o dia ganha fio ou muda de veredito e sai do alvo, pode marcar a **Pauta** como feita. A leitura não fecha nem reabre o chamado pelo marco gravado. `transicionar_com_pauta`, usado pelo lastro, executa o fecho delegado e entrega ao DP eventuais recusas S143 do ato. `app/ponto/services/pautas_dos_discordantes.py:40` `app/ponto/services/pautas_dos_discordantes.py:75` `app/ponto/services/pautas_dos_discordantes.py:195` `app/folha/fora_da_folha.py:70` `app/chamados/services/fecho_runtime.py:146` |

Há também leitores de manutenção do campo. `nomear_marco_por_lampada` preenche o acervo sem marco a partir de apagados. `reconciliar_grade` pode atualizar os marcos de uma cobrança órfã em `--apply`, sujeito à guarda do fio respondido e vivo. A fila de regeneração usa o campo para identificar e exibir cobranças afetadas por reescrita. A emissão de disputa verifica se o chamado passado nomeia marcos e então lê os faltantes da ata para formular perguntas. `app/chamados/management/commands/nomear_marco_por_lampada.py:61` `app/chamados/management/commands/nomear_marco_por_lampada.py:105` `app/ponto/management/commands/reconciliar_grade.py:442` `app/ponto/management/commands/reconciliar_grade.py:461` `app/escala/services/regeneracao.py:141` `app/escala/services/regeneracao.py:186` `app/chamados/services/disputa_emissao.py:744` `app/chamados/services/disputa_emissao.py:1180`

### 3. Predicado de fechamento e efeito sobre um eventual marco “sem par”

O fecho reativo do lastro exige um `batida_ausente` na fila, dia identificável, célula gravada e `marcos_acesos(ch, cel) is True`. O casamento é por **hora e tipo** para todos os marcos nomeados. A seguir, `reconciliar()` precisa aceitar o fecho. Já o vigia `julgar` tenta casar uma batida do mesmo tipo dentro da tolerância quando falta a lâmpada correspondente e usa `reconciliaria()` para classificar o resultado, sem fechar. `app/ponto/services/lastro.py:82` `app/ponto/services/lastro.py:183` `app/ponto/services/lastro.py:284` `app/ponto/services/lastro.py:312` `app/ponto/services/lastro.py:329` `app/ponto/services/lastro.py:401` `app/chamados/models.py:661`

**O efeito de criar um chamado para a sobra depende de como ela for gravada.**

- Se a sobra for gravada como um par hora/tipo que coincide com uma lâmpada já acesa, o lastro pode fechar a cobrança na primeira avaliação, se o gate permitir. O predicado não consulta se aquela batida ainda está sem par. `app/ponto/services/lastro.py:183` `app/ponto/services/lastro.py:407` `app/chamados/models.py:702`
- Se o marco for gravado com hora e tipo próprios, ausentes de uma ata com lâmpadas, o fecho por **marco aceso** não entra. Mas `marcos_inexistentes` pode dar `True`. O cartório tenta reconciliar por esse outro caminho no julgamento, e `premissa_regularizada` aceita essa condição. Um marco sintético ausente da ata, portanto, também pode ser encerrado. `app/ponto/services/lastro.py:98` `app/ponto/services/lastro.py:187` `app/ponto/services/cartorio.py:1651` `app/chamados/models.py:1363`
- Se a sobra estiver apenas em outro campo, sem marco conferível em `marcos_faltantes`, o caminho por marco do lastro não fecha. Na cópia não há formato específico de cobrança da sobra nesses predicados. `app/ponto/services/lastro.py:62` `app/ponto/services/lastro.py:110` `app/ponto/services/lastro.py:183` `app/ponto/services/lastro.py:286`

### 4. Retenção em `folha/export.py`

- **`_dias_sem_decisao`** tem dois chamadores diretos. `motivos_retencao_celula` devolve os motivos e, opcionalmente, os dias retidos. `informativo_rescisao` usa os dias para informar o DP sobre desligados. A retenção chega a `classificar_export` por `motivos_retencao_espelho` e marca `fora / furo_espelho`. Prévia, Fora da Folha e vigia da pausa também consomem a lista de dias pelo mesmo ponto. `app/folha/export.py:1220` `app/folha/export.py:1229` `app/folha/export.py:1316` `app/folha/export.py:968` `app/folha/previa.py:170` `app/folha/fora_da_folha.py:120` `app/folha/services/vigia_pausa.py:125`
- **`nao_retem_pela_pausa`** é lido por `_dias_sem_decisao` e pelo vigia da pausa. Exige o conjunto exato `{'FURO_PARCIAL'}`, pelo menos um papel apagado e todos os apagados pertencentes à pausa. Ata desalinhada, em aberto ou indeterminada falha no predicado. `app/folha/export.py:1148` `app/folha/export.py:1173` `app/folha/export.py:1308` `app/folha/services/vigia_pausa.py:62` `app/folha/services/vigia_pausa.py:127` `app/folha/services/vigia_pausa.py:157`
- **`nao_retem_sem_chamado`** é lido por `_dias_sem_decisao`. Exige o conjunto exato `{'FURO_PARCIAL'}` e ata com pelo menos uma lâmpada, todas com `acesa is True`. Ata vazia, lâmpada desconhecida ou protesto misto não ganham essa dispensa. `app/folha/export.py:1176` `app/folha/export.py:1308` `app/ponto/services/lastro.py:138`
- **A existência ou vitalidade do chamado não é condição da retenção.** O ramo do veredito acusador usa os protestos e as duas dispensas. Assim, `FURO_PARCIAL` puro com tudo aceso é dispensado mesmo se houver chamado vivo. Com saída apagada, por exemplo, o dia pode reter sem chamado algum. O teste de retenção cria só a célula e exige `espelho_furo` para esse caso. `app/folha/export.py:1260` `app/folha/export.py:1286` `app/folha/export.py:1308` `app/folha/tests/test_a_tela_furo_sem_marco_nao_retem.py:52` `app/folha/tests/test_a_tela_furo_sem_marco_nao_retem.py:61`

### 5. Detecção e gravação da batida sem par

O pareador entrega um `Turno` aberto se há entrada e falta saída. Uma saída órfã é um `Turno` com saída e sem entrada. `sem_par_do_pareador` reduz esses objetos a dois booleanos por `data_turno`. O cartório carrega os turnos da mesma janela usada por `dias_encerrados`, só passa o sinal quando o turno do dia está encerrado e aplica o horizonte de competência `2026-11` em diante. `app/ponto/turnos.py:234` `app/ponto/turnos.py:312` `app/ponto/turnos.py:1495` `app/ponto/supra_juiz.py:86` `app/ponto/services/cartorio.py:316` `app/ponto/services/cartorio.py:333` `app/ponto/services/cartorio.py:1208` `app/ponto/services/cartorio.py:1223`

`supra_juiz.classificar_dia` gera `FURO_PARCIAL` no ramo sem marco `missing` quando o dia de trabalho encerrou, há previsto, o déficit supera 10 minutos e há `E` sem par ou `S` órfã com realizado positivo. Também exige âncora, ausência aprovada que não esteja cobrindo esse ramo e dia sem justificativa. Sem vínculo, o ramo gera `PREVISTO_COM_DIA_INDEFINIDO`. O cartório passa os sinais, conserva apenas os códigos, chama `lavrar_veredito` e persiste `CelulaDia.protestos` e a ata pelo carimbo. `app/ponto/supra_juiz.py:253` `app/ponto/supra_juiz.py:507` `app/ponto/motor_calculo_v2.py:51` `app/ponto/services/cartorio.py:1553` `app/ponto/services/cartorio.py:1953` `app/ponto/portas/celula.py:1104` `app/ponto/portas/celula.py:1126` `app/ponto/portas/celula.py:1235`

**O `pk` e o instante completo estão disponíveis no objeto `Batida` durante o pareamento.** O caminho do protesto perde essa identidade ao reduzir os turnos a `(E, S)` booleanos. A impressão guarda um hash que inclui as batidas da janela e o sinal `E`, `S` ou `ES`. `ata.orfas` guarda horários `HH:MM` das sobras do casamento da grade, sem `pk` e sem vínculo explícito com a batida que produziu o sinal do pareador. O protesto lavrado, sozinho, não identifica a sobra por `pk` ou instante. `app/ponto/turnos.py:43` `app/ponto/turnos.py:312` `app/ponto/turnos.py:1868` `app/ponto/models.py:23` `app/ponto/supra_juiz.py:99` `app/ponto/services/cartorio.py:522` `app/ponto/services/cartorio.py:601` `app/ponto/services/cartorio.py:1233` `app/ponto/services/cartorio.py:785` `app/escala/utils.py:673` `app/ponto/services/cartorio.py:1553`

### 6. `col348` e os guardas que deixam furo apagado sem chamado

Os 21 dias são a premissa do item 6. O anexo fornecido não traz as linhas por dia, o vínculo, `CelulaDia.trabalha`, a ata ou o resultado dos vetos. Qual guarda ocorreu em cada um dos 21 dias fica `[nao verificado]`.

**O guarda de escala que basta por si só é `CelulaDia.trabalha=False`.** O vínculo lê esse valor, o detector pula em `if not resultado_dia`, o cartório retorna `None` em `_marcos_do_furo_parcial` porque exige `eh_dia_trabalho(...) is True`, e o funil ainda tem `VETO_CELULA_FOLGA` se for chamado. O regime intermitente responde `False` no cálculo do template, enquanto a grade tem um caminho próprio para dia intermitente com batida. Essa combinação pode produzir dia com fato e marco posicional apagado sem passar pelo emissor. A folha retém pelo veredito e pelas exceções do laço, sem exigir `trabalha=True` no ramo acusador. `app/escala/models.py:820` `app/escala/models.py:1098` `app/escala/models.py:1330` `app/escala/utils.py:1614` `app/escala/utils.py:1784` `app/ponto/management/commands/detectar_ausencias.py:306` `app/ponto/services/cartorio.py:2006` `app/ponto/marcos.py:155` `app/folha/export.py:1286`

Os demais guardas relevantes estão nestes pontos:

- **No `handle` do detector**, o universo é de colaboradores em operação, com rollout e vínculo vigente hoje. O laço pula isento, data fora de hoje/ontem, dia sem vínculo ou sem previsão de trabalho, sem início previsto, sem marco vencido ou com turno já encerrado. Também pula ausência que cobre, substituição aplicável, entrada encontrada para o dia-turno, chamado já existente pela chave do dia e `--dry-run`. Em particular, uma entrada existente basta para o detector não abrir chamado por saída ou pausa faltante. `app/colaboradores/queries.py:50` `app/ponto/management/commands/detectar_ausencias.py:97` `app/ponto/management/commands/detectar_ausencias.py:137` `app/ponto/management/commands/detectar_ausencias.py:306` `app/ponto/management/commands/detectar_ausencias.py:337` `app/ponto/management/commands/detectar_ausencias.py:347` `app/ponto/management/commands/detectar_ausencias.py:364` `app/ponto/management/commands/detectar_ausencias.py:396` `app/ponto/management/commands/detectar_ausencias.py:424` `app/ponto/management/commands/detectar_ausencias.py:461`
- **No cartório**, pode não haver nova avaliação, por exemplo com impressão igual sem `forcar`. No ramo parcial, o portão exige o protesto, colaborador não desligado, piso de competência ou toque, ausência de afastamento agora, marco nomeável, modo `apply`, nenhum chamado vivo do dia e ausência da condição `parcial_so_lista`. Esta última cobre parcial sem período ou parcial já julgada sem fato novo. Mesmo com lâmpada apagada, a extração pode ficar vazia se faltar vínculo, se o vínculo não disser trabalho, ou se hora/tipo não tiverem instante na cadeia e motivo mapeável. Lâmpada pendente também fica fora. `app/ponto/services/cartorio.py:1491` `app/ponto/services/cartorio.py:1830` `app/ponto/services/cartorio.py:1916` `app/ponto/services/cartorio.py:465` `app/ponto/services/cartori

[truncado em 20000 de 26687 caracteres]

## 24/11 08:38 · sessao e6153ad48c0f · CLASSE-D-NA-FRENTE-DA-E4D · censo · origem=Code

No HEAD `7a4e79eefb7bd2613e4826449b0b22a43401cfb5`, o **C5 ainda emite um item por colaborador**, sem `data` e sem `dono_do_dia`. O alcance do vigia ainda usa `empresas_visiveis()`, e o juiz `dono_da_divergencia` ainda não recebe `FATO_EM_AUSENCIA`. `app/escala/services/cadastro_realidade.py:176-182,1079` · `app/chamados/services/vigia_fio.py:517-520` · `app/ponto/management/commands/e6_oraculo.py:53-54,98-110`

Comparei a cópia com os dois objetos Git e consultei o histórico no repositório de origem, cujo `HEAD` coincide com o hash pedido. As contagens e os casos individuais em produção ficam `[nao verificado]`.

## 1. Citações das seções 1, 3 e 4

O prefixo `app/` está implícito nas duas tabelas. Em “mudou?”, a comparação é com o código de `77dfa94fa`.

| citacao_antiga | hoje | mudou? |
|---|---|---|
| `escala/services/cadastro_realidade.py:78,137-173,176-182` (inclui `:180`) | `escala/services/cadastro_realidade.py:78,137-173,176-182` | **Não.** O coletor traz datas por colaborador. O formatador C5 emite um item por colaborador e põe até três datas no texto. |
| `escala/services/cadastro_realidade.py:342-355,358-376,383-397` | `escala/services/cadastro_realidade.py:342-355,358-376,383-397` | **Não.** C9 e C10 mantêm os itens datados. C12 mantém um item por publicação do motor, inclusive com `data=None` se a origem não trouxer dia. |
| `escala/services/cadastro_realidade.py:414` | `escala/services/cadastro_realidade.py:414-530`, em especial `:497-510` | **Sim.** Além do caminho por âncora terminal, o C11 aceita M1, com todas as vivas respondidas no horizonte e sem âncora. Os dois caminhos exigem folga não definida. |
| `escala/services/cadastro_realidade.py:450-453` | `escala/services/cadastro_realidade.py:452-455` | **Não.** A consulta mantém a seleção do colaborador ativo, veredito `indefinida`, protesto e janela. |
| `escala/services/cadastro_realidade.py:469` | `escala/services/cadastro_realidade.py:470-473` | **Sim, pelo retorno consultado.** O `if` é idêntico, mas agora também exclui o envelope de leitura desconhecida sem rota. `chamados/services/tarefa_do_dia.py:809-822` |
| `escala/services/cadastro_realidade.py:503-507,512` | `escala/services/cadastro_realidade.py:513-517,522` | **Não.** A montagem do dono continua com `sem_motor=True` e CxR lavrado. O item continua gravando o ISO da célula e o retorno de `.dono(d)`. |
| `escala/services/cadastro_realidade.py:671` | `escala/services/cadastro_realidade.py:682-684` | **Não.** A ação C5 ainda aponta a `TELA_COLAB` para adesão ou lançamento de ausência. |
| `escala/services/cadastro_realidade.py:768` | `escala/services/cadastro_realidade.py:860-868,1059-1073` | **Sim.** `lista()` passou a publicar também `linhas` ordenadas pela carga e `carga_resumo`. `postos` segue no retorno. |
| `escala/services/cadastro_realidade.py:857-863,866` | `escala/services/cadastro_realidade.py:956-962,965` | **Não.** A inclusão de C5 segue sob `try/except`, antes da chamada a `um_dono_por_dia`. |
| `escala/services/cadastro_realidade.py:885-916` (inclui `:887-904,910-912`) | `escala/services/cadastro_realidade.py:996-1027` (ramos `:998-1015`, contador `:1021-1023`) | **Não.** Só C9–C12 preenchem `_extra['dono']`. Incluir C5 apenas no conjunto faria o acesso em `:1023` levantar `KeyError` fora do `try` do produtor. |
| `escala/services/cadastro_realidade.py:941-947` | `escala/services/cadastro_realidade.py:1052-1058` | **Não.** `colabs` conta IDs distintos e `ocorrencias` conta itens incluídos. Para o C5 atual, os números coincidem. `escala/services/cadastro_realidade.py:180,1025-1027` |
| `escala/services/cadastro_realidade.py:963,966-1011` | `escala/services/cadastro_realidade.py:1079,1082-1127` | **Não.** Mesmo conjunto C9–C12 e mesma reconciliação de donos divergentes por dia. |
| `escala/services/cadastro_realidade.py:1014-1022,1025-1045` (inclui `:1042`) | `escala/services/cadastro_realidade.py:1130-1138,1141-1161` (guarda em `:1158`) | **Não.** A projeção exige `data` preenchida. O nível do juiz tem precedência. Conflito no nível escolhido dá `None`. |
| `chamados/services/vigia_fio.py:188-255` (inclui `:222-228,230-236,248-251`) | `chamados/services/vigia_fio.py:166-233` (métodos em `:200-206,208-214,226-229`) | **Não.** A classe conserva as consultas separadas de dia, colaborador e dono da mesa. |
| `chamados/services/vigia_fio.py:274-315` (inclui `:313`) | `chamados/services/vigia_fio.py:240-263` (`fio_do_admin` em `:261-262`) | **Sim.** O ramo `decisao_do_dia_pendente` saiu da função. `Resolver dia` passou a ser uma porta publicada pela tarefa. `chamados/services/tarefa_do_dia.py:781-790` |
| `chamados/services/vigia_fio.py:542,592` | `chamados/services/vigia_fio.py:487,537` | **Sim.** O alcance hoje testa a `rota` publicada, contabiliza `porta_acao` e passa o `hoje` da rodada a `dias_que_seguram`. `chamados/services/vigia_fio.py:595-609` |
| `chamados/services/vigia_fio.py:621,654` | `chamados/services/vigia_fio.py:566,598` | **Não.** `ps` continua vindo das vivas da célula em `alim['vivas']`. |
| `chamados/services/vigia_fio.py:696,700-703` | `chamados/services/vigia_fio.py:640-647` | **Não.** Os predicados de d e de `dia_na_lista_dono_*` são os mesmos. A porta e a causa c, avaliadas antes, mudaram. `chamados/services/vigia_fio.py:595-609,635-639` |
| `chamados/services/vigia_fio.py:715,737` | `chamados/services/vigia_fio.py:659,681` (`ROTULO_CAUSA` em `:483-484`) | **Não.** Mesmo incremento por causa e emissão de seu rótulo. |
| `chamados/management/commands/vigiar_fio_morto.py:175,181` | `chamados/management/commands/vigiar_fio_morto.py:184,190` | **Não.** O comando ainda passa a mesma `ListaDoAdmin` e o mesmo instante aos dois censos. |
| `chamados/management/commands/backfill_fio_morto_e4b.py:78` | `chamados/management/commands/backfill_fio_morto_e4b.py:78-79` | **Não.** `_alcance` ainda chama o censo real e seleciona as linhas `sem_porta`. |
| `chamados/management/commands/disparar_perguntas_competencia.py:56` | `chamados/management/commands/disparar_perguntas_competencia.py:56-57,68-71` | **Não.** O contador desdobra o dict em pares `(cid, dia)`. |
| `folha/vigia_retido_calado.py:43` | `folha/vigia_retido_calado.py:54-58,71-80` | **Sim.** Agora chama com os IDs visíveis intersectados com o recorte, sem a guarda antiga de existência de retrato. Tanto antes quanto hoje, `set(dict)` usa as **chaves de colaborador**. |
| `chamados/services/pergunta_no_app.py:35` | `chamados/services/pergunta_no_app.py:35` | **Não.** O motivo `celula_nunca_bateu` continua apontando a lista C5 do admin. |
| `chamados/services/tarefa_do_dia.py:638,648` | `chamados/services/tarefa_do_dia.py:809-822,825-827` | **Sim** para `porta_da_tarefa`, que inclui ações diretas e envelope de desconhecimento sem rota. **Não** para o delegador `carga_da_tarefa`. |
| `chamados/juizes.py:1595` | `chamados/juizes.py:1723,1748-1761` | **Sim** na carga. Passou a trazer decisões em lote. Continua publicando as vivas pelo juiz. |
| `folha/fora_da_folha.py:109` | `folha/fora_da_folha.py:125-140,165` | **Sim.** `dias_que_seguram` exige `hoje` da borda para a leitura dos dias. A seleção dos retidos ainda chama `classificar_export` em `:147`. |
| `ponto/management/commands/e6_oraculo.py:98-110,113-195` (inclui `:182-195,193-194`) | `ponto/management/commands/e6_oraculo.py:98-110,113-195` | **Não.** Mesmas precedências e mesmo `None` para o residual que dependeria de CxR quando `sem_motor=True` e a lavra falta. |
| `ponto/motor_calculo_v2.py:1623,1997,1528` | `ponto/motor_calculo_v2.py:1623,1997,1528` | **Não.** Os dois primeiros publicam dia do turno. `:1528` continua tomando `entrada.date()`. |
| `test_MORDE_F5_queries_nao_crescem_com_os_dias`, `escala/tests/test_f3_beco1_folga_na_lista.py:351` | `escala/tests/test_f3_beco1_folga_na_lista.py:395` | **Não** quanto ao contrato medido pelo teste, que segue sendo custo por número de dias do C11. |

## 2. Escritores e leitores no HEAD

| Dado | Escritores e leitores |
|---|---|
| `item['data']` | **Escrevem** C9 `escala/services/cadastro_realidade.py:350`, C10 `:395`, C11 `:522` e C12 `:374`. **Leem** o encaminhamento C11 `:1010` → `:714-719`, `um_dono_por_dia` `:1100-1101` e `dias_na_lista` `:1158-1160`. C11 usa `CelulaDia.data`; C9 publica `data_turno`, e C12 copia o dia publicado pelo motor, quando presente. `ponto/services/teto_na_lista.py:68-79` · `ponto/services/cxr_na_lista.py:84-103` |
| `item['dono_do_dia']` | **Escrevem** C9 `escala/services/cadastro_realidade.py:351`, C10 `:396`, C11 `:522` e C12 `:375`. **Reescreve** `um_dono_por_dia` em `:1125`, quando há divergência. Ele lê os candidatos em `:1104-1108`. O laço lê o dono para ação e exibição em `:998-1023`. `dias_na_lista` lê o valor cru em `:1160`. Tela e PDF exibem o `dono` formatado. `templates/escala/cadastro_x_realidade.html:31` · `escala/services/cadastro_realidade.py:1203-1206` |
| `dias_na_lista` | **Produz** `{(cid, ISO): dono\|None}` em `escala/services/cadastro_realidade.py:1141-1161`. O leitor de produção é `ListaDoAdmin._carregar`, `chamados/services/vigia_fio.py:189-196`. |
| `ListaDoAdmin` | **Preenche** `_dias` e `_colabs` em `chamados/services/vigia_fio.py:183-198` e registra dias sem resposta em `:200-233`. **Instanciam** o comando `chamados/management/commands/vigiar_fio_morto.py:184` e os dois censos quando chamados sem instância, `chamados/services/vigia_fio.py:337,580`. **Leem** `dono_fora_do_fio` em `:259-262`, as causas d/e em `:622,640-647` e o comando, que lê `sem_resposta` em `chamados/management/commands/vigiar_fio_morto.py:203`. |
| `censo_alcance`, d e e | **Escreve** d para célula sem viva, com `dia_na_lista == (False, None)` e colaborador presente, depois das exclusões anteriores. **Escreve** e para `sem_celula` ou o residual, incluindo dia datado sem viva com dono `ESTRUTURA` ou `None`. Incrementa `alcance_d/e`, `alcance_e_detalhe` e publica a linha em `chamados/services/vigia_fio.py:616-662`. **Leem** `linhas_alcance` em `:666-684`, o comando que imprime e carimba o agregado em `chamados/management/commands/vigiar_fio_morto.py:190,215,284` e o backfill que recolhe as linhas `sem_porta` em `chamados/management/commands/backfill_fio_morto_e4b.py:76-79,313-326`. A chave do balde no alcance é `alcance_fio_do_admin`. `chamados/services/vigia_fio.py:481-482,526-527,616-621` |
| `colabs_nunca_bateu` | **Produz** `{cid: [date, ...]}` a partir de `CelulaDia.veredito='nunca_bateu'`, da janela anterior até ontem, excluindo desligados. **Leem** `_nunca_bateu`, o contador de disparo por par e o vigia da Folha por colaborador. `escala/services/cadastro_realidade.py:137-182` · `ponto/services/cartorio.py:1317-1321` · `chamados/management/commands/disparar_perguntas_competencia.py:52-71` · `folha/vigia_retido_calado.py:71-80` |
| `DONO_PELO_JUIZ` | **Define** `frozenset({'C9','C10','C11','C12'})` em `escala/services/cadastro_realidade.py:1079`. **Leem** o contador `por_dono` em `:1021-1023`, a seleção para reconciliação em `:1100-1101` e os dois níveis de `_dono_resolvido` em `:1135-1138`. |

**`Empresa.em_rollout`.** Leitura direta no formulário `templates/colaboradores/empresa_form.html:34`, nos recortes `colaboradores/views.py:2630` e `chamados/services/painel.py:411`, no queryset `colaboradores/queries.py:65` e nas consultas `escala/management/commands/apurar_furos_diarios.py:9`, `ponto/services/furos_sem_cobranca.py:59` e `relatorios/management/commands/selo_leitores_no_mesmo_numero.py:51`. A borda do cadastro recebe a caixa e encaminha o valor nas operações de atualização e criação. `colaboradores/models.py:45` · `colaboradores/views.py:513,539-542,550-556`

**`empresas_visiveis()`.** O helper filtra `ativa=True` e exclui CNPJ iniciado por `11222333`. Chamadas de produção encontradas por arquivo. `colaboradores/queries.py:78-87`

- **Vigias, Folha e backfill.** `chamados/services/vigia_fio.py:191,517` · `chamados/management/commands/backfill_fio_morto_e4b.py:183,301` · `folha/vigia_retido_calado.py:54` · `folha/retrato_folha.py:1411` · `folha/views_retrato.py:149` · `folha/management/commands/lavrar_retrato_folha.py:51` · `folha/views.py:36,89,462,493`
- **Cadastro, escala, chamados e ponto.** `colaboradores/views.py:124,499,716,737,1002,1074,1124,1263,1787,1901,2221,2739,2977` · `escala/views.py:492,524` · `escala/views_plano_folgas.py:37` · `chamados/services/painel.py:438,544` · `chamados/views.py:763,1492` · `ponto/views.py:598,668,807,933,1130,1775`
- **Demais chamadas.** `api/views_mensageria.py:1222` · `comunicados/views.py:122` · `core/services/painel_op.py:66` · `core/views.py:767` · `core/views_config.py:399,486` · `core/views_usuarios.py:118` · `ferias/views.py:50` · `holerite/views.py:57,799` · `inteligencia/views.py:61` · `relatorios/views.py:43,118,169,238,326,377,426,589,671,1165,1499,1501,1556,1608` · `relatorios/views_pend.py:15`

## 3. Complementos e estado da cura

| Complemento | Sítios do HEAD e o que já existe |
|---|---|
| **CORTE-LU2-07-EMPRESA-1** | O sítio do recorte é `chamados/services/vigia_fio.py:517-520`, antes da iteração por empresa e competência em `:534-543`. A autoridade já existe em `colaboradores/models.py:45` e `templates/colaboradores/empresa_form.html:34-35`. **O recorte ainda não usa o campo.** Essas linhas do vigia vêm de `fc9beecea`, anterior à base. O mesmo `censo_alcance` é chamado pelo backfill em `chamados/management/commands/backfill_fio_morto_e4b.py:76-79`. A lista do admin tem seu próprio cruzamento em `chamados/services/vigia_fio.py:189-196`. Alterar o helper global alcançaria a lavra da Folha em `folha/management/commands/lavrar_retrato_folha.py:51`. O registro separado de `EMPRESAS=(2,3,4)` corresponde a `config/crons.py:17`. O anexo documenta 46 como subconjunto de 80. A recontagem no HEAD é `[nao verificado]`. |
| **1711-1005 item 6, `SEM_LEITURA`** | A cadeia a conferir para col265/col407 é `chamados/models.py:4377-4409` → `chamados/services/tarefa_do_dia.py:430-434,154-155,340-347` → `chamados/management/commands/backfill_fio_morto_e4b.py:98-100,158-161`. O backfill **já tem** a escrita pela porta `registrar`, anterior à base (`1250dbd98`), e `registrar` persiste a sequência em `chamados/models.py:4551-4576`. Desde a base, `d948d2ab5` e `20f5a0338` acrescentaram transporte e contagem de sem leitura. As testemunhas atuais são `chamados/services/vigia_fio.py:599-602,683`, `folha/fora_da_folha.py:215-238`, `folha/retrato_folha.py:1244` e `folha/vigia_retido_calado.py:84-100,115-117`. O anexo identifica a origem e tira col838 do recorte histórico. A linha faltante, a causa histórica da lacuna, o estado atual dos dois casos e qual alteração adicional seria necessária ficam `[nao verificado]`. |
| **1711-1050 item 6, `FATO_EM_AUSENCIA`** | Os dois sítios de implementação são **o juiz e seus insumos**, `ponto/management/commands/e6_oraculo.py:53-54,98-110,182-195`, e **o produtor e encaminhamento diário**, `escala/services/cadastro_realidade.py:928-965,996-1027,1079-1127`. A referência antiga `:1047` cai hoje na linha em branco `:1163`, entre `dias_na_lista` e `pdf_bytes`. A classificação já existe em `ponto/supra_juiz.py:395-418,450-457`, com mapeamento em `escala/models.py:1622-1631`. A tarefa já lê esse veredito numa faixa específica e emite `VALIDAR` para admin sem âncora nessa faixa. Esse ramo antecede a base e a porta é resolvida separadamente. `chamados/services/tarefa_do_dia.py:358-370,751-756`. **A entrega do item 6 ainda falta:** o juiz segue sem esse insumo e o laço da lista segue sem esse produtor. A edição de ausência e a retratação já têm portas em `ponto/views.py:2930-2937,2973,3453-3462`; a retratação anota e preserva a batida em `ponto/registro_batida.py:287-302`. |

Os leitores do juiz a cruzar para `FATO_EM_AUSENCIA` são `ponto/services/teto_na_lista.py:72,86`, `ponto/services/cxr_na_lista.py:99-103`, `ponto/management/commands/censo_fase_12x36.py:280-289`, `ferias/management/commands/listar_ferias_com_batida.py:53-57` e o próprio comando `ponto/management/commands/e6_oraculo.py:328,378`. `pautas_discordantes` tem uma chamada **direta** ao juiz para decidir se precisa do motor, além da chamada final via `.dono()`. `ponto/management/commands/pautas_discordantes.py:100-106,118`

## 4. Commits desde `77dfa94fa`

Hashes na ordem do `git log --oneline 77dfa94fa..HEAD -- <arquivo>`, consultado por caminho. Os números de linha identificam os sítios acima.

| Arquivo no HEAD | Commits | [nao verificado]
|---|---|
| `escala/services/cadastro_realidade.py:176,956,996,1079` | `a81f888dc` `d0210a400` `550c810b6` `4536fe161` `106920782` `a5ff3627f` `d948d2ab5` |
| `chamados/services/vigia_fio.py:517,640` | `57582ee14` `a65c4b712` `b9acb6828` `96d16af7d` `20f5a0338` `d948d2ab5` `c5a1bf90c` `b8681db0e` `f39d24d58` |
| `ponto/management/commands/e6_oraculo.py:53,182` | **Nenhum.** |
| `chamados/management/commands/backfill_fio_morto_e4b.py:98,161` | `ec46c1d4f` `67f5606ee` `568c14c0a` `b240b2f52` `57582ee14` |
| `chamados/services/tarefa_do_dia.py:154,363,433` | `eaf69fe33` `0dc6512a8` `c983ccc90` `0ee6521f5` `8542d4e72` `dae3bfca0` `96d16af7d` `57c1c9b94` `efcf3937a` `20f5a0338` `af1df2fe8` `a5ff3627f` `d948d2ab5` `517b6f16c` `c5a1bf90c` `b8681db0e` `f39d24d58` |
| `chamados/models.py:4377,4551` | `81c096c32` `4346eddae` `b240b2f52` `a1b5eafbe` `928c5cad1` `dfffac9b1` `131f1f6cf` `55f6689a0` `af495e9ab` `38da8c04d` `55d268b05` `05cb513bb` `c2601465f` `c9a67ed52` `f1e9c487e` `23e80acde` `d34dbfc84` `a3c23d10b` `9d386d1b1` `acdcd556d` `0db6afc73` `18bf86f01` `9500f8063` `41620774b` `5d9e1b0e4` `f54d7d15a` `dae3bfca0` `afda61fbd` `21f1e9e49` `e1386f13c` `a65c4b712` `b9acb6828` `f90ead1e0` `fbac30539` `0340fc1ca` `1fcb5d0b6` `d0cf621ac` |
| `templates/escala/cadastro_x_realidade.html:20,31` | `cff719f7d` `550c810b6` `106920782` |
| `escala/tests/test_f3_beco1_folga_na_lista.py:395` | `57c1c9b94` `efcf3937a` `6fb74c6d2` `714a92a61` `a5ff3627f` `d948d2ab5` `c5a1bf90c` `b8681db0e` |
| `escala/tests/test_cadastro_x_realidade.py:53,70` | **Nenhum.** |
| `chamados/tests/test_o261e_f5_alcance.py:260,308` | `a65c4b712` `efcf3937a` `20f5a0338` `714a92a61` `b8681db0e` `f39d24d58` |
| `chamados/tests/test_e4_backfill.py:151-162` | `c5a1bf90c` |

Entre os leitores do juiz citados no complemento, houve commits em `ponto/management/commands/pautas_discordantes.py:100-118` (`57c1c9b94`, `20f5a0338`, `a5ff3627f`) e `ponto/management/commands/censo_fase_12x36.py:280-289` (`0dd5f8323`). O histórico por caminho ficou vazio para `ponto/services/teto_na_lista.py:72`, `ponto/services/cxr_na_lista.py:99`, `ferias/management/commands/listar_ferias_com_batida.py:53` e a fonte de classificação `ponto/supra_juiz.py:395-418`.

## 5. Contra-exemplos

- **Algum produtor já emite C5 por dia? Não.** A busca de C5 no produtor encontra o cadastro da assinatura, `_nunca_bateu`, a ação e a ligação em `lista()`. O único ponto de emissão é `out[cid] = [{...}]`. Os pares produzidos pelo contador de disparo vêm do dict de datas e ficam no contador. `escala/services/cadastro_realidade.py:78,176-182,682-684,956-962` · `chamados/management/commands/disparar_perguntas_competencia.py:56-71`
- **Algum leitor de `por_ass['C5']['ocorrencias']` em tela ou PDF supõe unidade COLAB? Não encontrei leitor desse campo nesses dois caminhos.** A etiqueta da tela lê explicitamente `por_assinatura[*].colabs`. Na linha, tela e PDF leem primeiro `detalhe`, com fallback para `ocorrencias` **do item**. O C5 atual tem `detalhe`, e `lista()` atribui `ocorrencias=1` a cada assinatura de cadastro. A previsão do brief de mudança no número exibido na tela e no PDF não se confirma para o agregado `ocorrencias` no HEAD. `templates/escala/cadastro_x_realidade.html:20,31` · `escala/services/cadastro_realidade.py:180-181,1018-1020,1052-1058,1197-1206`

## 24/11 04:07 · sessao 0d2157c46e78 · RESOLVER-PASSA-NA-FRENTE · verificador · origem=Code

**Reprovado.** O Resolver pode lavrar o dia e deixar uma pergunta viva. Quando consegue encerrá-la, a pergunta some do histórico do app e a API normalmente oculta a única nota que diz “Resolvido pelo supervisor”. `app/ponto/services/veredito_celula.py:198–223,381–399` · `app/chamados/juizes.py:1502–1528` · `app/chamados/services/pergunta_no_app.py:45–92` · `app/api/views.py:62–65,1118–1127`

**PROCEDENTE** indica falha confirmada por leitura. A revisão usa o diff fornecido e os arquivos da cópia. Identidade Git da cópia, execução dos testes, tela nativa e incidência em produção [nao verificado]. [nao verificado]

1. **PROCEDENTE — escritores e leitores.** `fechar_pelo_resolver` chama `carimbar_identidade`, que grava `via_resolucao`, `materializada_em` e `materializada_por`. `fechar_o_fio_do_dia` registra `SILENCIO` por `DecisaoPergunta.registrar`, com autor, instante do carimbo, resposta preservada e `ato_motivo`. O motivo mora em **`DecisaoPergunta`**. A célula recebe `veredito` e `veredito_via='admin'` pela porta `lavrar_veredito`. `app/chamados/models.py:3360–3368,3410–3414,3526–3542,4334,4575–4586` · `app/ponto/services/veredito_celula.py:160–170,214–223,389–390` · `app/ponto/portas/celula.py:1128–1139`

   C6 e `q_vivas` conhecem a via pelo conjunto que vence a resposta. O fio filtra as disputas fechadas por essas autoridades e ainda pode mostrar a pergunta de uma disputa aberta. O card usa o rótulo das choices e o modal lê autor, data e motivo da decisão. O auditor conta por valor de via, o juiz do lastro aceita a identidade sem exigir batida, e a relavra sem lastro poupa a via. A célula é lida pelo C6 e pela marca de decisão humana do calendário. `app/chamados/juizes.py:729–742,1494–1537` · `app/chamados/models.py:3746–3771,4183–4188` · `app/chamados/services/fio.py:296–322` · `app/templates/chamados/partials/_card_atendimento.html:195` · `app/templates/chamados/partials/modal_fio.html:212` · `app/chamados/management/commands/auditar_invariantes_chamados.py:197–209,435` · `app/chamados/services/relavra_via.py:174–184` · `app/colaboradores/services/calendario.py:446,762` · `app/ponto/services/veredito_celula.py:105–125`

   **Dois leitores falham.** A testemunha de aplicação do fio chama de `aplicada` uma resposta de hora carimbada por `resolver_dia`, inclusive se o veredito foi `faltou` e nenhuma batida foi plantada. E `merito_perdedor` protege exclusivamente `admin_decidiu` com resposta. Se uma `resolver_dia` respondida chega como perdedora e os demais vetos liberam, o plano a manda para retratação e a porta troca sua via para `duplicata`. O backfill de `admin_decidiu` sem ato também é nominal, mas o fluxo normal do Resolver já grava seu próprio ato. `app/chamados/services/materializacao.py:879–904` · `app/chamados/services/fio.py:460–466,1394–1403,1446–1456,1665–1691` · `app/ponto/services/veredito_celula.py:365–374` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:129,360–361`

2. **PROCEDENTE — o carimbo inicial é único, mas pode ser desfeito pelo sistema.** O diff acrescenta um chamador de `carimbar_identidade` para a via nova e nenhuma escrita da FK `celula`. Reuso e rechaveamento já tinham suas próprias escritas dessa FK. Para dois Resolvers serializados pela trava do colaborador, mantendo-se o carimbo, o segundo fica fora de `q_vivas`. A guarda do método também devolve `False` sobre a instância atualizada, e a linha de decisão só nasce quando ele grava. `app/chamados/models.py:3248–3256,3360–3368,3526–3542,3754–3769` · `app/chamados/services/fio.py:1189–1198` · `app/ponto/services/veredito_celula.py:194–223,258–261`

   A ressuscitação existente pode selecionar uma pergunta **sem resposta** do Resolver. Ela exclui só `VIAS_FORA_DO_FATO`, limpa o carimbo na cópia antes de perguntar ao C6 e, se a cópia ficar viva, grava `REABERTURA` e limpa o carimbo real pelo lote. O cartório tem caminho para chamar essa porta. Assim, a terminalidade da via no C6 vale enquanto ela está gravada. `app/chamados/models.py:1548–1555,1575–1599,1676–1678,1731–1740,3639–3642` · `app/ponto/services/cartorio.py:1690–1721`

3. **PROCEDENTE — a transação é a mesma, mas o filtro roda na hora errada.** O serviço pega a trava do colaborador no `atomic`, fecha o fio e só então lavra a célula. Uma exceção propagada no registro da decisão desfaz as escritas do dia. O caso sem célula fecha a pergunta sem lavra de célula, conforme o corte. Uma recusa booleana do carimbo, porém, pula a decisão e ainda acrescenta “pergunta fechada” às ações. `app/ponto/services/veredito_celula.py:194–225,258–261,377–400` · `app/chamados/models.py:3528–3532,4575–4586`

   - **Célula lavrada, pergunta viva.** Em competência trancada, com célula e pergunta respondida sem FK, o C6 a exclui antes da lavra. A posterior lavra humana ou a auditoria do Resolver vale como toque e tira a tranca daquele dia. Na leitura seguinte, a pergunta sem FK pode voltar viva pelo degrau da resposta, sem carimbo `resolver_dia`. Com FK de outro dia, o mesmo vale se aquela célula não tiver decisão administrativa nem o marco concordante. A view aceita esse toque em dia trancado. `app/ponto/services/veredito_celula.py:198–214,381–399` · `app/ponto/views.py:3516–3523` · `app/chamados/juizes.py:1130–1139,1373–1396,1502–1528`
   - **Pergunta encerrada sem a trilha do Resolver.** Batidas e ausências são gravadas antes da seleção. Seus sinais chamam o cartório na hora. Se a nova lavra já absolveu uma pergunta muda com FK, `q_vivas` pode excluí-la antes da assinatura do supervisor. Se nenhuma pergunta chega à assinatura, a auditoria do dia leva o motivo, mas omite a frase fixa e usa uma porta que pode falhar sem reverter o ato. `app/ponto/services/veredito_celula.py:198–223,347–399` · `app/ponto/signals.py:85–93,131–147` · `app/ponto/services/cartorio.py:1015–1033` · `app/chamados/models.py:3764–3771` · `app/core/observ.py:94–105,124–139`

4. **NÃO PROCEDENTE — o motivo e a via nova não vazaram para `declarar_dia`, validação ou cobrança.** Falta delega sem `motivo_do_resolver`; folga fecha o fio sem esse argumento; a validação de folga continua usando `fechar_por_admin`. O botão de cobrança usa `formnovalidate` e a view lê colaborador, data e volta. **Há uma lacuna distinta:** o POST de validação no fio reconsulta C6 só para disputa fechada. Se ainda passar pela âncora de uma disputa aberta, pode validar a resposta preservada pelo Resolver. No aceite de folga, a validação pode inclusive chamar `fechar_por_admin` e substituir a via nova. `app/ponto/services/declarar_dia.py:205–218` · `app/chamados/services/validacao.py:547–563,587–596,755–787,810–819` · `app/chamados/services/fio.py:750–760` · `app/templates/ponto/partials/_veredito_dia.html:28` · `app/chamados/views_cobrar.py:324–344`

5. **PROCEDENTE — a oferta tem uma autoridade, mas a paridade tem limites.** Saiu o veto da pergunta viva **respondida**. Continuam obrigatórios dia passado, anterior à competência corrente e status entre `falta`, `irregular`, `aberto` e `sem_certidao`. O plural lê o campo publicado pelo mesmo calendário. Em caso de falha na autoridade de dia em aberto, a tela publica `None`; o plural com `agora` já pode ter respondido `False` para a competência corrente, e a função individual chamada diretamente devolve `bool`. `app/colaboradores/services/calendario.py:94–104,117–168,485–501,604–614,763,798–802`

   `ponto/views.py:3600–3609` lê o campo de **feriado vira trabalho**. O POST do Resolver já aceitava datas sem consultar a oferta, inclusive o toque explícito em trancada. O calendário também não usa K6 como veto, e o aval de exportada mantém a correção humana por essa porta. Há um caso de oferta sem decisão pendente no sentido literal de TT11: uma falta já lavrada pelo admin continua com status `falta`, que habilita o botão em competência anterior. A repetição da falta pode então ser recusada por sobreposição. Esse caso decorre do predicado de status que já existia. `app/ponto/views.py:3491–3523` · `app/docs/CORTES.md:354,462` · `app/core/juizes.py:737,751` · `app/ponto/status_dia.py:63–65` · `app/ponto/portas/celula.py:1045–1054` · `app/ponto/services/ausencia.py:451–471`

6. **NÃO PROCEDENTE — motivo vazio passa pela recusa antes de efeito de negócio.** Avulso e lote têm campo obrigatório e fazem POST na mesma view. Ela manda `''` quando o campo falta. O serviço recusa vazio e espaços antes do `atomic`; a view registra a recusa por data e só reconcilia se houver sucesso. O outro chamador de produção encontrado é `declarar_dia` para falta, que usa a entrada interna sem motivo. Não apareceu outro chamador de Resolver por cron, API ou command no censo da cópia. `app/templates/ponto/partials/_veredito_dia.html:18–26` · `app/templates/ponto/partials/_veredito_lote_barra.html:2–7` · `app/ponto/views.py:3513–3533` · `app/ponto/services/veredito_celula.py:238–260` · `app/ponto/services/declarar_dia.py:213–218`

7. **PROCEDENTE — o app perde a pergunta e normalmente esconde o texto.** Pendências, `historico` e `respondidas` vêm do universo vivo. Após o carimbo, a pergunta sai dos três. O JSON da pergunta não inclui `via_resolucao` nem `ato_motivo`. A mensagem só é anexada se houver chamado vivo do dia que o gate consiga reconciliar. Sai como nota `sistema`, e a API só publica notas de sistema cujo conteúdo tenha a palavra `justificativa`. Portanto até um chamado reconciliado e visível normalmente fica sem a frase prometida. `app/chamados/services/pergunta_no_app.py:45–92` · `app/api/views.py:62–65,1118–1127,2553–2556,2566–2604` · `app/ponto/services/veredito_celula.py:147–156,229–234,381–388` · `app/chamados/services/nota.py:75–89`

   A saída disponível no código é a abertura genérica pelo endpoint `chamados/abrir/`, que recebe assunto e descrição, sem vínculo específico com a pergunta ou a data resolvida. A tela web “Meu atendimento” também oferece “Nova solicitação”. A posição dessa ação no app nativo [nao verificado]. `app/api/urls.py:87` · `app/api/views.py:2141–2165,2189–2202` · `app/templates/chamados/meu_atendimento.html:48`

**7b. PROCEDENTE — a nova rota muda o que a Folha e os vigias concluem.** A carga consulta a oferta para dias sem rota e, com `True`, publica `admin / porta_acao / resolver_dia`. Uma âncora terminal continua sem modal de validação, mas pode ganhar uma porta real no calendário. A tranca, isenção, fora do vínculo e marco ainda não vencido ficam fora dessa consulta. O veto K6 da tarefa incide nas exposições administrativas da E2, sem alcançar o fallback do Resolver, que segue a oferta do calendário. Uma ausência com status `ausencia` segue fora da oferta. Uma ausência aprovada com veredito `fato_em_ausencia` pode ter status `irregular`, de modo que a dispensa do marco pela tarefa não impede a oferta do calendário. Incidência nesses grupos [nao verificado]. `app/chamados/services/tarefa_do_dia.py:74–77,273–278,315–324,358–370,702–717,751–790` · `app/ponto/status_dia.py:67–70,139–162` · `app/colaboradores/services/calendario.py:94–104`

- **Folha:** o universo retido vem do export. Dentro dele, muda o fio, pode mudar o grupo da pessoa para A e aparece o link “Resolver dia no calendário” com a data. Um dia que tinha `nao_sei` pode sumir da lista `dias_sem_leitura` mesmo com o booleano preservado no detalhe. `app/folha/fora_da_folha.py:86–122,197–247,275–295` · `app/folha/retrato_folha.py:1015–1020,1053–1059`
- **Vigia e furo sem canal:** uma rota passa a contar como fio vivo e alcance do admin. O dia sai dos baldes sem rota e de `sem_canal`; o `nao_sei` original ainda é contado. `porta_acao` por si não dispara o alarme do vigia. `app/chamados/services/vigia_fio.py:359–405,593–609,633–659` · `app/chamados/services/furo_sem_canal.py:96–119` · `app/chamados/management/commands/vigiar_fio_morto.py:85–94,210–211,302–303`
- **Pauta e beco:** a pauta descarta o dia com fio admin. Se ele era `nao_sei`, pode perder a proteção que mantinha aberta uma pauta já existente, embora a tarefa ainda carregue o desconhecimento. O beco C11 descarta o dia quando existe envelope de porta, inclusive o envelope anterior de desconhecimento. `app/ponto/services/pautas_dos_discordantes.py:86–97,194–213` · `app/escala/services/cadastro_realidade.py:469–474`

8. **PROCEDENTE — relógio e cobertura dos REDs.** A via nova entra no degrau 0b já existente, o SQL lê o mesmo conjunto e o selo de identidade foi atualizado. O diff fornecido não introduz outro juiz nem um novo `except`. O `now()` novo é lido **sob a trava**, portanto não antecipa o carimbo à aquisição. Ainda assim, a leitura fica dentro do método chamado para **cada pergunta**, separada do instante capturado pelo despachante. Carimbo e `SILENCIO` da mesma pergunta coincidem; perguntas do mesmo ato e a lavra podem ter instantes distintos. Há também a divergência de significado de `materializada_em` na testemunha do item 1. Na sequência, `ato_motivo` continua sendo texto de trilha. `app/chamados/juizes.py:1494–1510` · `app/chamados/models.py:2789–2806,3360–3368,3746–3769,4334–4355` · `app/ponto/services/veredito_celula.py:210–223,251,389–390` · `app/ponto/portas/celula.py:1128–1134` · `app/chamados/tests/test_o261e_f2_fila.py:106–149`

   Por inspeção, os REDs das três amarrações mordem a sobrevivência no C6. Os de motivo e trilha têm asserções específicas. O teste de idempotência chama a guarda diretamente, mas o segundo POST de falta pode ser recusado por sobreposição, então ele não prova dois Resolvers bem-sucedidos. O teste do app **exige o desaparecimento** e passa sem testar a mensagem. O RED do calendário morde o retorno do veto via `pode_ultimo_recurso`; a paridade aceita `False == False` e não prova botão presente. Tranca após o toque, sinal do cartório antes da assinatura, ressuscitação e os efeitos nos leitores de 7b ficam sem prova nesses testes. Execução e resultado contra mutantes [nao verificado]. `app/ponto/tests/test_resolver_passa_na_frente.py:80–105,116–182` · `app/ponto/services/ausencia.py:451–471` · `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:65–87`

9. **NÃO PROCEDENTE — a migration está conforme.** A 0057 depende da 0056, a última anterior na cópia, e tem uma única operação `AlterField` em `PerguntaDisputa.via_resolucao`. A comparação com a última definição do campo na cadeia confirmou os mesmos parâmetros e choices anteriores, com apenas `resolver_dia` acrescentada. `app/chamados/migrations/0051_decisao_pergunta.py:109–112` · `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:17–27` · `app/chamados/migrations/0057_via_resolver_dia.py:8–18`

## 24/11 02:40 · sessao 8353229698a2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

Nova execução [nao verificado]. O executor grava arquivos. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/1f80df53084f4f5c8ea6abae96379375674459d4/bin/sonda_leitura.sh:126)

Saída fornecida [nao verificado]

```text
25
sonda_leitura: sonda_divergencia.py [284cfa46e4b59aba] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/wt-janela-turno-020503/app (copia), banco segue sombra como leitor

TOT diverge 25
TOT diverge_com_decisaohe 13
TOT diverge_janela_ligada 25
TOT pares_com_chave 23948
TOT sem_dia_do_juiz 107
rc_sombra=0
```

## 24/11 02:31 · sessao 98532e6761ac · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · verificador · origem=Code

**BLOQUEIA.** A nova âncora corrige a volta noturna antes do `hf`, mas cria um par invertido quando a volta e a saída já ocorreram depois do `hf`. Na geometria do selo B-I28, a janela da cópia devolveu **07:32 → 07:00 e 43 min fora**, para **11 min brutos**, em execução isolada em memória. A catraca do dia do par também falhou em duas verificações. `app/ponto/tests/test_o280_bateria_da_lei.py:5007,5081` · `app/ponto/motor_calculo_v2.py:1574,1775` · `app/ponto/janela_he.py:168` · `app/core/tests/test_medida_turno_do_par.py:544,560`

## 1. Escritores, leitores e chamadores

- **OK — Memo e chave.** Há três escritas em `_dia_jornada_do_par`: inicialização vazia a cada `calcular_mes`, gravação no laço principal e gravação no crédito do turno aberto. O único leitor é `_janela_do_dia`. Nos dois caminhos, o motor grava `localizar(entrada)` e repassa a mesma entrada a `calcular_periodo`. As três implementações que chamam `_janela_do_par` e a segunda leitura de `Motor12x36ComEscala` usam a mesma `localizar`, que converte ao fuso configurado e preserva segundos e microssegundos. O clipe vem depois da busca. Não encontrei caminho normal com a entrada ajustada, truncada ou em outro fuso usada como chave. `app/ponto/motor_calculo_v2.py:28,73,1549,2343,2493,2593,2800,2927,3265,3593`
- **BLOQUEIA — A chave pode existir com um dia diferente do juiz.** `_dia_do_turno` transpõe `Turno.data_turno` para um mapa por instante localizado das batidas que o juiz classificou como `E`. `calcular_mes` grava esse mapa em `_dia_turno_juiz_calc`. Para `data_jornada`, o laço usa a data civil local na ausência da chave e, com saída anterior fechada e vão inclusivo de **0 a 6 h**, herda `_prev_dj` mesmo quando há resposta diferente do juiz. A janela lê `data_jornada`. O BO-B e a acusação de `aplicar_tolerancia` leem o mapa do juiz. Os overrides comercial e partido também reagrupam pelo juiz após `super().calcular_mes`. `app/ponto/motor_calculo_v2.py:867,884,1612,2030,2432,2439,2459,2494,2949,2983,3355,3367`
- **OK — Crédito do aberto, subclasses e FT.** `_d_a` vem do `data_jornada` já atribuído ao período aberto. O crédito seleciona turnos cujo `t.data_turno == _d_a` e grava `_d_a` para a entrada materializada, inclusive `_za.timestamp` na borda posicional. Essa seleção prova o dia **do grupo**. A chave individual pode faltar no mapa do juiz, que só indexa `E`. `MotorIntermitente` herda o 12x36, `MotorPlantao` delega ao 12x36 e `MotorTurnoPartidoNoturno` herda o partido. A FT do laço principal grava os dois memos e sai da janela pela guarda de folga. O crédito do aberto grava apenas o memo de dia e parte de `resultado.periodos`, cuja classificação é anterior. `app/ponto/motor_calculo_v2.py:889,1532,2483,2493,2529,2550,2559,2572,2593,3575,3601,3610`
- **OK — Marcos e parâmetros.** `_marcos_do_dia_fn` vem do vínculo ou do template. `_janela_do_dia` usa `hi`/`hf` para construir `m_entrada`/`m_saida` e grava `j['dia']`, `j['marco_entrada']` e `j['marco_saida']`. `marco_na_jornada` recebe `date`, hora cadastrada e referência local, mantém o `tzinfo` da referência e zera segundos e microssegundos do **marco**. Seus chamadores são as construções de entrada e saída do par e do BO-B. As pontas efetivas leem os respectivos `datetime`, a extra declarada, o piso, o teto e o estado autorizado. `hi`/`hf` continuam sendo as horas cruas usadas no relato. O cadastro de empresa é lido uma vez por cálculo, e o motor verifica a vigência pelo dia recebido. `app/escala/models.py:1128` · `app/colaboradores/models.py:54,117` · `app/ponto/janela_he.py:33,46` · `app/ponto/motor_calculo_v2.py:1372,1394,1562,1574,1633,1679,1735,1744,1767,1775,1782,2323,2334`

**Censo dos chamadores diretos na cópia**

| Severidade e método | Chamadores |
|---|---|
| **OK — `calcular_periodo`, código de execução** | Laço e crédito do aberto em `app/ponto/motor_calculo_v2.py:2495,2594`. Delegações de `MotorPlantao` e `Motor12x36ComEscala` em `app/ponto/motor_calculo_v2.py:3576,3588`. O censo não encontrou chamada direta em view, serviço, cron, management command ou script da cópia. |
| **OK — `_janela_do_dia`** | `_entrada_efetiva` e `_saida_efetiva` quando `_j` é omitido, e `_janela_do_par`. `app/ponto/motor_calculo_v2.py:1724,1766,1793` |
| **OK — `_janela_do_par`** | `Motor12x36`, `MotorTurnoPartido` e `MotorComercial`. `app/ponto/motor_calculo_v2.py:2800,2927,3265` |
| **FILA — `_entrada_efetiva` e `_saida_efetiva`** | `_janela_do_par` chama ambas passando `j`. `Motor12x36ComEscala` chama `_entrada_efetiva` de novo, sem `j`, e repete a consulta da janela e os efeitos de relato. Essa releitura já existia no comportamento descrito pelo selo B-O272b. `app/ponto/motor_calculo_v2.py:1793,3593` · `app/ponto/tests/test_o280_bateria_da_lei.py:2571` |
| **OK — `calcular_periodo`, testes diretos existentes** | `app/ponto/tests/test_plantao_he_distribuicao.py:27` · `app/ponto/tests/test_plantao_he_8a_hora.py:38` · `app/ponto/tests/test_noturno_desconta_intervalo.py:195,198` · `app/ponto/tests/test_o271_pontualidade_no_comercial.py:60,175` · `app/ponto/tests/test_antecipada_intervalo_12x36.py:53,59,65,70,79` · `app/ponto/tests/test_l084_vence_o_t8.py:65,173` · `app/ponto/tests/test_a6_12x36_tolerancia.py:33` · `app/ponto/tests/test_12x36_atraso_pela_autoridade.py:56,61,66,70,74` · `app/ponto/tests/test_12x36_com_escala_noturna.py:41,53,66` · `app/ponto/tests/test_motor_escala_marcos_do_dia.py:31` |
| **OK — `calcular_periodo`, teste novo** | Chamada com janela ligada que espera `RuntimeError`. `app/ponto/tests/test_janela_no_dia_do_turno.py:197,206` |

## 2. Semântica do dia em cada leitor

- **BLOQUEIA — L-084 por par × BO-B por dia.** A primeira usa `data_jornada` para buscar os marcos, mede as pontas **cruas do par** e pode dispensar a janela antes de consultar o BO-B. O BO-B usa `_dt_b` do juiz e mede a primeira entrada e a última saída do grupo, ou a borda posicional. Ambos exigem **duas** distâncias absolutas estritamente acima de 180 min. Mesmo quando os dias coincidem, uma volta de pausa pode ficar a mais de 180 min de `hi` pela própria posição dentro da jornada. Quando os dias divergem, o BO-B pode dispensar a janela com base em outro dia. Sua acusação grava `_dt_b`, mas o contador grava `dia` do par. `app/ponto/motor_calculo_v2.py:1562,1589,1612,1633,1641,1650,1980`
- **BLOQUEIA — Relato × espelho × Gestão de HE.** `_acusa_cadastro_x_realidade` recebe `j['dia'] = data_jornada` e grava batida e marco em `HH:MM` e minutos arredondados. `pontas_do_relato` exige igualdade com a data da linha, mesma hora de marco, célula da ponta `missing` e ausência de ponta já marcada no mesmo sentido. O espelho apresenta as batidas no dia de `dia_das_batidas`, que usa o juiz por PK e data civil local para a batida que nenhum turno reclama. Portanto uma cauda herdada pelo motor em **D**, mas exibida em **D+1**, pode ter sua acusação ignorada. O caminho normal da grade também usa **primeira E e última S**, enquanto o motor agora pode recortar a saída de **cada par**. A Gestão recebe as pendências do espelho. `app/ponto/motor_calculo_v2.py:1702,1742,1780,2441` · `app/ponto/janela_he.py:217,300,316,334` · `app/ponto/turnos.py:2907` · `app/ponto/services/espelho.py:464,741,754` · `app/ponto/services/he_pendente_lavrado.py:114,132,148`
- **BLOQUEIA — Extra declarada e vigência.** O cadastro escreve minutos inteiros por ponta no tipo de escala. A geração congela os valores no DNA. O vínculo lê `(colaborador, data)`, responde zero se a célula existe sem declaração e lê o template se a célula falta. O motor consulta `data_jornada` pelo vínculo da fatia. Espelho e calendário consultam a data da linha e escolhem o vínculo desse dia. Concordam quando dia e vínculo coincidem. Na herança divergente, podem ler DNA, extra e vigência diferentes. A vigência inclui o dia inicial, o piso inclui a igualdade e a acusação por teto exige sobra estritamente maior. A janela calcula em minutos fracionários e as telas arredondam as pontas publicadas. `app/escala/services/cadastro_tipo.py:256` · `app/ponto/management/commands/gerar_celulas.py:128` · `app/escala/models.py:1238` · `app/ponto/motor_calculo_v2.py:1356,1382,1679,2329` · `app/ponto/services/espelho.py:736` · `app/colaboradores/services/calendario.py:413` · `app/ponto/janela_he.py:70,123,173,242`
- **BLOQUEIA — `DecisaoHE.data`.** O campo é `DateField`. A porta grava **a data recebida**, sem deduzir dia civil nem dia do turno. O formulário individual manda `s.data` do retrato do espelho. Lote e autorização por período leem o mesmo retrato, e o cron de recusa passa a data do item à porta. Assim, para turno reconhecido pelo juiz a origem usual é o **dia exibido do turno**. A divergência aparece quando ele difere de `data_jornada`, ou quando um chamador envia outra data. O motor carrega `{d.data: d.estado}` e `autorizado` solta ambas as pontas dos pares que consultam aquela chave. Uma decisão exibida em **D+1** pode ficar sem efeito no par herdado em **D**, ou a autorização de **D** pode alcançar um par exibido em **D+1**. O selo novo cria a linha em **D** diretamente no ORM e deixa essa ligação sem exercício. Datas de decisões reais [nao verificado]. `app/ponto/models.py:1300` · `app/ponto/portas/he.py:66,195,206,265,369,595` · `app/templates/ponto/gestao_he.html:259` · `app/ponto/views.py:543,630,684` · `app/ponto/services/gestao_he.py:397` · `app/ponto/management/commands/recusar_ponta_pequena.py:156,170` · `app/ponto/motor_calculo_v2.py:1428,1686,1730,1772` · `app/ponto/tests/test_janela_no_dia_do_turno.py:95`
- **FILA — `_janela_fora_por_l084`.** O motor inicializa a lista e insere `dia` com deduplicação nos dois ramos. O leitor externo efetivo encontrado é o teste que mede seu comprimento. O comando `diff_janela_he` conta `len(resultado.dias_cadastro_x_realidade)`, que também recebe acusações de teto da janela e da pontualidade. Assim, esse comando não mede dias únicos retirados pela L-084. `app/ponto/motor_calculo_v2.py:1596,1640,1702,2025,2336,2760` · `app/ponto/tests/test_o280_bateria_da_lei.py:2819` · `app/ponto/management/commands/diff_janela_he.py:63,70`

## 3. `RuntimeError` e produção

- **OK — Alcance no código atual.** A view e o comando de recálculo entram pelo fechamento. O recálculo por evento, inclusive disparado por um fato de um dia, recalcula a competência e chega a `calcular_mes`. Espelho e simulação de autorização passam pela autoridade do período. PDF e diagnóstico também chamam `calcular_mes`. O cron `vigiar_motor_autoridade` e os comandos `diff_espelho_alimentacao` e `diff_extrato_parcial` fazem o mesmo. A entrega de holerite busca o documento gravado. Não identifiquei caminho dessas rotas até o novo `raise` com a chave ausente. Uso fora da cópia e ocorrência em produção [nao verificado]. `app/ponto/views.py:1280` · `app/ponto/services/fechamento.py:286,754,827,1810` · `app/ponto/management/commands/recalcular_fechamento.py:118` · `app/ponto/services/espelho.py:1200,1215` · `app/ponto/services/autorizacao_he_periodo.py:96` · `app/relatorios/pdf_espelho.py:434` · `app/ponto/services/esmeril_espelho.py:174` · `app/config/crons.py:596` · `app/ponto/management/commands/vigiar_motor_autoridade.py:58` · `app/ponto/management/commands/diff_espelho_alimentacao.py:58` · `app/relatorios/management/commands/diff_extrato_parcial.py:68` · `app/holerite/views.py:1304,1393,1445`
- **FILA — Mudança de contrato do chamador direto.** Com par fechado, chave ausente e empresa identificada com janela ligada, o `raise` acontece **antes** da vigência e da consulta de marcos. Pode levantar mesmo com `janela_he_desde` ausente ou posterior ao par. É uma falha explícita, sem retorno à data civil. Se um chamador novo atingir esse ramo, a exceção propaga onde não houver captura. O recálculo por evento tem captura externa. Entre os testes diretos do censo, só o teste novo injeta o contexto de empresa com janela ligada, e espera a exceção. `app/ponto/motor_calculo_v2.py:1379,1394,1532,1549,1556,2793` · `app/ponto/services/fechamento.py:1807,1818` · `app/ponto/tests/test_janela_no_dia_do_turno.py:197,206`

## 4. Marco de saída e valor do par

- **OK — 15:00–23:50 com saída 00:10.** No diff anterior, `m_saida` usava a data civil **da saída**, logo o marco era 23:50 de **D+1**. Com a âncora em **D**, fica 23:50 de **D**. Com a janela aplicável, piso e extra zero, a mesma regra L-097 passa a recortar 20 min, de 550 para 530 min brutos após a janela. Isso é consequência direta da âncora dentro do aval. `app/ponto/motor_calculo_v2.py:1574,1775` · `app/ponto/janela_he.py:166` · `app/ponto/tests/test_janela_no_dia_do_turno.py:177`
- **BLOQUEIA — Par inteiro depois do marco.** No B-I28, a cauda de **07:32–07:43** herda o dia do turno 19–07. O motor conserva a entrada às 07:32 e recorta a saída a 07:00. Para a L-084, só a distância da entrada passa de 180 min. O cálculo carimba a duração invertida, limita trabalhadas a zero e conserva **43 min fora**. Antes, a fixture mantinha o par cru de 11 min. A nova saída viola as duas propriedades do aval. Resultado da execução isolada da janela em memória, com os insumos da fixture. Incidência em produção [nao verificado]. `app/ponto/tests/test_o280_bateria_da_lei.py:4993,5007,5081,5127` · `app/ponto/motor_calculo_v2.py:1574,1589,2800,2826` · `app/ponto/janela_he.py:168,175` · `app/ponto/tests/test_janela_no_dia_do_turno.py:121`
- **BLOQUEIA — A troca também altera a dispensa pela L-084 sem troca do dia civil da entrada.** Com `data_jornada=D`, escala **19:00–00:10**, par **D 00:00–23:55**, piso e extra zero, a geometria anterior via duas pontas longe e preservava **1.435 min brutos**. Com `hf` em **D+1**, a saída fica a 15 min do marco e a janela corta **1.140 min** na entrada. Restam **295 min brutos**. O caso foi reproduzido no recorte em memória. O pareamento dessa geometria em produção [nao verificado]. `app/ponto/motor_calculo_v2.py:1574,1589,1980` · `app/ponto/janela_he.py:115,125`
- **FILA — FT e 24x48.** A FT marcada no laço sai antes da construção dos marcos. Para plantão com `hi == hf`, a autoridade registra que cruza a meia-noite. Um plantão 07:00 de **D** até 07:00 de **D+1** conserva o `hf`. Para **06:40–07:30 de D**, com `hi == hf == 07:00`, a janela isolada passa de **0 min efetivos e 50 fora** para **30 efetivos e 20 fora**, pois o `hf` muda de **D** para **D+1**. O exemplo mostra a mudança de valor na borda de igualdade. Um caso real de 24x48 com essa geometria [nao verificado]. `app/ponto/motor_calculo_v2.py:1538,1578,2493,3575` · `app/escala/servico_jornada.py:104` · `app/ponto/janela_he.py:125,175`

## 5. Classes de reprovação

- **BLOQUEIA — Derivação paralela presente.** O juiz registrado da atribuição da batida é `turnos_do_colab`. O diff não introduz outro pareador nem outro predicado `cruza?`. Ele eleva a herança e a degradação civil já existentes a fonte do dinheiro da janela, enquanto BO-B e apresentação continuam lendo o dia do juiz. Há também composição paralela do **instante de saída**: `marcos_turno` encadeia todos os marcos, inclusive pausa, enquanto os dois ramos da janela usam apenas `hi/hf` e `+1`. Por exemplo, a sequência `18:00 / 23:00 / 01:00 / 19:00` põe `hf` em **D+1** no encadeador e em **D** na janela. Uso dessa sequência em produção [nao verificado]. `app/docs/JUIZES.md:20,24` · `app/core/juizes.py:215` · `app/escala/servico_jornada.py:69,82,104` · `app/ponto/motor_calculo_v2.py:1574,1612,1633,2439`
- **OK — Segundo escritor persistente, relógio fora da borda e mudança de trava ausentes no diff.** As escritas novas são no memo da instância, antes dos dois cálculos. A porta de HE continua adquirindo a trava e gravando a decisão. O marco usa a referência recebida. A L-084 segue antes do clipe e da leitura do estado autorizado. Há **zero `except` adicionados e zero removidos** no diff fornecido. O `except Exception` da consulta de empresa já existia. A chave ausente ganhou um `raise` e o retorno sem janela para empresa desligada ou indisponível. `app/ponto/motor_calculo_v2.py:1394,1549,1589,1674,2494,2593` · `app/ponto/portas/he.py:158,195,206` · `app/ponto/janela_he.py:46`
- **BLOQUEIA — Campo com dois significados presente.** `dia` no relato de janela e no contador vem do período com herança. `dia` da acusação BO-B e da pontualidade vem do juiz. `DecisaoHE.data` recebe o dia apresentado e o motor a consulta pelo dia herdado. `app/ponto/motor_calculo_v2.py:1641,1655,1686,1703,2030,2444` · `app/ponto/services/he_pendente_lavrado.py:148` · `app/ponto/portas/he.py:66`
- **OK — Os dois mutantes têm asserções capazes de morder.** Restaurar `entrada.date()` perde, por exemplo, o recorte de **2 min** esperado na cauda literal de col113. Suprimir o `+1` do marco **do par** quebra a guarda **18:40 → 07:00**, que espera saída em **D+1**, 720 min trabalhados e 20 fora. A execução em memória confirmou a quebra da geometria da guarda sem `+1`. O mutante que remove **só** o `+1` do BO-B não tem prova dedicada localizada. `app/ponto/tests/test_janela_no_dia_do_turno.py:160,231,310,323` · `app/ponto/motor_calculo_v2.py:1579,1635`
- **BLOQUEIA — Matriz incompleta e pré-requisito de pouso aberto.** As fixtures comercial e vespertina gravam `tipo_base='5x2'`, que a fábrica rejeita antes das asserções. A propriedade não cobre a cauda depois do `hf` do B-I28. O caso col200 cobra apenas a entrada, e o DNA criado na fixture não inclui `extra_declarada`, ainda que o tipo tenha minutos cadastrados. Suite integral verde, sonda dos 57 dias e dos ~16 mil pares e zero violações na frota [nao verificado]. `app/ponto/tests/test_janela_no_dia_do_turno.py:68,73,106,121,243,279,290,321` · `app/ponto/motor_calculo_v2.py:3634,3692` · `app/escala/models.py:1245` · `CLAUDE.md:383` · `CORTES.md:643` (anexo)
- **OK — `.replace(hour=...)` fora do módulo da janela ausente nos sítios vigiados.** O novo `replace` fica em `janela_he.py`. O teste estrutural verifica o módulo, os quatro métodos da janela e `calculador/regras.py`. `app/ponto/janela_he.py:48` · `app/ponto/tests/test_janela_he_regra.py:124,136,147,154`

## 6. Selos existentes e assinatura

- **OK — Assinatura no censo da cópia.** `entrada_efetiva` agora exige o marco como `datetime`. Um consumidor com `hi=` falharia pela assinatura, e um `time` posicional falharia na subtração. O chamador executável encontrado já passa `j['marco_entrada']`. `calculador/regras.py` só importa a função. `test_janela_he_regra.py` usa `calcular_mes` e verifica o import e a AST. O teste direto de `saida_efetiva` já passa `datetime`, e as chamadas com aliases no selo do calculador são texto de fixture AST. `app/ponto/janela_he.py:95,115` · `app/ponto/motor_calculo_v2.py:1734` · `app/ponto/calculador/regras.py:30` · `app/ponto/tests/test_janela_he_regra.py:67,124,136` · `app/ponto/tests/test_janela_he_saida.py:134` · `app/ponto/tests/test_chamador_do_calculador_aplica_a_janela.py:134`
- **BLOQUEIA — Duas falhas confirmadas na catraca.** Executei a versão em memória indicada no próprio teste, com `unittest` e sem Django. `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` falha porque o contador desce **84 → 83**. `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` falha porque a entrada civil da janela saiu e o registro mantém posições antigas, inclusive `2408 → 2444` na herança. `app/core/tests/test_medida_turno_do_par.py:20,299,544,560` · `app/ponto/motor_calculo_v2.py:1549,2444`
- **BLOQUEIA — A bateria O280 fixa comportamentos que a obra muda.** B-P4a exige o par antigo invertido e carrega um `expectedFailure` para os **150 min** da lei. B-I28 exige a cauda crua e **731 min** no dia, além de ter outro `expectedFailure`. São incompatibilidades identificadas por leitura e pela execução isolada da janela. Resultado integral desses testes Django e dos pinos [nao verificado]. `app/ponto/tests/test_o280_bateria_da_lei.py:4926,4935,4961,4982,5081,5113,5123`

## 7. Limite do aval, atraso e competência exportada

- **BLOQUEIA — O recorte excede a correção pedida no B-I28.** Os corpos de `calcular_per

[truncado em 20000 de 22110 caracteres]

## 24/11 02:29 · sessao 08df8f8609fb · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

# DEPOIS — NOTURNO-A-JANELA-NO-DIA-DO-TURNO

A soma das 18 classes é **402**, igual ao `C1 402` do anexo. Em `iguais_so_fechados`, são **320** casos, dos quais **308** têm `motor_agora=0` e **12** têm `motor_agora>0`. [nao verificado]

As marcações `[nao verificado]` nas tabelas indicam que a fonte é o anexo raspado fornecido, sem verificação independente na sombra. [nao verificado]

## 1. Contagem por classe

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | [nao verificado] |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | [nao verificado] |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | [nao verificado] |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | [nao verificado] |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | [nao verificado] |
| `entrada_repetida+virada_ou_outro_dia` | 1 | [nao verificado] |
| `iguais_com_aberto` | 62 | [nao verificado] |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | [nao verificado] |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | [nao verificado] |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 45 | [nao verificado] |
| `iguais_so_fechados\|bruto>30\|motor_agora>0` | 12 | [nao verificado] |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | [nao verificado] |
| `virada_ou_outro_dia` | 3 | [nao verificado] |
| **Soma = C1** | **402** | [nao verificado] |

O anexo termina em `rc_sombra=0` e não traz traceback para transcrever. [nao verificado]

## 2. Exemplos `fech` e corte da faixa 22h–05h

Cada `pares` mantém a ordem do anexo `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]`. O teste usa a borda da faixa que o par bruto alcançou. Nos 12 exemplos com motor zero, a saída efetiva eliminou o trecho após as 22h. Nos quatro com motor positivo, a entrada efetiva ficou antes das 05h do dia seguinte e o par preservou esse trecho da madrugada. [nao verificado] [nao verificado]

### `iguais_so_fechados|bruto5-30|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 108 | 2026-10-19 | 10.5 | 0.0 | `[["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-21 | 6.3 | 0.0 | `[["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-25 | 17.6 | 0.0 | `[["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-11-16 | 15.6 | 0.0 | `[["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto<=5|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 60 | 2026-10-31 | 3.5 | 0.0 | `[["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]]` | Sim, saída efetiva 19:00:00 < 22:00. [nao verificado] |
| 108 | 2026-10-01 | 0.7 | 0.0 | `[["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-03 | 0.5 | 0.0 | `[["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-05 | 0.9 | 0.0 | `[["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto>30|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 373 | 2026-10-22 | 48.8 | 0.0 | `[["2026-10-22 16:58:11", "2026-10-22 22:48:50", "2026-10-22 16:58:11", "2026-10-22 22:00:00", 301.8, 48.8]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-24 | 48.9 | 0.0 | `[["2026-10-24 16:52:22", "2026-10-24 22:48:55", "2026-10-24 16:52:22", "2026-10-24 22:00:00", 307.6, 48.9]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-26 | 49.6 | 0.0 | `[["2026-10-26 16:56:11", "2026-10-26 22:49:38", "2026-10-26 16:56:11", "2026-10-26 22:00:00", 303.8, 49.6]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-28 | 48.9 | 0.0 | `[["2026-10-28 16:59:53", "2026-10-28 22:48:56", "2026-10-28 16:59:53", "2026-10-28 22:00:00", 300.1, 48.9]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto>30|motor_agora>0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 37 | 2026-10-05 | 119.4 | 119.4 | `[["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 03:00:38", "2026-10-06 07:00:00", 239.4, 1.0]]` | Não, preservou 03:00:38–05:00 do dia seguinte. [nao verificado] |
| 37 | 2026-10-09 | 119.3 | 119.3 | `[["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 03:00:40", "2026-10-10 07:00:00", 239.3, 1.8]]` | Não, preservou 03:00:40–05:00 do dia seguinte. [nao verificado] |
| 113 | 2026-10-30 | 120.0 | 137.1 | `[["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 03:00:00", "2026-10-31 07:00:00", 240.0, 2.0]]` | Não, preservou 03:00:00–05:00 do dia seguinte. [nao verificado] |
| 113 | 2026-11-01 | 120.0 | 137.1 | `[["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 03:00:00", "2026-11-02 07:00:00", 240.0, 1.0]]` | Não, preservou 03:00:00–05:00 do dia seguinte. [nao verificado] |

## 3. `motor_agora>0` com `DiaPago=0`

**Sim, no sentido de divergência de valor frente ao recálculo da cópia.** Nos 12 casos dessa subclasse, a sonda reporta noturno legal positivo no motor atual e zero no `DiaPago` usado pelo C1. [nao verificado]

No código da cópia, a janela busca o dia da jornada atribuído ao par e coloca o marco de saída no dia seguinte quando o turno cruza a meia-noite. O motor calcula `minutos_noturnos_legais` sobre as pontas efetivas, desconta o noturno dos intervalos e aplica o fator de hora noturna, com prorrogação conforme a configuração. [`motor_calculo_v2.py:2494`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:2494), [`motor_calculo_v2.py:1549`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549), [`motor_calculo_v2.py:1574`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574), [`motor_calculo_v2.py:2833`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:2833), [`motor_calculo_v2.py:983`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:983)

Na lavra `versao='motor'`, o fechamento entrega os períodos e o mapa `dia_da_jornada`. `lavrar` soma `minutos_noturnos_legais / 60` em `horas_noturnas` do dia atribuído e substitui as linhas da competência para aquela versão. Se receber os mesmos períodos positivos do recálculo para o mesmo dia, a lavra desse dia sai positiva. [`fechamento.py:1201`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1201), [`fechamento.py:1217`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1217), [`fechamento.py:1227`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1227), [`dia_pago.py:173`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/dia_pago.py:359)

**A causa histórica do zero fica em aberto.** Data e versão da lavra original, períodos e insumos que ela recebeu, eventual tentativa de relavra e atribuição dos 12 casos à mudança de ancoragem da janela estão [nao verificado].

## 24/11 02:26 · sessao 7e01b5d91af0 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## (1) UNIVERSO e emp_comp

```text
UNIVERSO pares_fechados 16167 invertidos 4 sha256 848979805ef7be0fd6fce6610e41ce9e53c68dcfe3403c902b916763c17bb11d
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 2
emp_comp 2 11/2026 fechados 6337 invertidos 1
emp_comp 2 12/2026 fechados 275 invertidos 0
emp_comp 3 10/2026 fechados 1478 invertidos 1
emp_comp 3 11/2026 fechados 1408 invertidos 0
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 0
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## (2) Linhas INV

```text
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
```

## (3) Classificação e contas

**Os quatro casos são B.** Em cada linha do anexo, o dia civil da entrada coincide com o dia de jornada da coluna 5, a entrada efetiva é igual à entrada bruta e a saída foi recortada para antes de ambas as batidas. A cópia ancora os marcos no dia da jornada e grava as pontas resultantes do recorte. O limite de saída pode incorporar extra declarada. O marco cadastrado e a extra declarada, separadamente para cada par desta execução, ficam [nao verificado]. [app/ponto/motor_calculo_v2.py:1549](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549) · [app/ponto/motor_calculo_v2.py:1574](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574) · [app/ponto/janela_he.py:168](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/janela_he.py:168) · [app/ponto/motor_calculo_v2.py:1873](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1873)

| INV | Classe | Dia civil da entrada = dia de jornada | Saída efetiva < entrada bruta ≤ saída bruta | Saída efetiva − entrada efetiva |
|---|---|---|---|---|
| 109 | B | `2026-10-05` | `22:00:00 < 22:00:41 < 22:10:41` | `22:00:00 − 22:00:41 = −41 s` |
| 399 | B | `2026-09-30` | `18:30:00 < 18:30:29 = 18:30:29` | `18:30:00 − 18:30:29 = −29 s` |
| 456 | B | `2026-11-14` | `19:00:00 < 19:00:08 = 19:00:08` | `19:00:00 − 19:00:08 = −8 s` |
| 473 | B | `2026-09-21` | `19:00:00 < 19:00:18 < 19:23:54` | `19:00:00 − 19:00:18 = −18 s` |

## 24/11 02:23 · sessao e938a376abf2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

A sonda `DEPOIS` parou na inicialização do contêiner, com `rc_sombra=125`. O trecho raspado termina sem contagens de C1 e sem linhas `fech`. (`DADO RASPADO:3–6`, contando a partir da primeira linha `sonda_leitura`.)

### 1. Contagens e soma C1

| Classe | Noturno bruto | Motor agora | Contagem |
|---|---:|---:|---|
| `iguais_so_fechados` | ≤ 5 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | ≤ 5 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | > 0 | [nao verificado] |
| Demais classes, inclusive seus nomes | | | [nao verificado] |
| **Soma = C1** | | | **[nao verificado]** |

O trecho contém o erro do Docker abaixo. Traceback Python [nao verificado]. (`DADO RASPADO:3–6`)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "/home/ronald/saas-hasner/app/staticfiles" to rootfs at "/app/staticfiles": create mountpoint for /app/staticfiles mount: make mountpoint "/app/staticfiles": mkdirat /var/lib/docker/rootfs/overlayfs/ff272d7d2278fe95edf6641b5e8d4057eb88fbf061c4bd2fb5824188b871dde0/app/staticfiles: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

### 2. Exemplos `fech` e corte da faixa

Nas seis subclasses, os pares a transcrever e o resultado dos testes `efetivo_saida <= 22:00` e `efetivo_entrada >= 05:00` ficam [nao verificado]. O motor registra as pontas efetivas e usa o trecho recortado para calcular o noturno, com desconto das pausas e prorrogação conforme a configuração. [`motor_calculo_v2.py:1867`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867), [`motor_calculo_v2.py:2927`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2927), [`motor_calculo_v2.py:983`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) [nao verificado]

### 3. `motor_agora > 0` com `DiaPago = 0`

Se o positivo vier de um período que a lavratura `versao='motor'` incluiria na mesma linha de colaborador, competência e dia da jornada, o zero gravado diverge do que essa lavratura produziria hoje. `lavrar` soma `minutos_noturnos_legais / 60` dos períodos recebidos no dia atribuído e grava o resultado quando é chamada. [`fechamento.py:1200`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1200), [`fechamento.py:1217`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1217), [`dia_pago.py:173`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

A ocorrência dessa divergência na sombra, a data e a versão da lavra e a hipótese de que o zero veio de uma lavra anterior à mudança do motor ficam [nao verificado].

_8054 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
