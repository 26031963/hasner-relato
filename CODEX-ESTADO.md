# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 18:04** · publicado em **13/11/2026 18:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 18:04 · sessao 07de28eaa58b · RAIA-PREPARACAO-LASTRO · censo · origem=Code

**O escritor do estado já está concentrado; a decisão de lastro ainda está repartida.** O núcleo é `_gravar_estado`; o coletor mais próximo de uma autoridade única é `premissa_regularizada`; o degrau 2 de `lastro.julgar` e a proximidade dentro da lavratura continuam respondendo ao casamento batida × marco por critérios próprios. **LEU:** `app/chamados/models.py:201`, `app/chamados/models.py:1253`, `app/ponto/services/lastro.py:299`, `app/ponto/services/cartorio.py:666`.

Convenções: **LEU** indica leitura da cópia; **INFERIU** indica conclusão ou desenho apoiado nas referências. Os testes abaixo foram **lidos**; sua execução nesta cópia é **[não verificado]**. Dados atuais de banco, logs e resultado de deploy são **[não verificado]**. [nao verificado]

**1. Matriz de transições**

O campo temporal do chamado é **`resolvido_em`**, não `fechado_em`. `VIVOS = aberto/em_analise/registrado`; cobrança efetiva exclui `registrado`; `ENCERRADOS = resolvido/fechado`. **LEU:** `app/chamados/models.py:52`, `app/chamados/models.py:54`, `app/chamados/models.py:60`, `app/chamados/catalogo/motor.py:24`.

Nas linhas abaixo, **ritual T** significa: `_gravar_estado` grava `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; terminal recebe via e rebaixa urgência; vivo limpa carimbo/via e restaura urgência. Quando solicitado, grava também `respondido_por`. T solicita `LogAuditoria.acao='estado_chamado'`, com estado anterior, novo estado e via, antes do `save`, no mesmo `atomic`. **LEU:** `app/chamados/models.py:228`, `app/chamados/models.py:233`, `app/chamados/models.py:247`, `app/chamados/models.py:280`, `app/chamados/models.py:292`.

| Transição / gatilho | JUIZ que decide | OPERAÇÃO que escreve | Persistência / trilha | Retorno | Prova existente, lida |
|---|---|---|---|---|---|
| Inexistente → `aberto` | `decidir(EMITIR)`; classificação acionável em `_evento_nascimento`. `app/chamados/catalogo/motor.py:130`; `app/chamados/models.py:1047` | `abrir`, por `get_or_create`; ou `criar`, por `create`. `app/chamados/models.py:972`; `app/chamados/models.py:1096` | Chamado, vínculo com célula e `nascimento_chamado`; módulos de turno com célula emitem `EventoCelula`. `app/chamados/models.py:978`; `app/chamados/models.py:1098`; `app/chamados/models.py:1219` | `abrir`: `(ch, criado)`; `criar`: `ch`; recusas podem devolver ausência. `app/chamados/models.py:966`; `app/chamados/models.py:982`; `app/chamados/models.py:1075`; `app/chamados/models.py:1101` | `app/chamados/tests/test_chamado_nasce_com_trilha.py:30`; deduplicação: `app/chamados/tests/test_selo_dedup_premissa_viva.py:114` |
| Inexistente → `registrado`, módulo sem ação devida | `decidir(REGISTRAR)`. `app/chamados/catalogo/motor.py:133`; `_evento_nascimento`, `app/chamados/models.py:1047` | Mesmas portas de nascimento. `app/chamados/models.py:972`; `app/chamados/models.py:1096` | Nascimento com `status_local=registrado`; trilha de nascimento. `app/chamados/models.py:939`; `app/chamados/models.py:1219` | Mesmos retornos acima. `app/chamados/models.py:982`; `app/chamados/models.py:1101` | `app/chamados/tests/test_chamado_nasce_com_trilha.py:38` |
| `aberto/registrado` → `em_analise`, evento `ANALISAR` | `decidir`. `app/chamados/catalogo/motor.py:112` | `analisar` → T. `app/chamados/models.py:801` | Ritual T. `app/chamados/models.py:807` | `bool`. `app/chamados/models.py:806` | `app/chamados/tests/test_e1_v5_ordem_trava.py:100` |
| `aberto` → `em_analise`, ação humana | `pode_transicionar` → `decidir(TRANSICAO_HUMANA)`. `app/chamados/models.py:119`; `app/chamados/catalogo/motor.py:100` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199`; `app/chamados/models.py:447` | T, via `admin`, autor. `app/chamados/models.py:450` | `None`; recusa levanta `ValueError`. `app/chamados/models.py:128`; `app/chamados/models.py:199` | `app/chamados/tests/test_transicoes.py:57` |
| `aberto/em_analise` → `resolvido`, ação humana | Matriz humana, guardas de justificativa/disputa e S122; S122 pergunta `premissa_regularizada`. `app/chamados/catalogo/motor.py:100`; `app/chamados/models.py:136`; `app/chamados/models.py:181` | `transicionar`/`fechar` → `_aplicar` → T. `app/chamados/models.py:199`; `app/chamados/models.py:452` | T via `admin`; `fechar` pode anexar nota de fechamento. `app/chamados/models.py:450`; `app/chamados/models.py:456` | `None`; recusa pode levantar. `app/chamados/models.py:192`; `app/chamados/models.py:455` | `app/chamados/tests/test_transicoes.py:94`; `app/chamados/tests/test_transicoes.py:178` |
| `aberto/em_analise/registrado/resolvido` → `fechado`, ação humana | Matriz humana; S122 está condicionado ao destino `resolvido`, não a todo fechamento. `app/chamados/catalogo/motor.py:99`; `app/chamados/models.py:181` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199` | T via `admin`. `app/chamados/models.py:450` | `None`. `app/chamados/models.py:199` | `app/chamados/tests/test_contract_status_nucleo.py:49` |
| `em_analise` → `aberto`, ação humana | `decidir`, matriz humana. `app/chamados/catalogo/motor.py:101` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199` | T; permanece vivo, sem via de encerramento. `app/chamados/models.py:247` | `None`. `app/chamados/models.py:199` | Matriz exercitada em `app/chamados/tests/test_transicoes.py:57`; caso específico desta aresta **[não verificado]** |
| `resolvido` → `em_analise`, ação humana | Matriz humana. **Não passa por `recusa_da_reabertura`.** `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:199` | `transicionar` → `_aplicar` → T | T; limpa carimbo/via. `app/chamados/models.py:247` | `None`. `app/chamados/models.py:199` | Caso funcional específico **[não verificado]** |
| `fechado` → estado permitido por override `admin_ti` | Ramo especial de `decidir`. `app/chamados/catalogo/motor.py:150` | `transicionar` → `_aplicar` → T. `app/chamados/models.py:199` | T; campos dependem do destino. `app/chamados/models.py:233` | `None` ou recusa. `app/chamados/models.py:124` | `app/chamados/tests/test_transicoes.py:178` |
| VIVO → `resolvido`, premissa regularizada | `_decisao_do_fechamento`: motor + premissa + pergunta viva. Coletor reconhece justificativa, decisão admin, dia sem trabalho, marcos acesos/inexistentes e fatos do dia. `app/chamados/models.py:664`; `app/chamados/models.py:1253` | `reconciliar` → T. `app/chamados/models.py:642` | T via `sistema`; eventual reaponte de perguntas precede fechamento autorizado. `app/chamados/models.py:655`; `app/chamados/models.py:686` | `bool`; `reconciliaria` fornece o mesmo sim/não sem escrita. `app/chamados/models.py:656`; `app/chamados/models.py:658` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:95`; `:113`; `:244` |
| `batida_ausente` VIVO → `resolvido`, marco aceso no evento | `marcos_acesos is True`, depois o gate de `reconciliar`. `app/ponto/services/lastro.py:395`; `app/ponto/services/lastro.py:401` | `fechar_pelo_marco_colab` → `transicionar_com_pauta(ato=reconciliar)` | T + ocorrência `lastro_encontrado`, no mesmo `atomic`. `app/ponto/services/lastro.py:400` | Número de fechados; falhas podem ser acumuladas. `app/ponto/services/lastro.py:407`; `app/ponto/services/lastro.py:409` | `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:97`; idempotência `:112`; rollback da nota `:152` |
| VIVO → `resolvido`, cartório: marco satisfeito/dia sem trabalho | **Sem juiz registrado para a composição `_protesta`**; combina `trabalha` e `marcos_acesos`, depois pergunta o gate. `app/ponto/services/cartorio.py:1458`; `app/ponto/services/cartorio.py:1539` | `_julgar_colab_corpo` → `reconciliar` → T | T; atualiza contadores e agenda rejulgamento. `app/ponto/services/cartorio.py:1541` | Resultado incorporado ao `out` do julgamento. `app/ponto/services/cartorio.py:1541` | Marco individual: `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:185`; prova específica de toda a composição **[não verificado]** |
| VIVO → `resolvido`, marcos antigos inexistentes após troca | `marcos_inexistentes`, depois gate. `app/ponto/services/cartorio.py:1550`; `app/chamados/models.py:1298` | Cartório → `reconciliar` → T | T + nota de troca de escala; rejulga célula. `app/ponto/services/cartorio.py:1552`; `app/ponto/services/cartorio.py:1556` | Contagem em `out`. `app/ponto/services/cartorio.py:1563` | Referência aos REDs no próprio coletor: `app/chamados/models.py:1291`; execução **[não verificado]** |
| VIVO → `resolvido`, dedup/agrupado, mesmo com furo no dia | Pai vivo ou sobrevivente escolhido por C8. `app/chamados/models.py:739`; `app/chamados/models.py:749` | `reconciliar` → T | T; dedup acrescenta `fechamento_por_dedup`, com sobrevivente. `app/chamados/models.py:691`; `app/chamados/models.py:757` | `bool`. `app/chamados/models.py:656` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:80`; agrupado `:143` |
| VIVO → `resolvido`, furo sem dia apurável | **Sem juiz do lastro:** exceção inline permite sem julgar premissa; guarda da pergunta viva ainda participa. `app/chamados/models.py:733`; `app/chamados/models.py:686` | `reconciliar` → T | T + `fechamento_sem_dia_do_fato`. `app/chamados/models.py:735` | `bool`. `app/chamados/models.py:656` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:130` |
| VIVO → `resolvido`, retratação da premissa | `_veredito_premissa` → catálogo `avaliar`; `_retratar` aplica retenções. `app/chamados/reconciliador.py:248`; `app/chamados/reconciliador.py:504` | `retratar_premissa_morta_colab` → `_retratar` → `reconciliar` | T, tratamento das perguntas/justificativa pendente e nota `retratado`. `app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:533`; `app/chamados/reconciliador.py:539` | Unidade `bool`; lote `int`. `app/chamados/reconciliador.py:542`; `app/chamados/reconciliador.py:1471` | `app/chamados/tests/test_selo_retratador_turno_do_chamado.py:81`; `app/chamados/tests/test_retratar_rejeita_justificativa.py:48` |
| Chamado de turno VIVO → `resolvido`, célula absolveu | `_absolvido_pela_celula`: veredito não acusador e nenhum marco pendente. `app/chamados/reconciliador.py:1019` | `retratar_chamados_absolvidos_por_celula` → `_retratar` | Mesmo ritual de retratação. `app/chamados/reconciliador.py:962`; `app/chamados/reconciliador.py:504` | Número de retratados. `app/chamados/reconciliador.py:1016` | `app/chamados/tests/test_selo_retratador_turno_do_chamado.py:95`; `:117` |
| Origem VIVA → `resolvido`, justificativa aprovada | Aprovação é o fato; retenção é da unidade de retratação e regularização é do coletor. `app/ponto/services/justificativa.py:16` | `encerrar_chamado_de_origem` → `retratar`, dentro da porta de aprovação | T + nota da aprovação; pode registrar retenção sem mudar estado. `app/ponto/services/justificativa.py:27`; `app/ponto/services/justificativa.py:46` | `bool`. `app/ponto/services/justificativa.py:33` | `app/ponto/tests/test_justificativa_encerra_origem.py:74`; `:119`; `:141` |
| VIVO → `resolvido`, ausência aprovada | C11 `reconciliar_por_ausencia`: cobertura + `_premissa_morre` + gate. `app/core/juizes.py:287`; `app/chamados/services/silencio_ausencia.py:94` | `reconciliar_por_ausencia` → `reconciliar` | T + nota `falta_redundante_ausencia`, trilha `reconciliar_ausencia_lancada`; pode levantar silêncio. `app/chamados/services/silencio_ausencia.py:102` | `'fechou'`, outros resultados nomeados ou `None`. `app/chamados/services/silencio_ausencia.py:53` | `app/ponto/tests/test_reavaliar_catalogo_premissa.py:98` |
| VIVO → `resolvido`, feriado/folga | `espurios` pergunta A2 e C3; gate confirma. `app/chamados/services/feriado_reavaliar.py:24` | `encerrar` do serviço → `reconciliar` | T + nota `falta_espuria_feriado` + auditoria `reconciliar_feriado`. `app/chamados/services/feriado_reavaliar.py:48` | Unidade `bool`; lote `int`. `app/chamados/services/feriado_reavaliar.py:57`; `:86` | `app/ponto/tests/test_reavaliar_catalogo_premissa.py:98` prova ausência; prova específica de feriado nesta leitura **[não verificado]** |
| VIVO → `resolvido`, tentativa por ordem de cobertura/substituição | C3 recebe fatos falsos do chamador, mas gate pode recusar porque o dia permanece furado. `app/ponto/services/cobertura.py:118`; `app/ponto/services/cobertura.py:312` | Portas de cobertura → `reconciliar` | T somente se aceito; nota distingue coberto de chamado ainda aberto. `app/ponto/services/cobertura.py:123`; `app/ponto/services/cobertura.py:316` | Resultado da porta inclui recusados. `app/ponto/services/cobertura.py:319` | `app/ponto/tests/test_cobertura_followup.py:20`; cobertura completa das recusas **[não verificado]** |
| Pai VIVO → `resolvido`, disputa fechada | Receiver lê `fechada_em`; a premissa é decidida pelo gate. `app/chamados/signals.py:20`; `app/chamados/signals.py:36` | Receiver → `reconciliar` | T + nota `fechado_pela_disputa`, em `atomic`. `app/chamados/signals.py:42` | Receiver retorna `None`. `app/chamados/signals.py:44` | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:268`; `:278` |
| VIVO/`resolvido` → `fechado`, cascata | `decidir(ENCERRAR)` + guarda da pergunta viva; não pergunta o gate da premissa. `app/chamados/models.py:1615` | `encerrar` → T | T via `cascade`. `app/chamados/models.py:1620` | `bool`. `app/chamados/models.py:1621` | `app/chamados/tests/test_contract_status_nucleo.py:61`; `app/ponto/tests/test_justificativa_encerra_origem.py:298` |
| VIVO → `cancelado` | `decidir(CANCELAR)` + guarda da pergunta viva. `app/chamados/models.py:784` | `cancelar` → T | T via `cancelado`; nota se houver motivo. `app/chamados/models.py:789` | `bool`; terminal absoluto. `app/chamados/models.py:793`; `app/chamados/catalogo/motor.py:142` | `app/chamados/tests/test_motor_cancelado.py:22` |
| VIVO → `fechado`, competência trancada | Serviço `medir` seleciona competência e exclusões; gate pergunta motor/fio. `app/chamados/services/competencia_trancada.py:66`; `app/chamados/models.py:1623` | Serviço `encerrar` → `encerrar_por_competencia_trancada` → T | T via `competencia_trancada`, nota e auditoria própria. `app/chamados/models.py:1640`; `app/chamados/services/competencia_trancada.py:210` | Gate `bool`; serviço devolve medição com aplicadas. `app/chamados/models.py:1641`; `app/chamados/services/competencia_trancada.py:234` | `app/chamados/tests/test_competencia_trancada.py:68`; `:106`; `:211` |
| `resolvido/fechado` → `em_analise`, reabertura pela porta própria | `recusa_da_reabertura`: veto, admin sem fato novo, premissa, agrupado, irmão e motor. `app/chamados/models.py:1319` | `reabrir` → T → `ressuscitar_perguntas` | T; limpa via/carimbo; ressuscita apenas universo aprovado pelos juízes. `app/chamados/models.py:1443`; `app/chamados/models.py:1469` | `bool`; recusa pode registrar motivo/redirecionar perguntas. `app/chamados/models.py:1395`; `:1412`; `:1453` | `app/chamados/tests/test_reabrir_pergunta_a_premissa.py:61`; `app/chamados/tests/test_reabrir_decide_pelo_banco.py:1` |
| Encerrado reencontrado pelo emissor → `em_analise`, renascimento | `_renascer_por_premissa_viva` aplica restrições e pergunta `reabrir`. `app/chamados/models.py:984` | Wrapper → `reabrir` → T | T + nota `renascido`. `app/chamados/models.py:1020` | `bool`; `abrir` devolve chamado existente. `app/chamados/models.py:1025`; `:982` | `app/chamados/tests/test_selo_dedup_premissa_viva.py:85`; `:128` |
| Encerrado pela tranca → `em_analise`, competência reaberta/desarquivamento | Serviço seleciona via/competência; `reabrir(humano=True)` conserva as demais guardas. `app/chamados/services/competencia_trancada.py:260`; `:290` | Serviço → `reabrir`; desarquivamento também levanta véu | T + nota `competencia_reaberta` + auditoria própria. `app/chamados/services/competencia_trancada.py:291` | Serviço `int`; tela JSON. `app/chamados/services/competencia_trancada.py:298`; `app/chamados/views_arquivar.py:89` | `app/chamados/tests/test_tranca_de_verdade.py:74`; `app/chamados/tests/test_competencia_trancada.py:164` |
| Arquivar/suspender **sem mudar estado** | Prazo do motor e motivo do chamador. `app/chamados/services/arquivo.py:107`; `:141` | `arquivo.silenciar` | `silenciado_ate/por/motivo`, `atualizado_em` e auditoria da ação. `app/chamados/services/arquivo.py:166` | `bool`. `app/chamados/services/arquivo.py:174` | `app/chamados/tests/test_porta_arquivo_silencio.py:314`; estado intacto `app/chamados/tests/test_f1_arquivar.py:42` |
| Levantar arquivo/suspensão **sem mudar estado** | Guardas da porta de arquivo. `app/chamados/services/arquivo.py:177` | `arquivo.levantar` | Limpa silêncio; pode carimbar primeiro `resgatado_em`; auditoria. `app/chamados/services/arquivo.py:202` | `bool`. `app/chamados/services/arquivo.py:212` | `app/chamados/tests/test_porta_arquivo_silencio.py:233`; `:336` |

`superado` permanece como estado de acervo, mas a porta `superar` foi retirada; não o conto como transição executável corrente. **LEU:** `app/chamados/models.py:795`.

`EventoCelula` é a entrega do fato ao consumidor, não a trilha da transição do chamado: nasce por `emitir_evento_celula`; o consumidor executa retratações/lastro e grava `processado_em`, ou incrementa `tentativas/erro`, ou adia pelo teto. **LEU:** `app/ponto/portas/celula.py:945`, `app/chamados/reconciliador.py:1364`, `app/chamados/reconciliador.py:1383`, `app/chamados/reconciliador.py:1393`.

**Limite da persistência:** T solicita auditoria, mas `evento` é explicitamente *best-effort* e retorna `None` se falhar. **INFERIU:** o HEAD não garante incondicionalmente “estado persistido implica uma trilha”. **LEU:** `app/core/observ.py:94`, `app/core/observ.py:105`, `app/core/observ.py:133`.

---

**2. Censo AST: script e resultado**

O script abaixo lê e analisa texto Python; não importa aplicações nem acessa banco. A parte de escritores procura atribuições e chamadas ORM, incluindo `defaults`/`update_fields` em variáveis locais. A parte de leitores indexa o manifesto de decisões encontrado na leitura e compara chamadas diretas com o registro. **INFERIU — instrumento proposto.** Chamadas dinâmicas, `setattr` com nome calculado e resolução completa de tipos são **[não verificado]** por esse instrumento.

```python
import ast
from pathlib import Path

APP = Path("app")
CAMPOS = {"status_local", "via_encerramento", "fechado_em"}

LEITORES = {
    "ponto/services/lastro.py":
        "marcos_inexistentes marcos_acesos julgar",
    "chamados/models.py":
        "_decisao_do_fechamento _gate_premissa_do_fechamento "
        "premissa_regularizada",
    "chamados/reconciliador.py":
        "turnos_abertos_de turno_do_chamado_aberto _tem_turno_aberto "
        "_par_integro _ancora_ausente _filhos_vivos _veredito_premissa "
        "_absolvido_pela_celula veredito_orfa",
    "chamados/services/silencio_ausencia.py":
        "_premissa_morre reconciliar_por_ausencia",
    "chamados/services/feriado_reavaliar.py": "espurios",
    "ponto/services/cartorio.py": "_protesta",
    "ponto/services/cluster_espurio.py":
        "_leitura_da_celula _vivos_que_a_premissa_matou",
    "chamados/juizes.py":
        "_celula_furada _pergunta_materializada_de_fato "
        "_pergunta_viva_calc via_pelo_fato",
}

def producao(p):
    return not (
        "migrations" in p.parts
        or any(x == "tests" or x == "tests.py"
               or x.startswith(("test_", "tests_")) for x in p.parts)
    )

def strings(n):
    return {
        x.value for x in ast.walk(n)
        if isinstance(x, ast.Constant) and isinstance(x.value, str)
    }

registro = ast.parse((APP / "core/juizes.py").read_text())
jui = next(
    n.value for n in registro.body
    if isinstance(n, ast.Assign)
    and any(isinstance(t, ast.Name) and t.id == "JUIZES"
            for t in n.targets)
)
JUIZES = {s for s in strings(jui) if "::" in s}

for p in sorted(APP.rglob("*.py")):
    if not producao(p):
        continue
    tree = ast.parse(p.read_text())
    pais = {c: n for n in ast.walk(tree)
            for c in ast.iter_child_nodes(n)}

    def funcao(n):
        while n in pais:
            n = pais[n]
 

[truncado em 20000 de 46357 caracteres]

## 13/11 18:00 · sessao 751ef6d6ed02 · O168 · verificador · origem=Code

**INFERIU: o BLOQ 1 específico foi curado; a igualdade geral entre CSF e retratação não foi estabelecida, porque os universos continuam diferentes.** Os dois ramos do CSF agora ignoram a tranca, como a retratação. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/juizes.py:2033–2035`)

1. **CSF × retratação**

   **LEU:** o predicado comum exige ausência de validação e materialização, mais `respondida_em` preenchido **OU** texto não vazio. Com `ignorar_tranca=True`, não consulta a tranca. (`app/chamados/models.py:1898–1902`)

   **INFERIU:** para o mesmo chamado e o mesmo estado lido, os conjuntos são:

   | Pergunta pendente | CSF segura? | `irmas_respondidas_sem_consumo` segura? |
   |---|---:|---:|
   | Do pai, destinada a outro chamado **vivo** — portanto não creditada ao pai | Sim | Não |
   | Do pai, com destino nulo ou não vivo | Sim | Sim |
   | Destinada ao chamado, embora pertença a outro pai | Sim | Sim |

   A tabela decorre do CSF ler **pai inteiro ∪ destino**, enquanto a retratação lê **destino ∪ pai sem outro destino vivo**. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/juizes.py:2033–2035`)

   **INFERIU:** a retratação é subconjunto do CSF: desaparece “CSF libera/retratação segura” por essa diferença de tranca; permanece “CSF segura/retratação libera” no primeiro caso da tabela. Isso compara as decisões sobre perguntas, sem garantir igualdade com todas as demais condições do executor. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/reconciliador.py:438–455`)

2. **C7 item 8, leitores e saída da fila**

   **LEU:** a isenção permanece no comportamento default do juiz e no C6: pergunta factual de dia trancado deixa de bloquear por leitura, sem receber carimbo. (`app/chamados/models.py:1896–1902`; `app/chamados/juizes.py:1403–1409`)

   **LEU/INFERIU:** estes são os leitores diretos encontrados e o efeito do delta:

   | Leitor | Regime atual | Mudança direta |
   |---|---|---|
   | `pode_fechar` e `fechar` | Default: respeitam a isenção | Nenhuma. (`app/chamados/models.py:1909`, `:1942`) |
   | Desligamento | Default | Nenhuma. (`app/colaboradores/services/desligamento.py:141–143`) |
   | Ficha do colaborador | Default, intersectado com perguntas do app | Nenhuma. (`app/colaboradores/services/ficha.py:166–171`) |
   | Gaveta `respondido_a_decidir` | Default | Nenhuma. (`app/chamados/services/fila_por_causa.py:225–227`) |
   | Comando `validar_classe_a` | Default | Nenhuma. (`app/chamados/management/commands/validar_classe_a.py:44–55`) |
   | Reconciliador de órfãs | Default | Nenhuma. (`app/chamados/reconciliador.py:1164–1173`) |
   | Medição da tranca/Pauta DP | Já usava `True` | Nenhuma. (`app/chamados/services/competencia_trancada.py:124–139`) |
   | Retratação | Já usava `True` | Nenhuma. (`app/chamados/juizes.py:2033–2035`) |
   | CSF, pai e destino | Agora usa `True` | Passa a segurar resposta pendente mesmo quando a tranca a isentaria. (`app/ponto/supra_juiz.py:573–574`) |

   **INFERIU:** existe mudança **indireta** na Defesa 1: a disputa pode fechar pela isenção C7, mas o pai permanecer vivo. O signal tenta `ch.reconciliar`; esse caminho passa pelo gate de conversa, que agora segura a resposta trancada. (`app/chamados/signals.py:43–46`; `app/chamados/models.py:686`, `:719–740`)

   **LEU:** a saída existente é `competencia_trancada.encerrar`: seleciona chamados elegíveis da competência, sem perguntas vivas, e chama `encerrar_por_competencia_trancada`. Essa porta consulta `_gate_pergunta_viva`, **sem consultar o gate de conversa**, e grava a via `competencia_trancada`. (`app/chamados/services/competencia_trancada.py:101–118`, `:160–171`, `:206–220`; `app/chamados/models.py:1669–1686`)

   **LEU:** Pauta DP não encerra o chamado: registra as perguntas para decisão no calendário, depois do processamento dos encerramentos. Existe retorno anterior à Pauta quando `acoes == 0`. (`app/chamados/services/competencia_trancada.py:190–193`, `:232–257`)

   **INFERIU:** há caminho de saída, mas **não há garantia de saída automática para tranca já viva, sem novo ato**. O disparo ocorre somente ao criar/retrancar; o comando do passivo não tem cron. Portanto o pai pode continuar retido até uma execução dessa via ou outro ato aplicável. (`app/ponto/services/fechamento.py:1411–1428`; `app/config/crons.py:1276–1279`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`)

   **LEU, anexo:** a frota v8 informa quatro `liberado__conversa_pendente`; ela não demonstra o destino desses quatro na v9, nem identifica um caso sem saída automática. **[não verificado]**

3. **Selo de custo**

   **LEU:** o novo caso deixa **todas** as N perguntas pendentes e compara a quantidade de queries com 1 e 3 disputas, chamando o gate real em `registrar=False`. Ele cobre o buraco da fixture anterior, que deixava apenas uma pendente. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47–58`, `:69–75`)

   **LEU:** `_perguntas_do_pai` não tem `select_related` de disputa/colaborador. **INFERIU:** isso não produz N+1 neste caminho atual: `ignorar_tranca=True` interrompe o `or` antes de `dia_da_pergunta_trancado`, e o predicado só acessa colunas da pergunta. O acesso às relações estaria no leitor da tranca. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/models.py:1898–1902`; `app/chamados/juizes.py:991–992`)

   **LEU/INFERIU:** a nota também não relê cada pergunta: recebe `_pend` já calculado e formata seus PKs. Contudo, o selo novo não mede a escrita da nota, pois usa `registrar=False`; prova constância de queries, não custo constante de processamento ou tamanho da lista. (`app/chamados/models.py:719`, `:735–739`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:56–61`)

   Quantidades SQL efetivamente executadas em Django nesta verificação: **[não verificado]**.

4. **RED e mutantes de um único universo**

   **INFERIU:** ambos os mutantes são pegos pelo **mesmo `assertEqual` da linha 307**, em iterações diferentes:

   | Mutante | Primeira falha |
   |---|---|
   | Retirar o kwarg do universo **destino**, `supra_juiz.py:574` | Linha 307, `quem='destino'` |
   | Retirar o kwarg do universo **pai**, `supra_juiz.py:573` | Destino passa; linha 307, `quem='pai'` |

   A fixture distingue pai e destino, simula tranca verdadeira e exige `[p.pk]` em ambos. O `assertFalse` da linha 310 não chega a executar na iteração que já falhou na linha 307. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:44–57`, `:300–310`)

   **LEU:** o RED consulta `irmas_respondidas_sem_consumo(destino)`, mas não consulta `irmas_respondidas_sem_consumo(pai)`; portanto não certifica igualdade entre os universos. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304–310`)

   Confirmei os dois resultados em ensaio em memória com os corpos extraídos; execução do `TestCase` Django e dos mutantes contra ORM: **[não verificado]**.

5. **Quem escreve e quem lê os campos**

   **LEU:** o delta altera a opção de leitura da tranca no CSF; o predicado continua lendo os quatro campos abaixo. (`app/ponto/supra_juiz.py:573–574`; `app/chamados/models.py:1898–1902`)

   | Campo | Escritores e reversões | Leitores relevantes |
   |---|---|---|
   | `resposta_colab` | Resposta humana; automática sem sobrescrever texto; reabertura limpa. (`app/chamados/services/disputa_emissao.py:1467–1469`, `:1647–1656`, `:1593`) | Juiz O168, C6 e conteúdo do plantio. (`app/chamados/models.py:1900–1901`; `app/chamados/juizes.py:1417`; `app/chamados/services/materializacao.py:412–419`) |
   | `respondida_em` | Resposta humana/automática; reparo pelo instante testemunhado na trilha; `carimbar` preenche nulo com texto; reabertura limpa. (`app/chamados/services/disputa_emissao.py:1462–1468`, `:1652–1653`, `:1661–1678`, `:1593`; `app/chamados/models.py:3186–3189`) | O168 lê nulidade; validação exige preenchido; materializador também. (`app/chamados/models.py:1900`; `app/chamados/services/validacao.py:416–418`; `app/chamados/services/materializacao.py:1375–1378`) |
   | `validada_em` | `_carimbar_validacao`; preenchimento por `carimbar(resposta='validar')`; nova resposta, recusa e reabertura desfazem. (`app/chamados/services/validacao.py:150–164`, `:218–228`; `app/chamados/models.py:3190–3193`; `app/chamados/services/disputa_emissao.py:1480–1482`, `:1593`) | O168 exclui; C6 encerra; materializador seleciona. (`app/chamados/models.py:1899`; `app/chamados/juizes.py:1400–1402`; `app/chamados/services/materializacao.py:1375–1378`) |
   | `materializada_em` | Atribuição central `_escrever_carimbo`, usada pelas portas normal/identidade; descarimbo avulso/lote limpa. (`app/chamados/models.py:3153–3157`, `:3180–3208`, `:3224–3246`, `:3259–3268`, `:3307–3322`) | O168 exclui; C6 lê junto da via; fila de materialização exige nulo. (`app/chamados/models.py:1899`; `app/chamados/juizes.py:1385–1395`, `:1412–1413`; `app/chamados/services/materializacao.py:1375–1378`) |
   | Tranca | `trancar_periodo` cria/retranca; `reabrir_periodo` preenche `reaberto_em`. (`app/ponto/services/fechamento.py:1400–1419`, `:1432–1441`) | Juiz do período lê `reaberto_em IS NULL`; juiz da pergunta combina competência e ausência de toque posterior; C6 e juiz default leem essa resposta. (`app/ponto/services/fechamento.py:1464–1480`; `app/chamados/juizes.py:1062–1079`, `:1237–1251`, `:1405`; `app/chamados/models.py:1902`) |

   **LEU:** os carimbos são instantes dos atos, não o dia discutido. A pergunta factual recebe dia do turno; conversa recebe dia civil do pedido. O leitor da tranca devolve “sem dia de turno” para conversa e usa `data_evento` para fato. (`app/chamados/services/disputa_emissao.py:1462–1468`; `app/chamados/services/validacao.py:157`; `app/chamados/models.py:3180`, `:2840–2858`; `app/chamados/juizes.py:334–338`, `:989–992`)

   **INFERIU:** não há vocabulário uniforme: O168 aceita **carimbo OU texto**; validar exige carimbo; a cláusula de resposta do C6 exige **carimbo E texto**. Texto sem `respondida_em` pode segurar o fechamento e receber `sem_resposta` na validação. Há reparo pela trilha, cuja disponibilidade nos casos de produção é **[não verificado]**. (`app/chamados/models.py:1900–1901`; `app/chamados/services/validacao.py:416–418`; `app/chamados/juizes.py:1417`; `app/chamados/services/disputa_emissao.py:1661–1678`)

   **INFERIU:** `materializada_em` também não prova batida criada: a porta de identidade atende admin/dedup/colisão, embora o help text prometa “batida criada + chamado fechado”. (`app/chamados/models.py:3224–3246`, `:3449–3451`)

6. **LEI-AKITA, commit e dinheiro** [nao verificado]

   A tentativa de `git -C` na cópia retornou “not a git repository”. A mensagem de e050e522f e sua linha LEI-AKITA permanecem **[não verificado]**; a afirmação da triagem sobre oito commits não substitui essa conferência. **[não verificado]**

   **LEU:** a lei exige origem, testemunha, RED, censo, juízes novos e padrão; exige também conferir se o padrão nomeado corresponde ao diff. **INFERIU:** o delta mantém o predicado compartilhado e muda a política de tranca dos dois leitores CSF, acrescentando os dois selos; não demonstra igualdade de universos nem cria outra autoridade. O juiz existente está registrado. (`CLAUDE.md:62–75`; `app/ponto/supra_juiz.py:573–574`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:69–75`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:291–310`; `app/core/juizes.py:294–295`)

   **INFERIU:** o impacto demonstrável é na retenção de chamados e na cascata de fechamento. Não há escrita monetária nessas duas chamadas alteradas, mas isso não certifica impacto financeiro zero: a retratação também controla resolução de perguntas e rejeição automática de justificativa pendente. O valor financeiro efetivo, por pessoa/competência/rubrica, permanece **[não verificado]**. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/models.py:719–740`; `app/chamados/reconciliador.py:519–537`)

## 13/11 17:59 · sessao 1c27186a31ab · - · - · origem=Ronald (tmux)

LEU: as exceções estruturais já estão vazias: `app/core/juizes.py:470` contém `PENDENTES['chamado'] = ()`; alias em `:1366`; `app/config/crons.py:2035` a `:2100` contém somente comentários, nenhum elemento da tupla. A célula permanece declarada falsa porque omite `verde=True`, cujo default é `False` (`app/core/contratos_estruturais.py:62`, `:262`); a nota ainda exige O168/O169/LASTRO (`:270`). Não há conteúdo residual nessas duas listas. O teste cobra pendentes ≤0 (`app/chamados/tests/test_contract_juiz_chamado.py:85`, `:98`).

LEU: `lastro.fechar` não existe no HEAD; quem escreve pelo lastro é `fechar_pelo_marco_colab` (`app/ponto/services/lastro.py:364`). O cron é vigia, aceita `--alarme`, mede uma vez e sai 2 se houver escape (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:44`, `:53`, `:77`); configuração `--alarme` em `app/config/crons.py:731`.

O censo lido identifica estes decisores:

| Sítio | Pergunta registrada ou deriva |
|---|---|
| `app/ponto/services/lastro.py:98`, `marcos_inexistentes` | Deriva: todos os marcos cobrados ausentes de ata com lâmpadas. Não consta em `JUIZES` (`app/core/juizes.py:276`). |
| `app/ponto/services/lastro.py:153`, `marcos_acesos` | Deriva: todos os marcos nomeados acesos; `None` sem marco/tipo/lâmpada; não registrado (`app/core/juizes.py:276`). |
| `app/ponto/services/lastro.py:196`, `julgar` | Lê lâmpada em `:289`; sem ela deriva casamento linear de tipo e distância ≤90 min em `:304`. Pergunta `reconciliaria` em `:318`. |
| `app/chamados/models.py:1253`, `premissa_regularizada` | Coletor misto: justificativa aprovada `:1267`, admin `:1283`, C18 `dia_sem_trabalho` `:1285`, `marcos_acesos` `:1289`, `marcos_inexistentes` `:1298`, `_celula_furada` `:1300`, A2/C3 via `espurios` `:1306`, C3 `avaliar` `:1315`. |
| `app/chamados/models.py:718`, `_gate_premissa_do_fechamento` | Delega ao coletor `:739`; acrescenta dedup C8 `:749`; sem dia permite fechar com trilha `:733`. |
| `app/chamados/models.py:664`, `_decisao_do_fechamento` | Motor registrado `:674`; combina gate premissa `:683` e pergunta viva `:686`. |
| `app/chamados/reconciliador.py:83`, `turnos_abertos_de` | Deriva query entrada presente/saída ausente `:97`; veto por `dia_posicional_completo` `:113`. |
| `app/chamados/reconciliador.py:118`, `turno_do_chamado_aberto` | Delega ao anterior `:143`; recorta entrada declarada `:146` ou data_turno `:156`. |
| `app/chamados/reconciliador.py:162`, `_tem_turno_aberto` | Delega com dia `:173`; sem dia deriva `.exists()` global `:174`. |
| `app/chamados/reconciliador.py:182`, `_par_integro` | Deriva identificação por hora única `:213`, retratação `:217` e tipos alternados `:222`. |
| `app/chamados/reconciliador.py:227`, `_ancora_ausente` | Deriva vínculo ativo sem âncora `:233`. |
| `app/chamados/reconciliador.py:239`, `_filhos_vivos` | Lê VIVOS registrados `:247`; sem IDs devolve desconhecido `:244`. |
| `app/chamados/reconciliador.py:248`, `_veredito_premissa` | Delega à implementação da C3, `avaliar`, em `:258` e `:302`; coleta turno `:288`, âncora `:292`, filhos `:294`, par `:296`, geofence `:301`. Falta/cluster/intervalo não são coletados aqui: ficam desconhecidos pela C3 (`app/chamados/catalogo/premissa.py:127`). |
| `app/chamados/reconciliador.py:1019`, `_absolvido_pela_celula` | Deriva veredito não acusador `:1028` e ausência de marcos pendentes `:1030`. |
| `app/chamados/reconciliador.py:1061`, `veredito_orfa` | Compõe fato do marco `:1085`, C6 `:1087`, teto civil `:1089` e dia encerrado `:1091`. |
| `app/chamados/services/silencio_ausencia.py:29`, `_premissa_morre` | Chama C3 com três fatos falsos `:37`; deriva veredito não acusador `:41`. `None` também satisfaz o teste `:42`. |
| `app/chamados/services/silencio_ausencia.py:52`, `reconciliar_por_ausencia` | Autoridade registrada C11 (`app/core/juizes.py:287`); pergunta cobertura `:66`; aprovada passa pelo anterior `:94`. |
| `app/chamados/services/feriado_reavaliar.py:24`, `espurios` | Pergunta A2 `:34`, deriva faixa e previsto `:35`, e pergunta C3 `:39`. |
| `app/ponto/services/cartorio.py:1458`, `_protesta` | Deriva `trabalha is not False` e lê `marcos_acesos` `:1459`. Exceção declarada à segunda cláusula do C18 em `app/core/juizes.py:145`. |
| `app/ponto/services/cluster_espurio.py:15`, `_leitura_da_celula` | C18 `:25`; depois deriva assinatura ≥2 órfãs em 15 min e ≥1 apagada `:31`. |
| `app/ponto/services/cluster_espurio.py:53`, `_vivos_que_a_premissa_matou` | Lê próprio dia sem limite de idade `:68`; preserva desconhecido `:75`; pergunta C3 `:76`. |
| `app/chamados/juizes.py:459`, `_celula_furada` | Pergunta cobertura `:476` e C18 `:494`; deriva apagadas `:508` e órfãs `:510`. |
| `app/chamados/juizes.py:662`, `_pergunta_materializada_de_fato` | Implementação interna do C6: vias `:679`, plantio `:755`, destino encerrado `:780`, lei do dia `:790`. |
| `app/chamados/juizes.py:1399`, `_pergunta_viva_calc` | Implementação do juiz registrado C6; tranca C20 `:1405`. |
| `app/chamados/juizes.py:1715`, `via_pelo_fato` | Classifica via; consulta cobertura em `:1756`. |

“Sem juiz registrado direto” não equivale automaticamente a duplicação: `avaliar` é a implementação chamada pelo juiz registrado `premissa_morta` (`app/chamados/catalogo/premissa.py:149`); os auxiliares do C6 pertencem ao corpo de sua autoridade (`app/core/juizes.py:282`). Isso precisa aparecer na interpretação do AST.

INFERIU: a função existente mais próxima da autoridade ampla do lastro é `ChamadoColaborador.premissa_regularizada`, pois fechar e reabrir já compartilham sua resposta (`app/chamados/models.py:1254`, `:1261`, `:739`, `:1350`). A autoridade da transição é `_decisao_do_fechamento`, que já combina motor, premissa e fio (`app/chamados/models.py:674`, `:683`, `:686`). Falta ao coletor retorno rico, desconhecido explícito, fonte/prova, relógio/carga explícitos e alcance às classes coletadas separadamente pelo reconciliador (`app/chamados/models.py:1253`, `:1317`; `app/chamados/reconciliador.py:263`).

INFERIU: contrato proposto pode separar parecer puro e aplicação. Parecer com identidade do chamado/fato/dia/marcos, resultado `sustentado | regularizado | desconhecido`, fundamento e evidências; plano da transição com destino/via/recusa/perguntas a reatar. A aplicação continua na porta existente; o consumidor e vigia leem o mesmo parecer. Bases existentes: tríade da C3 (`app/chamados/catalogo/premissa.py:37`, `:124`), tríade dos marcos (`app/ponto/services/lastro.py:154`), plano da pergunta viva (`app/chamados/models.py:374`) e decisão comum dry/apply (`app/chamados/models.py:658`).

Provas existentes, lidas sem execução: fecho e nota no evento (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:97`), segunda passada sem nota extra (`:112`), marco apagado (`:122`), todos ou nenhum (`:130`), recusa sem nota (`:137`), falha de nota desfaz fecho (`:152`), degrau 2 continua escape mesmo com evento (`:234`), cron vigia (`:275`). A2 mede estado/nota/contador, não conta explicitamente `LogAuditoria.estado_chamado` (`:117`–`:120`).

Pontos a confrontar nas perguntas de lei, sem inventar regra:

- Desconhecido: C3 conserva fato não coletado (`app/chamados/catalogo/premissa.py:127`); gate sem dia fecha com trilha (`app/chamados/models.py:733`). São dois decisores com tratamentos distintos de ignorância.
- Lâmpada `acesa=None`: `marcos_acesos` rejeita (`app/ponto/services/lastro.py:179`); `_celula_furada` só prende por `is False` e pode regularizar (`app/chamados/juizes.py:507`); reconciliador exige zero pendentes (`app/chamados/reconciliador.py:1030`). Três sítios.
- Degrau 2 linear versus papel do marco é dívida já decidida: juiz do par registrado (`app/core/juizes.py:211`), regra posicional L-115 (`app/docs/LEIS.md:151`), conta própria ainda em `app/ponto/services/lastro.py:304`. Não contar como lei nova aberta.

## 13/11 17:58 · sessao 998bf5247530 · - · - · origem=Ronald (tmux)

LEU = código/documento da cópia. INFERIU = comparação dos anexos com esse código. Os diffs anexos não têm arquivo local verificável; suas mudanças são “LEU anexo; [não verificado no HEAD]” até a comparação abaixo.

**Resultado medido da comparação.** Calculei em memória o SHA1 no formato de blob Git dos 21 arquivos de produção dos diffs 164050/215253: 20 são byte a byte iguais ao lado anterior de pelo menos um dos diffs. A exceção é `app/folha/export.py`, cujo arquivo diverge, mas o hunk de `prontidao` continua igual (`app/folha/export.py:1095`). O HEAD de `ausencia.py` e `cartorio.py` coincide com a base da 164050, enquanto a 215253 os tinha rebasedos em bases diferentes. Não foi realizado merge ou apply; conflito mecânico de merge é [não verificado].

| Raia | O que resolve no anexo | Situação lida no HEAD | Sobreposição/conflito |
|---|---|---|---|
| `lastro3` rescaldo | Troca recuo linear pelo juiz do par; extrai compositor `grade_do_julgamento`; exige papel/cadeia, trata noturno e final versus intervalo; elimina tolerância local. | Pendência continua: `TOLERANCIA_MIN` em `lastro.py:59`; recuo próprio em `:299`, comparação por tipo/instante linear em `:307`; grade montada inline no cartório em `cartorio.py:1156` e `:1175`. | **Parte superada:** o diff altera `fechar()` e `--apply`, que não são a porta atual. Hoje comando é vigia (`commands/fechar_cobranca_com_lastro.py:20`, `:44`) e escritor por evento é `lastro.py:364`. Builder exige `agora=` (`escala/utils.py:2084`), ausente no compositor do diff antigo. Não transplantar literalmente: INFERIU destas referências. |
| `204847`, c3 | Mapa explícito, `TipoSemFaixa`, contador de tipos ativos sem faixa. | Continua o default `'atestado'` em `precedencia.py:76`; mapa pequeno em `:52`; contador não aparece na lista que começa em `vigia_ausencia.py:154`. | É a mesma raiz c3 incluída no histórico 215253; aplicar duas vezes seria duplicar mudança (LEU anexo; INFERIU). |
| `164050`, c3–c12 | Além da c3: separa cobertura de rótulo, preserva efeito histórico de inativos, transporta campos pela ata, recusa nomeada em leitores e cadastro, isola colaborador no cartório; recusa fica em buraco. | Os sítios correspondentes continuam com o comportamento anterior, detalhados abaixo. | É base documental escolhida pelo redesenho, `BACKLOG.md:1837`. Sobrepõe quase toda a 215253 até c12; não são duas obras independentes (LEU anexo; INFERIU). |
| `215253`, c3–c19 | Inclui c3–c12; depois remove montagem própria de grade no PDF, conserva recusa no builder, põe instante com fuso em órfãs desde o juiz, transporta ata→tela→PDF, identifica órfã por instante nos três consumidores e na reconciliação. | Continua só HH:MM no produtor (`escala/utils.py:326`), na ata (`cartorio.py:745`), no leitor (`leitor_celula.py:187`), no PDF (`pdf_espelho.py:174`, `:472`, `:609`, `:716`, `:960`). | Diverge da 164050 justamente nos consumidores posteriores e formato. **Documento mais novo interrompe este caminho:** `BACKLOG.md:1833` diz “orfas_instantes sai” e obra parada; `:1837` diz c20 encerrada, voltar à base 164050 e construir após acessor. `:2089` proíbe formato e leitor no mesmo pouso. Conflito de desenho/ordem LEU; aptidão de merge [não verificado]. |
| `lastro-folga`, c2a–c2c | Guarda `Ausencia.save`: recusa introduzir tipo fora do catálogo vivo; cache por schema; portas traduzem recusa; `update_fields` sem tipo não pergunta. | `Ausencia.save` começa por carimbar decisão e termina em `super().save`, sem guarda de tipo (`ponto/models.py:868`, `:908`). Cache do catálogo é único no processo (`catalogo/ausencias.py:257`). | Complementa guarda de faixa, mas não a substitui: “tipo no catálogo” e “tipo tem faixa” são perguntas distintas; guarda de faixa no cadastro também não existe (`services/ausencia.py:1302`). Conflito textual com FAIXA no catálogo/cache e portas de ausência; precisa composição, não sobrepor versões (INFERIU). Diff integral dessa raia não foi fornecido; merge exato [não verificado]. |

**Censo dos sítios dos diffs FAIXA que ainda estão antes da mudança.** Todos os caminhos abaixo têm prefixo `app/`; são LEU.

| Sítio HEAD | Antes ainda observado / mudança correspondente nos anexos |
|---|---|
| `ponto/precedencia.py:52`, `:76`, `:259`, `:273`, `:381` | Mapa/default atestado; sem canal de recusas; universo exclui rejeitada, sem exceção `troca_de_plantao`; acumula faixa sem recusa. |
| `ponto/catalogo/ausencias.py:240`, `:249`, `:257`, `:267`, `:293` | Só ativos, cache único, efeito de existente usa o mesmo catálogo ativo. |
| `ponto/services/ausencia.py:1302` | Cadastro copia campos/salva sem recusa de código/sem faixa. |
| `ponto/views_tipos_ausencia.py:82`, `:86`, `:94` | Chama cadastro sem tratar `AusenciaInvalida`. |
| `ponto/services/vigia_ausencia.py:154`; `pautas/semanal.py:55` | Lista e rótulos sem `tipos_ativos_sem_faixa`. |
| `escala/utils.py:1156`, `:1202`, `:1220` | Overlay não chama A2, rotula todo não-falta como `ausencia`; não transporta recusa. |
| `ponto/services/cartorio.py:730`, `:766`, `:772`, `:1244` | Ata transporta parcial, não os três campos `ausencia_tipo/id/label`; cobertura inteira usa rótulo ausencia/falta. |
| `escala/services/leitor_celula.py:429`, `:433` | Transporta parcial, não campos de cobertura. |
| `ponto/services/dia_pago.py:387`, `:395` | Universo trabalho/ausencia; abono condicionado ao rótulo. |
| `folha/export.py:1095` | Prontidão ainda deriva seus mapas inline; é o único arquivo cujo blob inteiro difere da base dos anexos. |
| `ponto/services/fechamento.py:1046`, `:1081`, `:1087`, `:1303` | Universo pelo rótulo e exceções genéricas; nenhuma propagação específica de `TipoSemFaixa`. |
| `ponto/management/commands/processar_cartorio.py:65`; `cartorio.py:1954` | Sem isolamento/contador por colaborador recusado. |
| `ponto/status_dia.py:221`, `:298`, `:301`, `:307` | Só fatos, sem recusas. |
| `colaboradores/services/calendario.py:499`, `:606`, `:661`, `:715` | Só fatos; chama `do_dia` diretamente. |
| `escala/services/semana_do_posto.py:151`, `:168`, `:178` | Só explicado/buraco; exceção genérica vira trabalho. |
| `ferias/situacao.py:72`, `:102`, `:246`, `:320` | Sem canal de aviso por tipo sem faixa. |
| `ponto/services/dia_decidido.py:371` | Reconhece somente `tipo == 'folga'`, sem `folga_da_escala`. |
| `ponto/services/dia_do_colab.py:15` | Consulta A2 avulsa, sem representação da recusa. |
| `ponto/services/espelho.py:297`, `:489`, `:644`, `:648` | Sem recusa do builder/da A2; sem publicação de instantes das órfãs. |
| `relatorios/pdf_espelho.py:475`, `:543`, `:598` | Faz duas perguntas à grade antes de consultar a tela. |
| `escala/utils.py:1650`, `:1675`, `:2005`, `:2039` | Dry-run e reconciliação de órfã continuam identificando/removendo por HH:MM. |
| `escala/utils.py:1084` | Adaptador perde as órfãs quando não há marcos. |

**Pendências que os anexos não resolvem, comprovadas no HEAD.**

- Dependência da grade da janela continua registrada como aberta, não curada: `BACKLOG.md:1066`. O rescaldo escolhe janela unitária, mas isso não prova invariância do construtor (INFERIU; `cartorio.py:1156`, `:1175`).
- Cartório tem outro casamento próprio: soma horas de colunas e órfãs (`cartorio.py:627`), quando falta coluna casa por proximidade linear ≤30 min (`:666`) e acende (`:672`); ata continua gravando órfãs (`:745`). A própria documentação chama isso de segundo juiz (`BACKLOG.md:2091`) e o comentário descreve a invariante disjunta/teste (`cartorio.py:682`, `:702`). Nenhuma das cópias fornecidas remove esse ramo (LEU anexo; INFERIU). [nao verificado]
- `como_acendeu` ainda substitui `luz` faltante pela hora prevista (`lastro.py:340`); backlog afirma que não procede por ausência de escritor produtivo (`BACKLOG.md:1787`), mas uma sonda de produção atual não está disponível: [não verificado].
- As provas c3–c19 são listadas nos anexos, mas os diffs de testes não foram fornecidos para as raias FAIXA e não estão na árvore; que passam/mordem nesta cópia é [não verificado]. [nao verificado]
- Acessor tipado está apenas no plano/fila (`BACKLOG.md:1833`, `:2088`); arquivo `escala/services/ata.py` ausente nesta cópia: existência de implementação pousada [não verificado].

**Perguntas de lei: separar lacuna de regra já respondida.**

1. **Efeito de tipo desconhecido é lacuna candidata, 4 sítios.** Fecha suprimindo (`fechamento.py:698`–`:701`), coletor não o inclui em nenhum efeito (`efeito_ausencia.py:57`–`:62`), visual chama ABONADO (`dia_decidido.py:468`–`:473`), precedência chama atestado (`precedencia.py:76`). A divergência está registrada em `BACKLOG.md:1812`; regra única explícita para `efeito=None` não localizada [não verificado]. Não equiparar faixa de precedência a efeito financeiro: as perguntas são diferentes (`precedencia.py:45`; `dia_pago.py:372`).
2. **Não contar como nova pergunta** raio versus juiz do par: a ordem de substituição está expressa e a opção raio foi superada (`BACKLOG.md:1756`).
3. **Não contar como nova pergunta** cluster morrer por idade: já decidiu L-125 (`LEIS.md:146`; `CORTES.md:430`).
4. **Não contar como conflito vigente** reconstruir passado versus FOTO: L-124 declara a revogação, preserva fato novo e exportada (`LEIS.md:145`; `CORTES.md:419`). O redesenho fixa ausência de bump na cópia, mas prova de que a nova regra atua só no horizonte é [não verificado].
5. **Não inventar decisão** sobre quantas das demais divergências são “perguntas de lei”: proximidade órfã/acesa é violação potencial de juiz único/invariante já escrita (`cartorio.py:691`, `:702`; `LEIS.md:60`), não demonstra por si duas leis conflitantes.

## 13/11 17:57 · sessao e38e884bd20f · - · - · origem=Ronald (tmux)

Q2 — leitura estática, sem executar nem escrever.

**LEU:** a isenção C7 item 8 permanece no predicado default: respostas pendentes de dia trancado são excluídas por `not dia_da_pergunta_trancado(q)`; `ignorar_tranca=True` desativa essa exclusão. O delta muda somente os dois leitores CSF para `True`. [app/chamados/models.py:1889](app/chamados/models.py:1889), [app/chamados/models.py:1902](app/chamados/models.py:1902), [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/ponto/supra_juiz.py:574](app/ponto/supra_juiz.py:574).

**LEU — leitores diretos e efeito:**

| Leitor | Regime atual | Efeito do delta |
|---|---|---|
| `DisputaSupervisao.pode_fechar` | Default, preserva C7; exige também ausência de fio vivo | Nenhuma mudança direta. [app/chamados/models.py:1909](app/chamados/models.py:1909) |
| `DisputaSupervisao.fechar` | Default na recusa de resposta sem veredito | Nenhuma mudança direta; fechamento da disputa pode ocorrer em dia trancado. [app/chamados/models.py:1942](app/chamados/models.py:1942) |
| Desligamento | Default antes de fechar disputa | Nenhuma mudança direta. [app/colaboradores/services/desligamento.py:141](app/colaboradores/services/desligamento.py:141) |
| Ficha do colaborador | Default, intersectado com perguntas do app | Nenhuma mudança direta. [app/colaboradores/services/ficha.py:169](app/colaboradores/services/ficha.py:169) |
| Gaveta “respondido_a_decidir” | Default | Nenhuma mudança direta. [app/chamados/services/fila_por_causa.py:226](app/chamados/services/fila_por_causa.py:226) |
| Reconciliador de órfãs | Default antes de `pode_fechar/fechar` | Nenhuma mudança direta. [app/chamados/reconciliador.py:1164](app/chamados/reconciliador.py:1164), [app/chamados/reconciliador.py:1171](app/chamados/reconciliador.py:1171) |
| Comando `validar_classe_a` | Default, a partir da gaveta | Nenhuma mudança direta. [app/chamados/management/commands/validar_classe_a.py:44](app/chamados/management/commands/validar_classe_a.py:44), [app/chamados/management/commands/validar_classe_a.py:54](app/chamados/management/commands/validar_classe_a.py:54) |
| Medição da tranca/Pauta DP | `ignorar_tranca=True` | Já ignorava a isenção; nenhuma mudança direta. [app/chamados/services/competencia_trancada.py:126](app/chamados/services/competencia_trancada.py:126) |
| Retratação `irmas_respondidas_sem_consumo` | `ignorar_tranca=True` | Já ignorava a isenção; nenhuma mudança direta. [app/chamados/juizes.py:2035](app/chamados/juizes.py:2035) |
| `perguntas_que_seguram_csf` | `True` nos universos pai e destino | Passa a segurar também resposta sem veredito cujo dia foi calado pela tranca. [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/ponto/supra_juiz.py:574](app/ponto/supra_juiz.py:574) |

**LEU:** o C6 também preserva C7: antes de considerar resposta, `dia_da_pergunta_trancado` torna a pergunta morta. O juiz real exige dia de turno em competência trancada e nenhum toque posterior do admin; conversa não tem competência de fato. [app/chamados/juizes.py:1403](app/chamados/juizes.py:1403), [app/chamados/juizes.py:974](app/chamados/juizes.py:974), [app/chamados/juizes.py:334](app/chamados/juizes.py:334), [app/chamados/juizes.py:1078](app/chamados/juizes.py:1078).

**INFERIU:** a disputa pode fechar normalmente por C7 enquanto seu pai permanece vivo pela guarda CSF alterada: o signal chama `reconciliar`, que passa pela guarda da conversa; essa guarda usa a lista CSF inteira. Portanto há mudança indireta no resultado da Defesa 1, embora `fechar/pode_fechar` não mudem. [app/chamados/signals.py:43](app/chamados/signals.py:43), [app/chamados/models.py:686](app/chamados/models.py:686), [app/chamados/models.py:719](app/chamados/models.py:719), [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573).

**LEU — saída existente:** `competencia_trancada.medir` seleciona chamados vivos cuja **competência do chamado** coincide com a trancada, exclui resgatados/exceções e retém os que tenham pergunta viva; os demais entram em `r['chamados']`. `encerrar` chama `encerrar_por_competencia_trancada`, cuja guarda é `_gate_pergunta_viva`, sem `_gate_conversa_do_pai`; grava `via=competencia_trancada`. Assim a nova recusa CSF não barra essa via. [app/chamados/services/competencia_trancada.py:101](app/chamados/services/competencia_trancada.py:101), [app/chamados/services/competencia_trancada.py:109](app/chamados/services/competencia_trancada.py:109), [app/chamados/services/competencia_trancada.py:160](app/chamados/services/competencia_trancada.py:160), [app/chamados/services/competencia_trancada.py:209](app/chamados/services/competencia_trancada.py:209), [app/chamados/models.py:1684](app/chamados/models.py:1684), [app/chamados/models.py:1686](app/chamados/models.py:1686).

**LEU — limite crucial:** essa saída não é cron automático para acervo já trancado: dispara somente no **ato** de criação/retranca; `ato = criado or reaberto_em is not None`. O passivo depende do comando `encerrar_por_competencia_trancada --apply`; o registro de crons declara expressamente que ele não é cron. [app/ponto/services/fechamento.py:1411](app/ponto/services/fechamento.py:1411), [app/ponto/services/fechamento.py:1420](app/ponto/services/fechamento.py:1420), [app/config/crons.py:1276](app/config/crons.py:1276), [app/chamados/management/commands/encerrar_por_competencia_trancada.py:29](app/chamados/management/commands/encerrar_por_competencia_trancada.py:29).

**LEU/INFERIU:** Pauta DP **não tira chamado da fila por si**: recebe ids e escreve texto pedindo decisão no calendário; o fechamento foi feito antes pela via `competencia_trancada`. Há inclusive retorno anterior à criação da Pauta se `r['acoes'] == 0`. Portanto “vai para Pauta DP” não prova saída automática de um pai retido agora em tranca antiga. [app/chamados/services/competencia_trancada.py:192](app/chamados/services/competencia_trancada.py:192), [app/chamados/services/competencia_trancada.py:232](app/chamados/services/competencia_trancada.py:232), [app/chamados/services/competencia_trancada.py:248](app/chamados/services/competencia_trancada.py:248), [app/chamados/services/competencia_trancada.py:256](app/chamados/services/competencia_trancada.py:256).

**INFERIU:** não há beco absoluto demonstrado — existe via de encerramento da tranca —, mas não há garantia de progresso automático sob a hipótese “tranca já viva, admin nunca toca”. A nova guarda não dispara essa limpeza; se o chamado estiver fora da competência selecionada, sem dia, resgatado ou em módulo excluído, nem essa passagem o seleciona. A competência do chamado é julgada separadamente e pode divergir da competência de uma pergunta. [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/chamados/services/competencia_trancada.py:105](app/chamados/services/competencia_trancada.py:105), [app/chamados/services/competencia_trancada.py:113](app/chamados/services/competencia_trancada.py:113), [app/chamados/services/competencia_trancada.py:161](app/chamados/services/competencia_trancada.py:161), [app/chamados/juizes.py:2352](app/chamados/juizes.py:2352). Chamado específico da frota nessa situação: **[não verificado]**.

## 13/11 17:57 · sessao d93793f5cd28 · - · - · origem=Ronald (tmux)

**5 — LEU, mapa de escrita/leitura.** O predicado usado pelos dois caminhos lê `validada_em`, `materializada_em`, `respondida_em`, `resposta_colab` e, se `ignorar_tranca=False`, a tranca; o delta muda somente a opção de leitura da tranca, sem escrever esses campos: `app/chamados/models.py:1889-1902`, `app/ponto/supra_juiz.py:562-577`.

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `resposta_colab` | Resposta humana em `responder_pergunta_disputa_core`, junto de `respondida_em`: `app/chamados/services/disputa_emissao.py:1462-1469`; resposta automática sem sobrescrever texto existente: `:1647-1656`; reabertura limpa: `:1593`. | Juiz O168 aceita texto não vazio como fala mesmo sem carimbo: `app/chamados/models.py:1898-1901`; C6 exige carimbo **e** texto na sua cláusula da resposta: `app/chamados/juizes.py:1417`; plantio usa o conteúdo como hora: `app/chamados/services/materializacao.py:412-419`. |
| `respondida_em` | Resposta humana recebe `now()`: `app/chamados/services/disputa_emissao.py:1462-1468`; automática recebe instante fornecido: `:1652-1653`; cura de limbo recebe o instante testemunhado pela trilha: `:1661-1678`; porta `carimbar(resposta=...)` preenche nulo somente com texto: `app/chamados/models.py:3186-3189`; reabertura limpa: `app/chamados/services/disputa_emissao.py:1593`; ressuscitação limpa em lote: `app/chamados/models.py:1613`. | O168 lê somente nulidade: `app/chamados/models.py:1900`; validar recusa sem ele: `app/chamados/services/validacao.py:416-418`; materialização seleciona apenas não nulo: `app/chamados/services/materializacao.py:1375-1378`; estado da pergunta o usa para distinguir pendente/respondida: `app/chamados/models.py:3097-3104`. |
| `validada_em` | Ato de validar em `_carimbar_validacao`, `timezone.now()` e linha da sequência com mesmo instante: `app/chamados/services/validacao.py:150-164`; nova resposta desfaz validação antiga: `app/chamados/services/disputa_emissao.py:1470-1482`; devolução por recusa limpa: `app/chamados/services/validacao.py:218-228`; reabertura limpa: `app/chamados/services/disputa_emissao.py:1593`; porta de simetria `carimbar(resposta='validar')` preenche nulo: `app/chamados/models.py:3186-3193`; ressuscitação limpa: `:1613`. | O168 considera qualquer não nulo julgado: `app/chamados/models.py:1899`; C6 encerra antes dos demais degraus: `app/chamados/juizes.py:1400-1402`; validar recusa repetição: `app/chamados/services/validacao.py:419-421`; materializador exige não nulo: `app/chamados/services/materializacao.py:1375-1378`. |
| `materializada_em` | Atribuição central em `_escrever_carimbo`: `app/chamados/models.py:3153-3157`; `carimbar` usa instante fornecido ou `now()` e consulta C6 antes de salvar: `:3180-3208`; identidade carimba sem consultar C6: `:3224-3246`; descarimbar avulso e lote limpam junto de autor/via: `:3259-3268`, `:3307-3322`. | O168 exclui qualquer pergunta materializada: `app/chamados/models.py:1899`; C6 lê carimbo e via: `app/chamados/juizes.py:1385-1395`, `:1412-1413`; fila de plantio exige nulo: `app/chamados/services/materializacao.py:1375-1378`. |
| Tranca (`PeriodoFechado`, `reaberto_em`, `fechado_em`) | Nascimento/retranca em `trancar_periodo`: `app/ponto/services/fechamento.py:1400-1419`; `fechado_em` nasce com `auto_now_add`: `app/ponto/models.py:926`; reabertura preenche `reaberto_em` e desativa: `app/ponto/services/fechamento.py:1432-1441`. | `periodo_trancado`/`competencia_trancada` leem existência com `reaberto_em IS NULL`; `ativo` é documental: `app/ponto/services/fechamento.py:1464-1480`. Pergunta trancada exige também ausência de toque humano posterior: `app/chamados/juizes.py:1062-1079`, `:1237-1251`. O168 atual ignora essa leitura nos dois universos CSF: `app/ponto/supra_juiz.py:575-576`; retratação também: `app/chamados/juizes.py:2033-2035`. |

**LEU — toque que muda o resultado da tranca:** o leitor distingue cobrança manual, resolução do dia e desarquivamento; resolução exige assinatura `via='admin'` ou chave `acoes`, excluindo lavra do cartório herdando usuário humano: `app/chamados/juizes.py:1083-1093`, `:1245-1251`, `:1292-1298`. Escritas localizadas: cobrança manual `app/chamados/services/cobrar_dia.py:39`; desarquivar chama `arquivo.levantar(...acao='desarquivar_chamado')` em `app/chamados/views_arquivar.py:77`; constante da via humana `app/ponto/services/veredito_celula.py:76`.

**LEU — dia do turno versus dia civil:** carimbos são instantes dos atos, não datas do turno, pelas atribuições `now()` acima. `data_evento` da conversa é dia civil do pedido (`localdate()`): `app/chamados/models.py:2850-2858`; o contrato distingue explicitamente esse dia do dia do turno da pergunta factual: `:2868-2870`. A tranca usa `_dia_de_turno_da_pergunta`, que devolve `None` para conversa e `data_evento` para fato: `app/chamados/juizes.py:334-338`, `:989-992`; calcula competência por `janela_atual(d, empresa)`, nunca pelo mês civil cru: `:1062-1066`.

**INFERIU — não há significado uniforme em todos os leitores:** o juiz O168 aceita **carimbo OU texto** (`app/chamados/models.py:1900-1901`), validar exige carimbo (`app/chamados/services/validacao.py:416-418`) e C6 exige carimbo **E texto** na cláusula de resposta (`app/chamados/juizes.py:1417`). Logo texto sem `respondida_em` pode segurar CSF/retratação e ser recusado como `sem_resposta` na validação; é divergência herdada, não alteração deste delta (mesmas referências). A cura existente para esse meio estado exige instante testemunhado pela trilha: `app/chamados/services/disputa_emissao.py:1661-1678`; disponibilidade dessa trilha para casos de produção: **[não verificado]**.

**INFERIU — `materializada_em` também tem vocabulário excessivo:** o help text promete “batida criada + chamado fechado” (`app/chamados/models.py:3449-3451`), mas a porta de identidade atende admin/dedup/colisão e grava o carimbo sem plantio obrigatório (`:3224-3246`). Portanto carimbo não demonstra dinheiro nem batida nova; operacionalmente O168 o trata como processamento suficiente para excluir a pergunta (`:1899`).

## 13/11 17:57 · sessao cb5af247cf08 · - · - · origem=Ronald (tmux)

LEU: a matriz e as provas foram enviadas ao agente principal, com estes pontos críticos:

- O chamado usa `resolvido_em`, não `fechado_em`; os campos estão em `app/chamados/models.py:52`, `:54` e `:60`.
- A escrita persistente de transição passa por `_gravar_estado`: `app/chamados/models.py:201`, `:228`, `:245`, `:249`, `:296`. O nascimento escreve por `abrir`/`criar`: `:939`, `:972`, `:1096`.
- `_vivas_entre` altera `status_local` somente em memória e restaura no `finally`: `app/chamados/models.py:356`, `:371`.
- O coletor existente mais próximo da pergunta única de lastro é `premissa_regularizada`: `app/chamados/models.py:1253`; a decisão operacional compartilhada por `reconciliar` e `reconciliaria` é `_decisao_do_fechamento`: `:664`.
- A trilha de estado é chamada antes do `save`, no mesmo atomic: `app/chamados/models.py:280`, `:292`, `:296`; entretanto, `evento` é best-effort e pode devolver `None`: `app/core/observ.py:94`, `:105`, `:133`, `:139`. INFERIU: o HEAD não garante incondicionalmente “uma trilha por estado persistido”.
- As recusas repetidas de reabertura por premissa regularizada/admin sem fato novo gravam evento a cada chamada: `app/chamados/models.py:1395`, `:1401`. O fechamento possui deduplicação específica de recusas: `:397`.
- Sem dia do fato, o gate do fechamento permite encerrar e registra sua ignorância: `app/chamados/models.py:733`; o coletor `premissa_regularizada` responde `False`: `:1272`.
- INFERIU: há alcance de lei a esclarecer entre reabertura pelo gate `reabrir`, que exige fato novo para fechamento de admin (`app/chamados/models.py:1343`), e a transição humana, cuja matriz permite `resolvido → em_analise` e override de admin TI (`app/chamados/catalogo/motor.py:102`, `:150`), aplicada sem aquela guarda (`app/chamados/models.py:199`).
- Arquivar por idade é véu, preservando estado: `app/chamados/services/arquivo.py:141`, `:154`, `:166`. Arquivar pela tranca encerra com via própria: `app/chamados/models.py:1623`, `:1640`.
- `superado` permanece no catálogo, mas sua porta foi retirada por ausência de chamador: `app/chamados/models.py:795`; não deve aparecer como transição executável corrente.

Não executei Django nem testes; as provas reportadas são testes lidos, com referências enviadas ao principal. [não verificado: execução] [nao verificado]

## 13/11 17:57 · sessao 95147ecea9fa · - · - · origem=Ronald (tmux)

**1 — LEU:** O predicado ficou igual nos dois caminhos: CSF e retratação chamam `sem_veredito_entre(..., ignorar_tranca=True)`; `pode_executar_csf` nega a lista resultante. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/juizes.py:2033–2035`.)

**1 — INFERIU:** O BLOQ “CSF libera / retratação segura” foi eliminado para a mesma pergunta nas mesmas condições, mas os universos continuam distintos:

- CSF = perguntas de todas as disputas do pai **∪** perguntas com `chamado_destino=ch`. (`app/ponto/supra_juiz.py:558`, `:573–574`.)
- Retratação = perguntas com `chamado_destino=ch` **∪** perguntas do pai cujo destino não está VIVO. (`app/chamados/juizes.py:2033–2035`.)
- Portanto a retratação é subconjunto do CSF. Ainda existe **CSF segura / retratação libera**: resposta pendente do pai creditada a outro destino vivo. Pergunta creditada ao chamado mas fora do pai entra em ambos pelo destino. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/juizes.py:2033–2035`.)

**3 — LEU:** `_perguntas_do_pai` não usa `select_related`. Entretanto o delta passa `ignorar_tranca=True` nos dois universos; o `or` evita executar `dia_da_pergunta_trancado`, e só se leem campos da própria pergunta. O caminho que acessaria disputa/colaborador está em `dia_da_pergunta_trancado`. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/models.py:1898–1902`; `app/chamados/juizes.py:991–992`.)

**3 — INFERIU:** Não sobra query por pergunta nesse gate atual. A nota utiliza a lista `_pend` já calculada e seus PKs, sem recarregar pergunta por pergunta. (`app/chamados/models.py:719`, `:735–739`.)

**3 — LEU:** O novo selo deixa todas as N perguntas pendentes e compara queries com 1 e 3 disputas. Ele chama `registrar=False`, portanto não mede a escrita da nota. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`, `:56–57`, `:69–75`.) Execução e contagens reais: **[não verificado]**.

**4 — INFERIU:** Os dois mutantes são pegos pelo **mesmo `assertEqual` da linha 307**, em iterações diferentes:

- Retirar o kwarg do **destino** (`supra_juiz.py:574`): falha primeiro com `quem='destino'`.
- Retirar o kwarg do **pai** (`supra_juiz.py:573`): destino passa; falha com `quem='pai'`.

A fixture tem pai e destino distintos, uma pergunta ligada aos dois, e tranca simulada como verdadeira; o predicado sem kwarg exclui a pergunta. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:44–57`, `:300–309`; `app/chamados/models.py:1902`.)

**4 — LEU:** O `assertFalse(pode_executar_csf(...))` fica na linha 310; o mutante já abortaria no `assertEqual` anterior. O RED verifica `irmas_respondidas_sem_consumo(destino)`, mas **não verifica `irmas_respondidas_sem_consumo(pai)`**, portanto não demonstra igualdade dos universos. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304–310`.) Execução dos mutantes: **[não verificado]**.

## 13/11 17:56 · sessao a5c16e32d31f · BUG-PAR · verificador · origem=Code

**BLOQUEIA.** Há generalização indevida de COLISÃO para PARIDADE, um caminho que pode regravar competência 10 exportada e uma recusa terminal ainda apresentada como “Tudo validado e gravado”. (`app/chamados/services/materializacao.py:1238`; `app/chamados/services/validacao.py:555`; `app/chamados/services/desenlace.py:72`)

Suíte, execução dos REDs, mutantes e ocorrência dos contraexemplos na produção: **[não verificado]**. [nao verificado]

**1 — BLOQUEIA: a primeira cura fecha o caso da irmã, mas altera também a colisão alheia.**

O ramo pergunta somente se `_conflito` traduz como COLISAO. Não identifica a batida ofensora. Entretanto, o chokepoint procura **qualquer** batida oposta na janela e devolve texto sem o identificador dela. Logo, “colisão pode ser com ela” não prova que o rollback removeu a causa. (`app/chamados/services/materializacao.py:1237`; `app/ponto/registro_batida.py:139`; `app/ponto/registro_batida.py:146`)

Contraexemplo sintético:

- Chão: `E 18:30`, **E alheia 01:08**, `S 06:30`; respostas a plantar: `S 01:10` e `E 02:00`.
- Chamado cobra `S 01:10`, enquanto o DNA vigente tem o marco S às `01:00`. Essa diferença é representável: o plantio prefere o marco cobrado no chamado; a exceção de colisão consulta os marcos vigentes e tolera cinco minutos. (`app/chamados/services/materializacao.py:488`; `app/chamados/services/materializacao.py:649`; `app/ponto/registro_batida.py:35`)
- A projeção aceita a vizinhança das duas metades; a irmã `E 02:00` grava. A tentativa de `S 01:10` colide com a **E alheia 01:08**, a dois minutos, fora da exceção do marco próprio. O rollback remove a irmã, mas preserva a E alheia. (`app/chamados/services/materializacao.py:227`; `app/chamados/services/materializacao.py:1442`; `app/ponto/registro_batida.py:145`)
- Mesmo assim, a cura grava motivo PARIDADE e deixa `materializada_em` nulo; o contrato existente manda COLISAO para `VIA_FATO_OCUPADO`. Na validação humana, a devolução também limpa `validada_em`. (`app/chamados/services/materializacao.py:1240`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:192`; `app/chamados/services/validacao.py:201`)

Portanto, para a colisão alheia persistente, deveria continuar valendo `fato_ocupado`. Com os mesmos insumos, sucessivas tentativas repetem a recusa e a devolução ao admin. Isso é um ciclo operacional de revalidação; não há laço computacional infinito demonstrado. (`app/chamados/services/validacao.py:570`; `app/chamados/services/validacao.py:575`)

**OK quanto ao catálogo e ao corte:** a frase nova casa a PARIDADE correta. A única regra anterior também retorna PARIDADE; COMPETENCIA e COLISAO vêm depois. A prioridade é da regra, pois o catálogo usa primeira correspondência com `any()`, independentemente da posição da expressão no texto. (`app/chamados/catalogo/recusa.py:29`; `app/chamados/catalogo/recusa.py:34`; `app/chamados/catalogo/recusa.py:89`)

Para `Pergunta #51530`, timestamp sem microssegundos e origem `disputa_s84_retro`, a composição nova tem **235 caracteres**. Se uma composição maior ultrapassar 240, o prefixo classificatório continua preservado; detalhes finais podem ser cortados. (`app/chamados/services/materializacao.py:1240`; `app/ponto/registro_batida.py:147`; `app/chamados/services/materializacao.py:1399`)

**2 — OK: os cinco chamadores querem o universo apurável.**

| Chamador | O que julga |
|---|---|
| `app/chamados/services/materializacao.py:215` | Se a metade isolada exige o par. |
| `app/chamados/services/materializacao.py:228` | Paridade com a irmã projetada. |
| `app/chamados/services/materializacao.py:712` | Antevisão do efeito do plantio na tela. |
| `app/chamados/services/materializacao.py:1150` | Guarda efetiva do plantio. |
| `app/chamados/services/materializacao.py:1161` | Rechecagem depois da irmã gravada. |

Nenhum desses usos apresenta o espelho cru. A autoridade distingue explicitamente apuração, que exclui retratadas, de exibição de marcação pela Portaria 671. A cura consulta essa autoridade nos dois lados. (`app/ponto/turnos.py:2856`; `app/ponto/turnos.py:2876`; `app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:113`)

`passivo_validado.py:497` descreve **outra guarda, adicional**: comparar a geometria do dia antes/depois pelo pareador. Ela continua necessária: alternância local pode passar enquanto um plantio avulso transforma `E* S` em `E* S E`. Não há divergência introduzida pela troca de universo. (`app/chamados/services/passivo_validado.py:492`; `app/chamados/services/passivo_validado.py:527`; `app/chamados/services/passivo_validado.py:557`; `app/chamados/services/materializacao.py:454`)

Não confundir com `_batida_existente_na_janela`: esse leitor permanece deliberadamente cru para impedir replantio de marca já retratada, pergunta diferente da paridade apurável. (`app/chamados/juizes.py:243`; `app/chamados/juizes.py:806`)

**3 — BLOQUEIA no significado de `recusa_motivo`; portas de carimbo preservadas.**

Censo dos escritores e leitores decisórios:

| Campo | Quem escreve | Quem lê para decidir |
|---|---|---|
| `via_resolucao` | Porta central `_escrever_carimbo`; descarimbo em lote; troca por fato no espelho; relavra. (`app/chamados/models.py:3127`; `app/chamados/models.py:3308`; `app/chamados/models.py:3317`; `app/chamados/services/relavra_via.py:125`) | C6/identidade, lastro, estado de aplicação, universo 103 e flip automático. (`app/chamados/juizes.py:1438`; `app/chamados/juizes.py:679`; `app/chamados/services/materializacao.py:692`; `app/chamados/services/materializacao.py:276`; `app/ponto/services/flip_auto.py:129`) |
| `materializada_em` | `_escrever_carimbo` e descarimbo em lote; a porta 103 chama `descarimbar`. (`app/chamados/models.py:3125`; `app/chamados/models.py:3308`; `app/chamados/services/validacao.py:268`) | Estado, C6, aplicação, seleção do materializador, terminalidade da recusa, devolução e elegibilidade 103. (`app/chamados/models.py:3069`; `app/chamados/juizes.py:1438`; `app/chamados/services/materializacao.py:688`; `app/chamados/services/materializacao.py:1744`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:192`; `app/chamados/services/materializacao.py:276`) |
| `validada_em` | Validação canônica; devoluções comum/103; resposta nova; reabertura individual/lote; simetria da porta de carimbo. (`app/chamados/services/validacao.py:136`; `app/chamados/services/validacao.py:201`; `app/chamados/services/validacao.py:271`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:1508`; `app/chamados/models.py:1567`; `app/chamados/models.py:3161`) | C6, estado, impedimento de nova validação, exigência de irmã validada e fila de materialização. (`app/chamados/juizes.py:1401`; `app/chamados/models.py:3067`; `app/chamados/services/validacao.py:498`; `app/chamados/services/materializacao.py:195`; `app/chamados/services/materializacao.py:1743`) |
| `recusa_motivo` | Recusa do materializador; pendência de marco; recusa humana; contestação respondida; duplicata, usando também a porta de identidade. (`app/chamados/services/materializacao.py:1411`; `app/chamados/services/materializacao.py:1125`; `app/chamados/services/validacao.py:705`; `app/chamados/services/disputa_emissao.py:1497`; `app/chamados/services/fio.py:1399`; `app/chamados/models.py:3214`) | Classificador/terminalidade, contador de paridade/103, presença exigida pela devolução, explicador, flip por regex e alarmes de ausência de motivo. (`app/chamados/services/materializacao.py:1400`; `app/chamados/services/materializacao.py:262`; `app/chamados/services/validacao.py:194`; `app/chamados/services/explicador.py:90`; `app/ponto/services/flip_auto.py:132`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`) |
| `Batida.retratada_em` | Escrita física por `anotar`; retratação fornece o instante; desfazer humano fornece `None`. (`app/ponto/registro_batida.py:253`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`) | Universo apurável, deduplicação/colisão do chokepoint, pareamento e lastro. (`app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:141`; `app/ponto/turnos.py:1864`; `app/chamados/juizes.py:755`) |
| Carimbo `BUG-PAR-103` | Porta trava, reconsulta elegibilidade, descarimba, limpa validação e registra DEVOLUCAO/evento antes/depois; falta de evento aborta. (`app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:268`; `app/chamados/services/validacao.py:276`; `app/chamados/services/validacao.py:281`; `app/chamados/services/validacao.py:288`) | Rótulo consulta precisamente DEVOLUCAO cujo motivo começa com `BUG-PAR-103`; estado de aplicação incorpora esse rótulo. (`app/chamados/services/materializacao.py:285`; `app/chamados/services/materializacao.py:697`) |

O problema de dois significados é concreto: texto livre de recusa humana compartilha campo com expressões que determinam código e terminalidade. A mensagem nova muda **estado**, contador e orientação ao admin, além da redação. (`app/chamados/services/validacao.py:705`; `app/chamados/catalogo/recusa.py:89`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/materializacao.py:262`)

Outros consumidores de tradução são o passivo e as respostas de tela. Eles passam a apresentar “vizinha do MESMO tipo” para uma causa original de **tipo oposto**. (`app/chamados/services/passivo_validado.py:144`; `app/chamados/services/fio.py:741`; `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:174`; `app/chamados/services/validacao.py:643`; `app/chamados/catalogo/recusa.py:35`; `app/chamados/catalogo/recusa.py:75`)

O regex independente do flip exige `batida vizinha #…`; a frase nova não fornece isso. Contudo, o leitor também exige `VIA_PARIDADE_RECUSADA`, que o ramo novo não grava: não há novo flip demonstrado por essa mensagem. (`app/ponto/services/flip_auto.py:129`; `app/ponto/services/flip_auto.py:132`; `app/chamados/services/materializacao.py:1402`)

Completude de leitores indiretos/dinâmicos: **[não verificado]**.

**4 — BLOQUEIA: regressões sem proteção suficiente e complemento da Central incompleto.**

- **Segundo escritor/juiz — OK no caminho examinado.** A irmã passa por `_materializar_uma`, a batida por `registrar_batida`, e tela/validação/materialização consultam `par_do_intervalo`. A projeção é temporária e restaurada em `finally`. (`app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:1219`; `app/chamados/services/materializacao.py:1440`)
- **Universo manual — OK.** Os dois lados usam `batidas_apuraveis`; não há cópia manual do filtro nessa cura. (`app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:113`)
- **Relógio — NAO-BLOQUEIA adicional.** O corte 103 compara o instante do carimbo com limite fixo no fuso da casa; não depende de “agora” durante a classificação. (`app/chamados/services/materializacao.py:273`; `app/chamados/services/materializacao.py:277`)
- **RED_2c — proteção insuficiente.** Injeta COLISAO em `01:00`, com irmã às `02:00`: não demonstra colisão real com a irmã dentro da janela de até 300 segundos. Também não exige preservar COMPETENCIA_APROVADA. O mutante “toda recusa vira PARIDADE” satisfaz suas asserções; RED_2b cobra rollback, mas não cobra o código da competência. (`app/chamados/tests/test_bug_par_projetado.py:690`; `app/chamados/tests/test_bug_par_projetado.py:695`; `app/chamados/tests/test_bug_par_projetado.py:707`; `app/chamados/tests/test_bug_par_projetado.py:672`; `app/chamados/tests/test_bug_par_projetado.py:679`; `app/ponto/registro_batida.py:31`)
- **RED_2d — OK por inspeção.** Se somente o anterior for curado, o primeiro cenário falha no posterior retratado; se somente o posterior for curado, o segundo cenário falha no anterior retratado. Execução dos mutantes: **[não verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:715`; `app/chamados/tests/test_bug_par_projetado.py:717`; `app/chamados/tests/test_bug_par_projetado.py:721`; `app/chamados/tests/test_bug_par_projetado.py:723`)
- **Exceção convertida em prosa — NAO-BLOQUEIA adicional, mas permanece.** `_materializar_uma` captura qualquer `Exception`, transforma em texto e entrega esse texto ao classificador de estado. Uma exceção concreta engolida na produção: **[não verificado]**. (`app/chamados/services/materializacao.py:1636`; `app/chamados/services/materializacao.py:1648`)

**BLOQUEIA adicional: a Central ainda mente em `VIA_FATO_OCUPADO`.** Contraexemplo: pergunta HORA recusada por colisão alheia, encerrada com essa via, sem batida plantada. O lastro aceita a identidade; `estado_aplicacao_pergunta` retorna `aplicada` para toda materializada exceto PARIDADE; o explicador só marca `gravou=False` para `recusada_paridade`. Assim, com a disputa quitada, o desenlace continua dizendo **“Tudo validado e gravado.”** (`app/chamados/models.py:3823`; `app/chamados/juizes.py:679`; `app/chamados/juizes.py:691`; `app/chamados/services/materializacao.py:688`; `app/chamados/services/explicador.py:101`; `app/chamados/services/desenlace.py:65`; `app/chamados/services/desenlace.py:72`)

O teste da Central cobre somente `VIA_PARIDADE_RECUSADA`; o controle seguinte presume “gravado” quando o item não traz a marca. (`app/chamados/tests/test_bug_par_projetado.py:261`; `app/chamados/tests/test_bug_par_projetado.py:274`)

**5 — BLOQUEIA: há efeito potencial em dinheiro, inclusive exportada.**

**Competência aberta:** sim, uma colisão antes terminal agora pode ficar reaplicável. Se o obstáculo alheio for posteriormente corrigido/retratado, uma nova validação pode plantar o par e alterar a hora apurada. O escritor de batida dispara recálculo por evento. Ocorrência e valor na frota: **[não verificado]**. (`app/chamados/services/materializacao.py:1238`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:201`; `app/ponto/registro_batida.py:160`; `app/ponto/services/fechamento.py:1809`)

**Competência 10 exportada:** há problema mesmo sem plantar batida:

1. A porta 103 devolve a pergunta para validação do admin. (`app/chamados/services/validacao.py:268`)
2. `validar_pergunta` agenda recálculo **antes** da tentativa de materialização. (`app/chamados/services/validacao.py:544`; `app/chamados/services/validacao.py:555`)
3. O chokepoint recusa a batida em competência aprovada/exportada. A devolução limpa a validação, mas a transação externa termina sem rollback. (`app/ponto/janelas.py:87`; `app/ponto/registro_batida.py:132`; `app/chamados/services/validacao.py:201`; `app/chamados/services/validacao.py:575`)
4. No commit, o callback chama o recálculo, que atualiza os campos calculados do fechamento aprovado/exportado. A guarda especial cobre somente **09/2026**. (`app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:36`)

Contraexemplo: fechamento 10 exportado com 100 horas gravadas e derivação atual de 110 horas. Validar uma pergunta devolvida pode regravar 110, embora o plantio seja recusado. Os valores são sintéticos; existência desse desvio na produção **[não verificado]**. O caminho permite a escrita, contrariando a condição específica de preservar o gravado do aval fornecido. (`app/ponto/services/fechamento.py:1162`; `app/chamados/services/validacao.py:555`)

**6 — Os NAO-BLOQ da triagem3: um sobe para BLOQUEIA.**

| Achado anterior | Veredito desta volta |
|---|---|
| Mutantes RED5/RED7 escapam | **NAO-BLOQUEIA isoladamente; sobrevivência exata [não verificado].** RED5 cobra conteúdo renderizado; RED7 cobra dois arquivos e conteúdo `[0,1]`. Isso não demonstra quais mutações foram executadas. (`app/chamados/tests/test_bug_par_projetado.py:766`; `app/chamados/tests/test_bug_par_projetado.py:838`) |
| Texto `FORA_DE_PIPELINE` | **NAO-BLOQUEIA isoladamente; divergência permanece.** Continua “exportada segue a L-113”. Alterar a frase não corrige o caminho financeiro acima. (`app/config/crons.py:774`; `app/chamados/services/validacao.py:555`) |
| Caminho de dinheiro | **BLOQUEIA**, pelos passos do item 5. DRY default não protege a validação humana posterior. O DRY retorna antes da escrita do arquivo. (`app/chamados/services/validacao.py:555`; `app/chamados/management/commands/bug_par_103.py:65`) |
| Foto de reversão antes da trava | **NAO-BLOQUEIA isoladamente, risco confirmado.** O comando fotografa/grava antes de chamar a porta; só nela ocorre a trava. A trilha efetiva antes/depois é lida sob trava. (`app/chamados/management/commands/bug_par_103.py:48`; `app/chamados/management/commands/bug_par_103.py:77`; `app/chamados/management/commands/bug_par_103.py:85`; `app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:263`) |

Contraexemplo da reversão: A fotografa; B devolve a pergunta; A adquire a trava e retorna `não_caso`. O arquivo de A inclui uma pergunta que A não alterou. Restaurá-lo cegamente desfaria B; aplicador desse JSON e ocorrência concorrente **[não verificado]**. (`app/chamados/management/commands/bug_par_103.py:55`; `app/chamados/services/validacao.py:261`; `app/chamados/management/commands/bug_par_103.py:91`)

**7 — NAO-BLOQUEIA adicional; LEI-AKITA do commit [não verificado].** [nao verificado]

A mensagem/metadados do commit não estão disponíveis nesta cópia sem `.git`; portanto, **presença e teor da linha LEI-AKITA de `4ab30d147` [não verificado]**. Os campos obrigatórios são `origem`, `testemunha`, `RED`, `quem-mais-le`, `juizes novos` e `padrao`. (`CLAUDE.md:62`)

Contra o código, a testemunha do universo foi centralizada corretamente, e a irmã usa o escritor existente. Entretanto, uma declaração de cura completa da classe COLISAO, de RED adversarial suficiente ou de preservação da exportada não seria verdadeira pelos contraexemplos acima. (`app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:1442`; `app/chamados/services/materializacao.py:1238`; `app/chamados/tests/test_bug_par_projetado.py:695`; `app/chamados/services/validacao.py:555`)

## 13/11 17:55 · sessao 4725a19810ec · - · - · origem=Ronald (tmux)

**BLOQUEIA — complemento incompleto.** Contraexemplo: pergunta de hora respondida, recusada por `COLISAO`, com carimbo `VIA_FATO_OCUPADO`; `_carimbar_recusa` produz esse estado sem criar batida (`app/chamados/services/materializacao.py:1402`, `app/chamados/services/materializacao.py:1408`).

Esse carimbo passa no lastro por identidade (`app/chamados/models.py:2531`, `app/chamados/models.py:3823`, `app/chamados/juizes.py:679`). A testemunha devolve `aplicada`, porque excepciona somente `VIA_PARIDADE_RECUSADA` (`app/chamados/services/materializacao.py:688`). O explicador consequentemente não define `gravou=False` (`app/chamados/services/explicador.py:99`), e a Central imprime **“Tudo validado e gravado.”** (`app/chamados/services/desenlace.py:65`, `app/chamados/services/desenlace.py:72`).

Reprodução em memória com os quatro corpos reais extraídos por AST confirmou: `lastro=True → aplicada → resolvida sem gravou → Tudo validado e gravado`. O `pode_fechar=True` foi alimentação sintética, seguindo o molde do teste existente (`app/chamados/tests/test_bug_par_projetado.py:256`); ocorrência em produção: **[não verificado]**.

O teste do complemento cobre exclusivamente paridade recusada (`app/chamados/tests/test_bug_par_projetado.py:261`); o outro presume que ausência da marca significa gravação (`app/chamados/tests/test_bug_par_projetado.py:272`).

## 13/11 17:55 · sessao 5da9bf11e56d · HJ-090 · verificador · origem=Code

**BLOQUEIA a aprovação final:** a reversão ainda aceita registro anterior a uma remarcação e pode desfazer um apply posterior. As guardas conferem valores e `[pk, dias]`, sem identificar datas nem o ato revertido (`app/ferias/services.py:137,176-179,635-636`).

A identidade da cópia com o commit informado, o histórico Git, a execução da suíte e qualquer ocorrência em produção ficam **[nao verificado]**. A cópia disponibilizada não contém `.git`; não executei testes que escrevem arquivos ou banco. **[nao verificado]** [nao verificado]

**1. Triagem2: fechamento de cada PROCEDENTE**

| Item | Veredito | Conferência e contraexemplo |
|---|---|---|
| 1 — registro depois do atomic | **OK para a falha original; BLOQUEIA a reversão completa** | Agora write, flush e fsync acontecem dentro do atomic externo, mantendo a trava até a saída dele; falha desfaz o período (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123-130`). Persistem os contraexemplos abaixo. |
| 2 — edição sobre foto, sem vendidos | **OK para o julgamento solicitado** | A porta relê sob trava e exige direito ≥ concedidos + vendidos; a view traduz `RecusaNaPorta` (`app/ferias/services.py:730-742`; `app/ferias/views.py:844-851`). Não localizei caminho adicional de edição do PA em admin/API/mensageria; o admin de férias registra somente `Afastamento` (`app/ferias/admin.py:2-5`). Existência de outro caminho externo à cópia: **[nao verificado]**. |
| 3 — contador ausente do carimbo | **OK** | Os dois sentidos entram no censo e em esperado=0; o leitor genérico compara e apresenta essas chaves (`app/chamados/management/commands/vigiar_fio_morto.py:98-99,227-246`; `bin/fio_estado.py:121-126,191-194`). |
| 4 — mensageria descarta saldo negativo | **OK no código; prova de regressão incompleta** | O produtor entrega o número e a ferramenta agora o repassa (`app/api/views_mensageria.py:835-836`; `mensageria/nucleo/ferramentas.py:2113-2115`). A retirada desse repasse não é exercitada pelos testes de volta3 (`app/ferias/test_hj090_volta3.py:1-245`). |
| 5 — `_recusou` aceita qualquer exceção | **OK** | Agora exige `RecusaNaPorta` e a mensagem esperada; há casos para propagar `TypeError` e rejeitar outro motivo (`app/ferias/test_hj090_red_por_valor.py:45-51`; `app/ferias/test_hj090_volta3.py:234-245`). |

Os resíduos concretos do item 1 são:

- **BLOQUEIA — remarcação não invalida o registro antigo.** PA com concedidos=10 e agendamento de 20 dias: realinhar grava 20; remarcar o mesmo agendamento mantendo 20 dias preserva PK, dias e concedidos; reverter o registro anterior passa e grava 10. A identidade ignora as datas, embora o contrato diga que remarcado não deve ser tocado (`app/ferias/services.py:134-137,168-179,635-636`). O teste existente troca o PK; não cobre remarcação do mesmo objeto (`app/ferias/test_hj090_um_juiz.py:212-218`).
- **BLOQUEIA — intenção sem commit pode desfazer outro apply.** A falha no fsync depois do flush pode deixar uma linha completa enquanto o atomic desfaz o banco. Depois, outro apply pode produzir os mesmos valores e vivos; reverter o arquivo da tentativa falha desfaz esse segundo ato. Também ocorre com apply A → reverter A → apply B → repetir reversão A (`app/ferias/management/commands/realinhar_concedidos_ferias.py:129-130`; `app/ferias/test_hj090_volta3.py:107-109`; `app/ferias/services.py:176-179`).
- **OK — JSON sintaticamente corrompido no meio não é tratado como cauda.** Uma linha completa inválida levanta antes do início das reversões; somente o EOF sem newline é ignorado (`app/ferias/management/commands/realinhar_concedidos_ferias.py:43-51,81-84`).
- **NAO-BLOQUEIA isoladamente — falta validar a estrutura inteira antes de reverter.** Uma linha `{}` no meio é aceita pelo parser; o `KeyError` aparece no processamento, depois de períodos anteriores poderem ter sido revertidos. Conferi esse comportamento executando somente a função extraída, com entrada em memória (`app/ferias/management/commands/realinhar_concedidos_ferias.py:49,81-84`). O teste cobre sintaxe inválida, não estrutura inválida (`app/ferias/test_hj090_volta3.py:150-158`).

Não encontrei, no apply normal do command, a antiga janela “banco confirmado, registro ainda não escrito” (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123-130`). A porta pública, isoladamente, grava sem arquivo, mas exige trilha no banco; o command é o chamador produtivo localizado dessa porta (`app/ferias/services.py:150-164,404-412`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:124`).

**2. Escritores, leitores e significados**

O censo dos campos centrais ficou assim:

| Campo | Escritores persistentes | Leitores centrais |
|---|---|---|
| PA.`dias_direito` | Criação com defaults; art.130; sincronização; edição (`app/ferias/services.py:335-357,369-370,739-742`; `app/ferias/views.py:184-187`) | Status, saldo, teto de venda, validação da edição e apresentação (`app/ferias/models.py:98,220,238`; `app/ferias/services.py:732-735`; `app/templates/ferias/_secao_colab.html:48`) |
| PA.`dias_concedidos` | Realinhar, reverter, histórico, agendar, cancelar DP/admin, remarcar e aprovar solicitação (`app/ferias/services.py:156-158,179-181,474-477,509-511,577-581,604-613,635-651,706-708`) | Saldo, contador, reversão, edição, filtros e apresentação (`app/ferias/models.py:220`; `app/ferias/services.py:125-126,155,176,732`; `app/ferias/views.py:32,61`; `app/templates/ferias/_secao_colab.html:49`) |
| PA.`dias_vendidos` | Agendamento com abono, histórico e venda avulsa (`app/ferias/services.py:258-261,475-477,818-820`) | Saldo, vendáveis, consumo na edição e apresentação (`app/ferias/models.py:220,238`; `app/ferias/services.py:732`; `app/templates/ferias/_secao_colab.html:51`) |
| PA.`dias_saldo` | **Sem coluna/escritor:** property direito − concedidos − vendidos (`app/ferias/models.py:218-220`) | Portas de saldo/remarcação; situação/API/IA; listas, telas e relatórios (`app/ferias/services.py:297-302,470-472,631-633,690-691,814-815`; `app/ferias/situacao.py:300-302`; `app/api/views_mensageria.py:841`; `app/core/views_ia.py:492,532`; `app/ferias/management/commands/listar_ferias_dp.py:60,68`; `app/relatorios/services.py:316`; `app/relatorios/views.py:370`; `app/templates/ferias/_secao_colab.html:50,68`) |
| PA.`status` | Juiz atribui em memória; portas anteriores persistem; sincronização e garantia também escrevem (`app/ferias/models.py:207-215`; `app/ferias/services.py:330-344,374-378,740-742`) | Habilitação, listas, situação, API, relatórios e alertas (`app/ferias/models.py:234,242`; `app/ferias/views.py:31-32,57-61`; `app/ferias/services.py:208,685`; `app/api/views_mensageria.py:841`; `app/relatorios/services.py:285-310`; `app/ferias/management/commands/alertar_ferias_vencendo.py:23,35-38`) |
| Agendamento.`dias`/datas | Histórico, agendamento normal, solicitação aprovada e troca de janela (`app/ferias/services.py:449-450,506-508,636-649,697-705`) | Fracionamento, saldo/remarcação, soma, cobertura, listas, API e exportação (`app/ferias/models.py:363-386`; `app/ferias/services.py:105-106,137,577,635`; `app/ferias/management/commands/listar_ferias_dp.py:73-84`; `app/api/views.py:2244-2246`; `app/folha/export.py:491-501`) |
| Agendamento.`status` | Default na criação; juiz no histórico; cancelamentos; troca de janela; alinhamento por data (`app/ferias/models.py:319`; `app/ferias/services.py:451,506-508,580-581,612-613,641-649,695-705,837-841`) | Soma/identidade, guardas, espelho, situação, listas e API (`app/ferias/services.py:106,137,561-562`; `app/ponto/services/ausencia.py:1181`; `app/ferias/situacao.py:165-169`; `app/ferias/management/commands/listar_ferias_dp.py:62`; `app/api/views.py:2247-2253`) |

Outros consumidores identificados de PA/agendamento são os painéis do colaborador e de chamados, aviso da batida, inteligência, situação por mensageria e painel de ausência (`app/colaboradores/views.py:2847,2900`; `app/chamados/views.py:627`; `app/ponto/services/aviso_da_batida.py:74`; `app/inteligencia/views.py:17,91-98,275-289`; `app/inteligencia/management/commands/gerar_alertas.py:86,100`; `app/api/views_mensageria.py:903-905,1048-1050`; `app/ponto/views.py:2021-2026`).

Para as demais fontes:

| Fonte | Quem escreve | Quem lê |
|---|---|---|
| `Ausencia(tipo='ferias')` | Espelho cria aprovada por dia, remove no cancelamento, apaga janela antiga e rejeita no cancelamento admin (`app/ponto/services/ausencia.py:1190-1215,1263-1284`) | Guarda do agendamento, lista com batida, vigia e cobertura comum/folha (`app/ferias/models.py:433-436`; `app/ferias/management/commands/listar_ferias_com_batida.py:40-64`; `app/ponto/services/vigia_ausencia.py:214-220`; `app/ponto/turnos.py:2511-2535`; `app/ponto/services/fechamento.py:683-687`) |
| `logs/HJ-090/realinhar_*` | Command: snapshot e JSONL da intenção (`app/ferias/management/commands/realinhar_concedidos_ferias.py:99-130`) | Loader do command; porta confere concedidos e vivos (`app/ferias/management/commands/realinhar_concedidos_ferias.py:39-52,76-85`; `app/ferias/services.py:175-179`) |
| `ferias_soma_maior` / `ferias_concedidos_maior` | Vigia, pelo contador comum, com esperado zero (`app/chamados/management/commands/vigiar_fio_morto.py:98-99,229-246`) | Comparação e apresentação genéricas do carimbo (`bin/fio_estado.py:121-126,191-194`) |

**BLOQUEIA — a trava do saldo não serializa a colisão entre períodos diferentes da mesma pessoa.** Dois POSTs podem travar PAs distintos, ambos validar a mesma janela antes dos inserts e ambos criar agendamento. A validação de sobreposição é consulta, sem trava comum do colaborador; eventual duplicata de Ausencia vira recusa por dia e não necessariamente desfaz o agendamento (`app/ferias/services.py:492-508`; `app/ferias/models.py:423-454`; `app/ponto/services/ausencia.py:417-435,1210-1218`). Trata-se de interleaving permitido pelo código; reprodução concorrente em banco **[nao verificado]**.

**NAO-BLOQUEIA — sobrevive escritor público de direito sem trava.** `aplicar_art130(..., salvar=True)` salva diretamente; o caminho produtivo localizado o chama com `salvar=False`, já sob trava. Corrida por endpoint atual **[nao verificado]** (`app/ferias/services.py:327-335,350-357`; `app/ferias/models.py:160-164`).

Há leitores com significados incompatíveis:

- **NAO-BLOQUEIA — direito manual versus recalculado.** Editar direito para 35 com consumo 30 pode retornar sucesso e, na própria resposta HTMX, recalcular para 30: a resposta sincroniza períodos adquiridos e reaplica art.130. A intenção de preservar edição manual não está demonstrada no material fornecido (`app/ferias/services.py:333-344,739-742`; `app/ferias/views.py:795-802,854-855`; `app/ferias/models.py:54-55`).
- **NAO-BLOQUEIA — status inexistente no leitor.** Inteligência procura agendamento `aprovado`; os estados são agendado/em_gozo/concluido/cancelado. Um agendamento normal dos próximos 30 dias desaparece desse KPI e dessa lista (`app/inteligencia/views.py:94,278,286`; `app/ferias/models.py:296-300`).
- **NAO-BLOQUEIA — “vigente pelas datas” versus “vigente pelo status”.** A autoridade inclui concluído com data viva; situação e aviso de retorno filtram apenas agendado/em_gozo (`app/ferias/services.py:49-64`; `app/ferias/situacao.py:165-169`; `app/ferias/management/commands/alertar_retornos.py:20-22`).
- **OK na unidade central:** gozo usa dias corridos inclusivos; o espelho cria um registro por dia; exportação recorta pela competência e cruza o dia civil local da batida. A lista administrativa declara outro universo e identifica plantão da véspera (`app/ferias/services.py:450`; `app/ponto/services/ausencia.py:1195-1208`; `app/folha/export.py:486-501`; `app/ferias/management/commands/listar_ferias_com_batida.py:4-12,44-64`).

**3. Ordem de trava e rollback externo**

**NAO-BLOQUEIA — a inversão da exclusão permanece, mas o deadlock específico da triagem2 não está demonstrado.** A ordem usual é AG → PA → Ausencia; exclusão faz PA → Ausencia → delete do AG. Os escritores que recusam cancelado param antes de adquirir o PA, cortando o ciclo proposto naquele caso (`app/ferias/services.py:419,560-564,756-762`; `app/ferias/models.py:456-460`). Deadlock reproduzido: **[nao verificado]**.

**OK — a repetição da trava no agendamento com abono mantém a mesma ordem.** A porta trava PA, chama o agendamento que relê o mesmo PA e depois o relê novamente para vender. Não identifiquei inversão nessa sequência (`app/ferias/services.py:245-258,493`).

**NAO-BLOQUEIA isoladamente — arquivo pode sobreviver a rollback ancestral.** Se `call_command` for envolvido por outro atomic, o atomic do command pode ser somente savepoint; um rollback posterior deixa a intenção em disco. O código documenta expressamente intenção sem confirmação, mas imprime `APPLY` antes de qualquer commit ancestral (`app/ferias/management/commands/realinhar_concedidos_ferias.py:13-15,123-131`). Chamador produtivo que faça esse envolvimento: **[nao verificado]**. A reversão equivocada após estado posterior coincidente é o bloqueio já descrito no item 1 (`app/ferias/services.py:176-179`).

**4. Classes de reprovação, RED e costura**

**BLOQUEIA — criação e edição derivam o ciclo anual de maneiras diferentes.** Criar usa aniversário menos um dia; editar usa 364 dias fixos. Para início em 01/01/2024, criação termina em 31/12/2024 e edição do mesmo início termina em 30/12/2024, deslocando também a concessão (`app/ferias/views.py:140-144,169-173`; `app/ferias/services.py:736-738`).

**NAO-BLOQUEIA — continuam derivações paralelas de vencimento.** A autoridade registrada é `status_de_periodo`; `periodos_vencendo` exige status persistido adquirido, enquanto `vencendo` consulta o juiz. Um ciclo adquirido pelas datas, ainda marcado em_curso, aparece em um e some do outro. Há também janelas de 60 mantidas separadamente (`app/core/juizes.py:265`; `app/ferias/services.py:45,201-209`; `app/ferias/models.py:249,255-261`; `app/ferias/situacao.py:55,206-209`; `app/ferias/management/commands/pautas_de_ferias.py:68-74`).

**NAO-BLOQUEIA esta rodada — relógios internos da fila anterior persistem.** Aviso e status podem consultar dias diferentes durante a virada, inclusive na nova edição de período, que chama `aplicar_status()` sem receber hoje (`app/ferias/services.py:491,510,637-650,740`; `app/ferias/models.py:94,283`).

O vigia também não cumpre integralmente sua promessa de uma leitura: não transmite `agora` ao alcance; a alimentação acaba consultando novamente o relógio da fila. Um silêncio que expire durante a rodada pode ser julgado depois do instante assinado no carimbo (`app/chamados/management/commands/vigiar_fio_morto.py:162-176,232-233`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/juizes.py:1576,1543`; `app/chamados/services/painel.py:190`). O command novo lê `localdate()` e `now()` separadamente, podendo entregar dias distintos ao carregador na meia-noite (`app/ferias/management/commands/listar_ferias_com_batida.py:36,52`).

**NAO-BLOQUEIA — exceções de programação ainda viram resultado de negócio.** `aprovar_solicitacao` já salva aprovada e captura qualquer exceção da materialização; um `TypeError` vira aviso e a view retorna aprovação com sucesso. Não é silêncio absoluto, mas o erro deixa de propagar (`app/ferias/services.py:676-679,693-710`; `app/ferias/views.py:780-782`).

**BLOQUEIA a alegação de trilha obrigatória em toda a obra.** Na correção de datas, a porta conclui o atomic antes de a view registrar o histórico; a auditoria é best-effort. Falha da trilha pode deixar a correção aplicada e a mensagem afirmar motivo registrado (`app/ferias/services.py:649-652`; `app/ferias/views.py:525,536-544`; `app/core/auditoria.py:17-29`). O realinhamento/reversão resolve isso com `_trilha`, mas essa proteção não foi generalizada (`app/ferias/services.py:161-162,182-183,404-412`).

Quanto à mordida dos testes: [nao verificado]

| Cura | Veredito por inspeção |
|---|---|
| Registro antes do commit | **OK** para write/short-write/flush/fsync do arquivo e preservação do período anterior (`app/ferias/test_hj090_volta3.py:98-121`). |
| Cauda truncada | **OK** para EOF parcial e sintaxe inválida completa; não cobre estrutura inválida, remarcação ou repetição de registro antigo (`app/ferias/test_hj090_volta3.py:123-158`; `app/ferias/test_hj090_um_juiz.py:149-155,212-218`). |
| Direito sob trava | **OK** para vendidos, foto velha, limite e presença de `FOR UPDATE` (`app/ferias/test_hj090_volta3.py:166-189`). |
| Carimbo | **OK** contra zero fixo, omissão e troca dos sentidos: usa resultados 1 e 2 (`app/ferias/test_hj090_volta3.py:206-229`). |
| Repasse na mensageria | **Sem mordida demonstrada**: volta3 não exercita a ferramenta (`mensageria/nucleo/ferramentas.py:2114`; `app/ferias/test_hj090_volta3.py:1-245`). |
| Fsync dos diretórios | **Sem mordida demonstrada**: a injeção restringe a falha ao fd do arquivo (`app/ferias/test_hj090_volta3.py:71-74`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:116-121`). |

Execução e RED anterior dessas provas: **[nao verificado]**.

**OK — não identifiquei perda da prova anterior de fio mudo pela costura nova.** Ela dubla somente férias; continuam afirmados os argumentos do contador de fio mudo, seus valores/esperados e o dia civil local. A integração de férias tem caso separado com contador real (`app/core/tests/test_fio_mudo_placar.py:129-139,230-247`; `app/ferias/test_hj090_volta3.py:206-229`).

**5. `FORA_DE_PIPELINE`**

| Command | Veredito |
|---|---|
| `listar_ferias_com_batida` | **OK** quanto a somente consultar/imprimir e exigir `--desde` (`app/config/crons.py:771-773`; `app/ferias/management/commands/listar_ferias_com_batida.py:24-25,40-69`). O tripwire citado não vigia esse mesmo universo: PA concedidos=soma com batida nas férias produz contador zero e ainda pode aparecer na lista (`app/ferias/services.py:123-127`; `app/ferias/management/commands/listar_ferias_com_batida.py:40-46`). |
| `listar_ferias_dp` | **NAO-BLOQUEIA — motivo incompleto.** “Só leitura” omite `--simular`, que cancela e escreve dentro de transação posteriormente desfeita, admitido apenas no banco chamado sombra (`app/config/crons.py:774-776`; `app/ferias/management/commands/listar_ferias_dp.py:52-53,98-115`). |
| `realinhar_concedidos_ferias` | **OK** para DRY default e apply com autor. Reverter também escreve, por modo explícito e com autor, sem exigir `--apply` (`app/config/crons.py:777-779`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:65-85,95-109`). |

O desenho 4a exige vigia de varredura e proíbe transformá-lo em reparador; não fundamenta tornar o realinhamento rotina (`CLAUDE.md:169-173`). Exigência de cadência específica para essas duas listas: **[nao verificado]**.

O command já apontado na triagem2 continua escrevendo sem `--apply`: `alertar_ferias_vencendo` chama a sincronização que altera status e direito; está agendado sem flag (`app/ferias/management/commands/alertar_ferias_vencendo.py:14-32`; `app/ferias/services.py:333-344`; `app/config/crons.py:708`). **NAO-BLOQUEIA**, resíduo já declarado.

**6. Impacto em dinheiro**

**BLOQUEIA afirmar que as fontes lidas pela folha ficam cercadas pela competência não exportada.** Histórico materializa férias; remarcação apaga ausências antigas e grava a janela nova; cancelamento admin rejeita/remove o espelho. Esses caminhos não fazem ali a consulta à exportação (`app/ferias/services.py:443-477,604-613,629-651`). A porta declara explicitamente que período fechado não recusa a materialização (`app/ponto/services/ausencia.py:1169-1170`).

Contraexemplo: um agendamento passado cobre dia com batida e retém o colaborador no TXT; remarcar para outro intervalo pode retirar essa retenção. A exportação consulta as datas atuais do agendamento e decide `ferias_com_batida` por elas (`app/folha/export.py:491-501,876-884`). A cobertura aprovada também alimenta o fechamento (`app/ponto/turnos.py:2516-2517`; `app/ponto/services/fechamento.py:683-687`).

Isso demonstra alteração de fonte e da 

[truncado em 20000 de 21079 caracteres]

## 13/11 17:51 · sessao 2860ede427e9 · - · - · origem=Ronald (tmux)

**BLOQUEIA — dinheiro do passivo em competência 10 exportada.** A porta 103 limpa carimbo/validação sem alterar resposta, deixando o admin validar novamente (`app/chamados/services/validacao.py:268-280`). Essa validação agenda recálculo **antes** da materialização (`:544-560`); se a batida for recusada, desfaz o carimbo sem reverter o atomic externo (`:570-577`; devolução `:197-210`). O callback roda no commit e chama o escritor do fechamento (`app/ponto/services/fechamento.py:1807-1824`), que atualiza inclusive campos calculados de aprovado/exportado (`:1145-1180`). A guarda temporária protege somente 09/2026 (`:36-43`, aplicação `:560-575`), portanto não protege 10/2026. Contraexemplo: pergunta 103 da competência 10 exportada, FM gravado divergente da derivação atual; admin valida, plantio recusa por FM exportado (`app/ponto/janelas.py:85-88`; `app/ponto/registro_batida.py:132-137`), mas o callback regrava as horas. Viola o aval raspado “validar não toca o gravado” **[anexo sem arquivo:linha]**. Existência/quantidade desse estado na produção **[não verificado]**.

**NAO-BLOQUEIA — FORA_DE_PIPELINE continua divergente, mas acompanha o problema anterior.** Texto continua remetendo à L-113 (`app/config/crons.py:770-774`), enquanto o aval específico exige preservação do gravado **[anexo sem arquivo:linha]**. Corrigir somente a redação não impede o recálculo acima (`app/chamados/services/validacao.py:553-560`).

**NAO-BLOQUEIA isoladamente — foto de reversão permanece antes da trava.** Comando coleta campos (`app/chamados/management/commands/bug_par_103.py:48-60`), grava JSON (`:77-79`) e depois chama a porta (`:83-85`), cuja trava só ocorre em `app/chamados/services/validacao.py:258-263`. Contraexemplo: A grava snapshot; B devolve a mesma pergunta; A chega à trava e retorna não-caso (`:261-262`). O arquivo A inclui pergunta que A não alterou; usá-lo cegamente desfaria B. Não há aplicador desse JSON demonstrado **[não verificado]**. A trilha efetiva contém antes/depois lidos sob trava (`:263-287`), mitigando reversão manual.

**OK — DRY não escreve pelo comando.** Retorna antes de `makedirs`/`open` (`app/chamados/management/commands/bug_par_103.py:65-79`); o teste correspondente só verifica carimbo preservado/saída DRY (`app/chamados/tests/test_bug_par_projetado.py:544-553`). Execução do teste **[não verificado]**.

**NAO-BLOQUEIA — RED_2c não protege a classificação de competência fechada.** Só injeta COLISAO e exige PARIDADE (`app/chamados/tests/test_bug_par_projetado.py:695-707`). Mutante “qualquer recusa do chokepoint vira PARIDADE” satisfaz essas asserções. O vizinho RED_2b injeta competência fechada, mas cobra apenas rollback/carimbo nulo (`:672-681`), sem cobrar código COMPETENCIA. Execução de mutantes **[não verificado]**.

**RED5/RED7 da triagem:** os testes atuais de modal cobram frases de apresentação (`app/chamados/tests/test_bug_par_projetado.py:766-798`); RED7 executa dois applies e cobra dois arquivos, com conteúdos de tamanhos `[0,1]` (`:838-857`). Scripts/mutações exatas chamados “RED5/RED7” e sua sobrevivência **[não verificado]**; não inferir execução pelo nome dos testes.

**LEI-AKITA do commit:** mensagem/metadados de 4ab30d147 ausentes nesta cópia sem `.git` **[não verificado]**. Não certifiquei presença/veracidade dos campos da linha do commit. A regra exige `origem`, `testemunha`, `RED`, `quem-mais-le`, `juizes novos`, `padrao` (`CLAUDE.md:60-68`); confirmar números de linha desse último trecho pelo parent, pois foram lidos inicialmente sem `nl` **[não verificado]**.

## 13/11 17:51 · sessao e1bf90c104d0 · - · - · origem=Ronald (tmux)

Item 3 — censo de escrita/leitura e efeitos da mensagem nova:

| Campo | Escritores efetivos | Leitores decisórios |
|---|---|---|
| `via_resolucao` | Atribuição central em `app/chamados/models.py:3127`, usada por `carimbar`, `carimbar_identidade`, `descarimbar`; lote em `app/chamados/models.py:3308`; duas portas que só trocam via: `app/chamados/models.py:3317` e `app/chamados/services/relavra_via.py:125`. | C6 lê projeção temporal em `app/chamados/juizes.py:1386` e identidade em `app/chamados/juizes.py:1438`; lastro em `app/chamados/juizes.py:679`; SQL de vivas em `app/chamados/models.py:3398`; estado de aplicação em `app/chamados/services/materializacao.py:692`; universo103 em `app/chamados/services/materializacao.py:276`; flip automático em `app/ponto/services/flip_auto.py:129`. |
| `materializada_em` | Atribuição central `app/chamados/models.py:3125`; lote `app/chamados/models.py:3308`; `BUG-PAR-103` chama descarimbo, sem escrita paralela, em `app/chamados/services/validacao.py:268`. | C6 `app/chamados/juizes.py:1386`, `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1444`; estado `app/chamados/models.py:3069`; aplicação `app/chamados/services/materializacao.py:688`; fila `app/chamados/services/materializacao.py:1744`; terminalidade COLISAO `app/chamados/services/materializacao.py:1402`; devolução `app/chamados/services/validacao.py:192`; universo103 `app/chamados/services/materializacao.py:276`. |
| `validada_em` | Validação `app/chamados/services/validacao.py:136`; devolução comum `app/chamados/services/validacao.py:201`; devolução103 `app/chamados/services/validacao.py:271`; zeragem na resposta/correção `app/chamados/services/disputa_emissao.py:1480`; zeragem na reabertura `app/chamados/models.py:1508` e lote `app/chamados/models.py:1567`; autovalidação pela porta do carimbo `app/chamados/models.py:3161`; restauração em memória `app/chamados/models.py:3190`. | C6 mata validada em `app/chamados/juizes.py:1401`; estado `app/chamados/models.py:3067`; guarda de já validada `app/chamados/services/validacao.py:498`; exigência da irmã `app/chamados/services/materializacao.py:195`; fila `app/chamados/services/materializacao.py:1743`; projeção histórica `app/chamados/juizes.py:1385`; trilha a partir da projeção `app/chamados/models.py:4021`. |
| `recusa_motivo` | Cinco produtores semânticos: recusa da porta `app/chamados/services/materializacao.py:1411` (ou mesma gravação da identidade via `app/chamados/models.py:3214`); pendência de marco `app/chamados/services/materializacao.py:1125`; recusa humana `app/chamados/services/validacao.py:705`; contestação respondida `app/chamados/services/disputa_emissao.py:1497`; duplicata por porta identidade `app/chamados/services/fio.py:1399`. | Código do motivo decide carimbo terminal em `app/chamados/services/materializacao.py:1400`; universo de paridade/103 em `app/chamados/services/materializacao.py:262`; retorno do passivo em `app/chamados/services/passivo_validado.py:144` e `app/chamados/services/passivo_validado.py:567`; devolução exige presença em `app/chamados/services/validacao.py:194`; regex do flip em `app/ponto/services/flip_auto.py:132`; I18/I19 testam vazio em `app/chamados/management/commands/auditar_invariantes_chamados.py:1075` e `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`. |
| `Batida.retratada_em` | Escrita dinâmica `anotar`: `app/ponto/registro_batida.py:253`; retratação canônica fornece `timezone.now()` em `app/ponto/registro_batida.py:274`; desfazer humano fornece `None` em `app/ponto/services/retratar_humano.py:207`. Chamam retratação: humano `app/ponto/services/retratar_humano.py:179`, geofence `app/chamados/services/acoes_chamado.py:277`, par relâmpago `app/ponto/management/commands/detectar_par_relampago.py:224`, passivos `app/ponto/management/commands/diff_passivo_s84.py:82` e `app/ponto/management/commands/passivo_disputa_retro.py:164`. | Chokepoint filtra retratadas em `app/ponto/registro_batida.py:109` e `app/ponto/registro_batida.py:141`; pareador `app/ponto/turnos.py:1864`; lastro `app/chamados/juizes.py:755`; novo vizinho delega universo em `app/chamados/services/materializacao.py:107`; outros consumidores de apuração usam porta/filtragem em `app/ponto/turnos.py:1902`, `app/ponto/turnos.py:2221`, `app/ponto/turnos.py:2353`, `app/ponto/turnos.py:2384`, `app/ponto/turnos.py:2469`, `app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2915`. |

**OK — carimbo BUG-PAR-103 usa porta e trilha.** A elegibilidade vem do predicado único em `app/chamados/services/materializacao.py:265`; a porta trava e reconsulta em `app/chamados/services/validacao.py:258`; descarimba e zera validação em `app/chamados/services/validacao.py:268`; grava `DecisaoPergunta.DEVOLUCAO` cujo motivo começa com BUG-PAR-103 em `app/chamados/services/validacao.py:276`; grava evento antes/depois em `app/chamados/services/validacao.py:281`; ausência do evento aborta em `app/chamados/services/validacao.py:288`. O leitor do rótulo consulta precisamente esse ato e prefixo em `app/chamados/services/materializacao.py:285`, chamado pela testemunha em `app/chamados/services/materializacao.py:697`.

**BLOQUEIA — texto livre controla terminalidade e agora generaliza COLISAO.** `_carimbar_recusa` trunca em 240 ANTES de traduzir e só carimba terminal se tradução = COLISAO em `app/chamados/services/materializacao.py:1399`. A nova mensagem classifica PARIDADE pela regra `guarda de paridade`/`par quebrado`, antes da regra COLISAO, em `app/chamados/catalogo/recusa.py:34`; isso é o comportamento pretendido para irmã desfeita, mas também transforma colisão ALHEIA persistente em pendente porque o ramo só verifica o código, sem identidade da batida, em `app/chamados/services/materializacao.py:1238`. A devolução então zera validação quando `materializada_em=None`, em `app/chamados/services/validacao.py:192` e `app/chamados/services/validacao.py:201`. O contador passa a contar essa colisão como paridade em `app/chamados/services/materializacao.py:262`. Contraexemplo: registrar a segunda metade recusa por batida alheia que permanece após rollback; qualquer nova validação repete o mesmo ramo e volta ao admin, sem produzir `fato_ocupado` — sequência demonstrável pela composição das linhas citadas; ocorrência em produção `[não verificado]`.

**OK — marca nova não cai numa regra anterior errada.** A única regra anterior à PARIDADE genérica procura `falta a outra metade do intervalo`, também de código PARIDADE, em `app/chamados/catalogo/recusa.py:29`; depois vêm COMPETENCIA em `app/chamados/catalogo/recusa.py:43` e COLISAO em `app/chamados/catalogo/recusa.py:74`. O catálogo usa `any()` e primeira regra correspondente em `app/chamados/catalogo/recusa.py:89`, portanto posição da marca dentro da string não muda prioridade; pô-la no começo apenas protege da truncagem de `app/chamados/services/materializacao.py:1399`.

**NAO-BLOQUEIA isoladamente — censo dos outros leitores textuais.** Tradução para mensagem aparece em `app/chamados/services/fio.py:741`, `app/chamados/services/acoes_disputa.py:34`, `app/chamados/services/acoes_disputa.py:174` e `app/chamados/services/validacao.py:643`; a nova classificação muda a instrução mostrada para “vizinha do MESMO tipo”, embora a causa original seja oposto em menos de 300s, conforme `app/chamados/catalogo/recusa.py:35` e `app/chamados/catalogo/recusa.py:75`. O regex independente do flip exige `batida vizinha #<id> (<tipo>` em `app/ponto/services/flip_auto.py:132`; a nova mensagem não contém essa estrutura (`app/chamados/services/materializacao.py:1240`), mas a própria consulta exige `via_resolucao=VIA_PARIDADE_RECUSADA` em `app/ponto/services/flip_auto.py:129`, via que o ramo novo não grava porque PARIDADE não carimba em `app/chamados/services/materializacao.py:1402`. Portanto o texto novo não cria um flip automático por esse leitor.

**OK quanto a escritor novo do delta — portas já têm tripwires.** O conjunto permitido para carimbo e via está explícito em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`, e o selo compara escritores em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:226` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`. `validada_em` tem dono e exceções por módulo em `app/chamados/tests/test_contract_dono_validada.py:27` e `app/chamados/tests/test_contract_dono_validada.py:30`; a descrição “allowlist vazia” está desatualizada diante das três entradas nesse mesmo arquivo (`app/chamados/tests/test_contract_dono_validada.py:14`, `app/chamados/tests/test_contract_dono_validada.py:34`). Nenhum teste foi executado; verde da suíte `[não verificado]`.

Este censo cobre os produtores semânticos e leitores decisórios dos campos no fluxo BUG-PAR; completude de todos os leitores indiretos/dinâmicos e comandos de toda a árvore `[não verificado]`.

## 13/11 17:50 · sessao 4725a19810ec · - · - · origem=Ronald (tmux)

**Item 2 — OK na troca de universo.** Os cinco usos julgam efeito de plantio, não exibem marcação:

| Chamador | Pergunta respondida |
|---|---|
| `app/chamados/services/materializacao.py:215` | A metade sozinha exige par? |
| `app/chamados/services/materializacao.py:228` | Cada metade aceita a irmã projetada? |
| `app/chamados/services/materializacao.py:712` | A tela pode anunciar plantio aplicável? |
| `app/chamados/services/materializacao.py:1150` | O plantio é permitido pela paridade? |
| `app/chamados/services/materializacao.py:1161` | Após gravar a irmã, a paridade continua válida? |

A lei distingue apuração de exibição crua (`app/docs/LEIS.md:76`); a porta exclui somente retratadas, preservando pendentes de DP (`app/ponto/turnos.py:2856`, `app/ponto/turnos.py:2876`). A troca usa essa porta nos dois lados, sem remontar universo manual (`app/chamados/services/materializacao.py:110`, `app/chamados/services/materializacao.py:113`).

**Contraprova ao possível falso positivo do cru:** `_batida_existente_na_janela` permanece deliberadamente crua (`app/chamados/juizes.py:243`). O contrato explica que sua pergunta é impedir replantio da marca já retratada; ignorá-la faria ciclo cluster → retratação → replantio (`app/chamados/juizes.py:806`). Isso não justifica cru na guarda de vizinhança, que responde outra pergunta (`app/chamados/services/materializacao.py:94`).

**`passivo_validado.py:497` — OK, sem divergência introduzida.** O trecho descreve uma guarda adicional de geometria do dia inteiro; mede antes, chama o mesmo materializador e reprova se aumentam turnos abertos/saídas órfãs (`app/chamados/services/passivo_validado.py:527`, `app/chamados/services/passivo_validado.py:537`, `app/chamados/services/passivo_validado.py:557`). A contagem consulta o pareador, sem construir batidas em paralelo (`app/chamados/services/materializacao.py:453`). Contraexemplo da própria especificação: plantar avulsa pode manter alternância local e transformar `E* S` em `E* S E`; essa guarda adicional continua necessária (`app/chamados/services/passivo_validado.py:492`).

**RED_2d — OK por inspeção: exige as duas curas.** Se somente anterior migrar, a primeira consulta ainda encontrará `E 03:00` retratada no posterior, escondendo `S 06:30`, e falhará em `assertIsNotNone` (`app/chamados/tests/test_bug_par_projetado.py:715`, `app/chamados/tests/test_bug_par_projetado.py:717`). Se somente posterior migrar, o segundo cenário deixa `S 00:30` retratada esconder a `E 18:30` anterior, falhando na segunda consulta (`app/chamados/tests/test_bug_par_projetado.py:721`, `app/chamados/tests/test_bug_par_projetado.py:723`). Execução efetiva/mutante: **[não verificado]**.

**Classes de regressão — nenhum segundo escritor de Batida nesse caminho identificado.** A irmã passa pelo mesmo `_materializar_uma`, com projeção temporária protegida por `finally` (`app/chamados/services/materializacao.py:1440`); o plantio efetivo chama `registrar_batida` (`app/chamados/services/materializacao.py:1219`). `par_do_intervalo` concentra os mesmos critérios para tela, validação e escrita, com ambos os lados projetados (`app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:227`).

**Exceções — risco existente, não introduzido pelo delta apresentado.** `_materializar_uma` transforma qualquer `Exception` em texto de recusa; o `finally` o envia ao classificador textual (`app/chamados/services/materializacao.py:1636`, `app/chamados/services/materializacao.py:1648`). Já a segunda opinião posicional, em erro, registra warning e retorna falso: restringe a autorização, sem aprovar plantio no escuro (`app/chamados/services/materializacao.py:421`). Um erro concreto engolido em produção é **[não verificado]**.

## 13/11 17:50 · sessao 206e3fa43e2b · - · - · origem=Ronald (tmux)

Censo e achados enviados; principais pontos para consolidação:

- **Cura 2 fecha o julgamento da edição:** único caller produtivo encontrado é `app/ferias/views.py:846`; julgamento usa linha relida e travada em `app/ferias/services.py:731-742`. Admin de férias registra somente Afastamento (`app/ferias/admin.py:2-5`).

- **Escritor público de direito ainda sem trava:** `aplicar_art130(pa, salvar=True)` salva diretamente em `app/ferias/services.py:350-357`, exposto por `app/ferias/models.py:160-164`. O caller produtivo encontrado usa `salvar=False` sob trava (`app/ferias/services.py:327-335`); corrida por endpoint atual **[nao verificado]**.

- **Direito editado pode ser imediatamente sobrescrito:** editar para 35 com consumo 30 gera status adquirido (`app/ferias/services.py:739-742`); resposta HTMX chama contexto (`app/ferias/views.py:854-855`), que sincroniza períodos (`app/ferias/views.py:795-802`); sincronização recalcula direito pelo art.130 (`app/ferias/services.py:333-344`). Sem faltas, volta a 30 pelo juiz (`app/ferias/models.py:54-55`). Contraprova estática; execução **[nao verificado]**.

- **Concorrência entre PAs diferentes não é serializada por colaborador:** cada criação trava apenas seu PA (`app/ferias/services.py:492-503`). As duas requisições podem passar pelo veto antes dos inserts (`app/ferias/models.py:423-442`). A constraint da Ausencia não necessariamente desfaz o Agendamento: INSERT tem savepoint (`app/ponto/services/ausencia.py:417-418`), duplicata vira AusenciaInvalida (`app/ponto/services/ausencia.py:430-435`), e espelho captura por dia e continua (`app/ponto/services/ausencia.py:1210-1218`). Interleaving estático possível; reprodução runtime **[nao verificado]**.

- **Ordem usual:** AG→PA (`app/ferias/services.py:560-564`), criação apenas PA (`app/ferias/services.py:493`), Ausencia depois. Exclusão inverte para PA→Ausencia→AG (`app/ferias/services.py:756-762`, `app/ferias/models.py:456-460`), mas os caminhos para cancelados recusam antes de pedir PA (`app/ferias/services.py:561-562`); deadlock desse caso **[nao verificado]**.

- **Fontes da folha mudam sem consultar exportada:** troca apaga Ausencias antigas e grava datas novas (`app/ferias/services.py:629-651`), histórico cria e materializa (`app/ferias/services.py:443-477`). A porta declara que período fechado não recusa férias (`app/ponto/services/ausencia.py:1169-1170`). Folha lê cobertura aprovada (`app/ponto/turnos.py:2516-2517`, `app/ponto/services/fechamento.py:683-687`); export lê datas inclusivas de Agendamento (`app/folha/export.py:491-493`) e altera retenção de quem teve batida (`app/folha/export.py:876-884`). Alteração de TXT já emitido ou de FechamentoMensal gravado **[nao verificado]**; a proteção deste último é distinta (`CLAUDE.md:411`).

- **Leitor com status errado:** KPI/lista próximos 30 dias filtra `aprovado` (`app/inteligencia/views.py:94`, `app/inteligencia/views.py:278`, `app/inteligencia/views.py:286`), enquanto escolhas de Agendamento são agendado/em_gozo/concluido/cancelado (`app/ferias/models.py:296-300`). Agendamento normal fica invisível nesse leitor.

- **Trilha fora do commit de correção:** correção de datas executa porta e só depois registra_log (`app/ferias/views.py:525`, `app/ferias/views.py:536`); atomic da porta termina antes (`app/ferias/services.py:649-652`). Falha posterior de trilha deixa mudança aplicada; `_trilha` obrigatório aparece somente no realinhamento/reversão (`app/ferias/services.py:161`, `app/ferias/services.py:182`).

Censo Ausencia ferias: cria aprovada por dia (`app/ponto/services/ausencia.py:1203-1208`), apaga no espelho (`app/ponto/services/ausencia.py:1190-1192`, `app/ponto/services/ausencia.py:1212-1215`), apaga janela (`app/ponto/services/ausencia.py:1263-1271`), rejeita admin (`app/ponto/services/ausencia.py:1282-1284`). Leitores diretos adicionais: guarda de sobreposição (`app/ferias/models.py:433-436`), lista com batida (`app/ferias/management/commands/listar_ferias_com_batida.py:40-64`), vigia de ausência (`app/ponto/services/vigia_ausencia.py:214-220`). Cobertura comum inclusiva e efeitos por destinatário: `app/ponto/turnos.py:2511-2535`, `app/ponto/turnos.py:2735-2782`.

## 13/11 17:50 · sessao aee2833cbbc9 · - · - · origem=Ronald (tmux)

1. **Cura 1: fecha a falha original de write/flush/fsync, mas a reversão continua incompleta.** O registro é escrito e sincronizado dentro do atomic externo antes da saída que confirma o período; erro desfaz o banco (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123`, `:127`, `:129`, `:130`). Os testes mordem as quatro falhas de escrita e preservação do primeiro período (`app/ferias/test_hj090_volta3.py:98`, `:111`). Execução desses testes: [nao verificado].

2. **BLOQUEIA — intenção não aplicada pode reverter um apply posterior.** Uma falha em fsync depois do flush pode deixar linha completa, com rollback do período; o teste reconhece essa possibilidade (`app/ferias/management/commands/realinhar_concedidos_ferias.py:129`; `app/ferias/test_hj090_volta3.py:107`). Contraexemplo: apply A falha; apply B, posterior, realinha o mesmo período e mesmos agendamentos; `--reverter` do arquivo A passa porque a guarda só compara concedidos e `[pk,dias]`, desfazendo B (`app/ferias/services.py:176`, `:179`). Também ocorre com apply A → reverter A → apply B → repetir reverter A. Não há identidade do ato nem conferência da trilha nessa guarda (`app/ferias/services.py:176`). Os testes existentes só alteram concedidos ou trocam o PK dos agendamentos (`app/ferias/test_hj090_um_juiz.py:149`, `:212`).

3. **BLOQUEIA — remarcação com os mesmos dias não invalida a reversão.** O contrato promete recusar período cujo agendamento foi remarcado (`app/ferias/services.py:170`), mas a identidade contém somente PK e dias (`app/ferias/services.py:134`, `:137`). Contraexemplo: direito 30, concedidos 10, agendamento de 20; realinha para 20; remarca o mesmo agendamento para outra janela mantendo 20 dias; a remarcação preserva concedidos e `[pk,dias]` (`app/ferias/services.py:635`, `:636`); reverter o registro anterior passa e grava 10 (`app/ferias/services.py:176`, `:179`). O teste troca o agendamento por outro PK e não cobre esse caso (`app/ferias/test_hj090_um_juiz.py:212`).

4. **NAO-BLOQUEIA/limitação de integridade — parser valida sintaxe, não estrutura.** Cauda sem newline só pode ser a última linha; JSON sintaticamente inválido em linha completa provoca recusa antes de qualquer reversão (`app/ferias/management/commands/realinhar_concedidos_ferias.py:43`, `:49`, `:51`, `:81`). Portanto a hipótese de ignorar JSON inválido no meio **não procede nesse sentido**. Contudo `{}` com newline no meio é JSON válido, entra no registro e causa `KeyError` apenas no processamento; períodos anteriores já podem ter sido revertidos (`app/ferias/management/commands/realinhar_concedidos_ferias.py:49`, `:82`, `:83`). O teste cobre somente `{invalido}\n` (`app/ferias/test_hj090_volta3.py:150`, `:155`).

5. **Rollback ancestral: possível na API, caller produtivo adicional não demonstrado.** O command usa atomic comum, sem impedir aninhamento; seu arquivo permanece se um atomic ancestral posteriormente desfizer o banco (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123`, `:130`). A documentação o chama expressamente de intenção sem confirmação (`:13`). O teste herda de `TestCase`, logo não prova commit efetivo por período (`app/ferias/test_hj090_volta3.py:81`; `app/ferias/test_hj090_um_juiz.py:63`). Caller produtivo que envolva esse command em outro atomic: [nao verificado]. A porta pública escreve sem arquivo, mas grava trilha obrigatória; único caller produtivo localizado é o command (`app/ferias/services.py:150`, `:161`, `:164`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:124`).

6. **Mordida dos testes:** curas de direito, trava, foto velha, vendidos, contador nos dois sentidos e exceção específica possuem casos que atingem seus mutantes naturais (`app/ferias/test_hj090_volta3.py:166`, `:173`, `:181`, `:206`, `:234`, `:241`). Não há mordida para retirar fsync dos diretórios: a injeção só falha no descritor do arquivo (`app/ferias/test_hj090_volta3.py:71`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:116`). A cura da mensageria não é exercitada por volta3; retirar o repasse não muda seus asserts (`mensageria/nucleo/ferramentas.py:2114`; `app/ferias/test_hj090_volta3.py:206`, `:245`).

7. **Costura do placar: não identifiquei perda de prova anterior.** A costura acrescenta somente a nova derivação externa; mantém command, gravação real, argumentos do contador e asserts anteriores (`app/core/tests/test_fio_mudo_placar.py:116`, `:129`, `:131`, `:135`, `:136`). A integração de férias tem teste separado com contador real envolvido por `wraps`, dados divergentes 1×2 e conferência do carimbo (`app/ferias/test_hj090_volta3.py:206`, `:215`, `:219`, `:224`).

8. **Possível confusão entre tenants — evidência de desenho, ocorrência não verificada.** O registro não contém schema, apenas período/antes/depois; reversão procura o PK no tenant corrente (`app/ferias/services.py:164`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:83`). Se dois tenants tiverem PKs e `[pk,dias]` iguais, arquivo do primeiro pode alterar o segundo (`app/ferias/services.py:176`, `:179`). Invocação equivocada em produção: [nao verificado].

## 13/11 17:50 · sessao 7f1417515870 · - · - · origem=Ronald (tmux)

Achados e evidências do recorte:

1. **OK — triagem2 #3, contador no carimbo.** O produtor usa `ferias_concedidos_x_soma`, grava ambos sentidos e declara esperado zero; o leitor compara todas as chaves de `esperado` e publica divergências sem limiar próprio. Contraexemplo removido: soma>concedidos agora aparece no ESTADO. `app/chamados/management/commands/vigiar_fio_morto.py:98-99,227-246`; `bin/fio_estado.py:121-126,191-194`; `bin/gerar_estado.py:260-261`.

2. **OK — triagem2 #4, mensageria.** `saldo_negativo_dias` passa do resultado do juiz para API e ferramenta, preservando inclusive o número negativo. `app/ferias/situacao.py:297-302`; `app/api/views_mensageria.py:835-836`; `mensageria/nucleo/ferramentas.py:2113-2115`. O novo teste `CarimboFerias` cobre contadores distintos `(1,2)`, esperado zero e chamada ao contador real; morde omissão, zero fixo e troca de sentidos. `app/ferias/test_hj090_volta3.py:206-229`. Execução dos testes [nao verificado].

3. **NAO-BLOQUEIA — relógio ainda escapa no vigia, preexistente.** O comentário promete uma leitura; `handle` captura `agora`, mas não o transmite a `censo_alcance`. Este chama `alimentar_portas` sem `agora`, que chega como `None` à fila e provoca novo `timezone.now()`. Contraexemplo: instante capturado 23:59:59, chamado silenciado até 00:00:00; consulta posterior já o considera porta, carimbo assina instante/dia anterior. `app/chamados/management/commands/vigiar_fio_morto.py:162-176,232-233`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/juizes.py:1576,1531-1543`; `app/chamados/services/painel.py:190`. O contador novo de férias não usa relógio. `app/ferias/services.py:116-127`.

4. **NAO-BLOQUEIA — dois relógios no command novo.** `listar_ferias_com_batida` lê `localdate()` e `now()` separadamente. Contraexemplo: virada da meia-noite entre chamadas entrega `hoje` do dia anterior e `agora` do seguinte ao carregador e à ordenação de competências. `app/ferias/management/commands/listar_ferias_com_batida.py:36,52,61`.

5. **NAO-BLOQUEIA — motivo incompleto em FORA_DE_PIPELINE.** `listar_ferias_dp` é descrito como “só leitura, sem efeito em dado”, porém `--simular` cancela via porta e escreve dentro de transação desfeita. A própria recusa da flag admite a escrita; só aceita `DATABASES.default.NAME == 'sombra'`. Persistência dessas alterações não demonstrada: há rollback explícito e tratamento depois do atomic. `app/config/crons.py:774-776`; `app/ferias/management/commands/listar_ferias_dp.py:52-53,98-115`. A guarda não confere `HOST`, embora a configuração da sombra declare nome e host como requisitos de isolamento. `app/config/settings/sombra.py:29-50`.

   Os motivos dos outros dois são compatíveis: lista exige `--desde`, consulta ausências/batidas e imprime; realinhamento sai antes da escrita no DRY e exige autor para apply/reverter. `app/config/crons.py:771-779`; `app/ferias/management/commands/listar_ferias_com_batida.py:24-25,40-69`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:65-85,95-109`. Não há fundamento no desenho 4a para transformar reparo pontual em rotina; o desenho determina vigia de varredura, sem reparo. `CLAUDE.md:169-173`.

6. **OK — costura de fio mudo preserva a prova anterior identificável.** A costura nova dubla somente a derivação de férias; o contador de fio mudo continua com chamada e valores afirmados, incluindo esperado diferente de zero, dia civil local e alarme no placar. `app/core/tests/test_fio_mudo_placar.py:129-139,230-247`. O placar continua consumindo exclusivamente as chaves fio mudo e nunca bateu; essa costura não prova alarme de férias no placar, nem o código atual oferece esse alarme. `app/core/management/commands/pipeline_placar.py:85-107,135-136`. Antes/depois da execução da suite [nao verificado].

7. **NAO-BLOQUEIA — leitores antigos ainda divergem do significado declarado de férias vigentes.** A autoridade inclui qualquer agendamento não cancelado coberto pelas datas, explicitamente inclusive concluído com data viva; `situacao_de_ferias` e `alertar_retornos` filtram só agendado/em_gozo. Contraexemplo: concluído com datas no presente é férias para o juiz e folha, mas some da lista da API e do aviso de retorno. `app/ferias/services.py:49-64`; `app/ferias/situacao.py:165-169,303`; `app/api/views_mensageria.py:843-846`; `app/ferias/management/commands/alertar_retornos.py:20-22`.

8. **NAO-BLOQUEIA — derivação paralela de vencimento, preexistente.** `periodos_vencendo` exige `status='adquirido'` persistido; o juiz `vencendo` pergunta `status_de_periodo` ao vivo. Contraexemplo: aquisição completou e cron ainda não atualizou em_curso: modelo diz vencendo, service omite. `app/ferias/services.py:201-209`; `app/ferias/models.py:255-260`. Janelas de 60 dias também continuam separadas em `services`, `situacao` e `pautas_de_ferias`, apesar de existir `DIAS_VENCENDO` no modelo. `app/ferias/services.py:45`; `app/ferias/situacao.py:55,206-209`; `app/ferias/management/commands/pautas_de_ferias.py:68-74`; `app/ferias/models.py:249,255`.

9. **NAO-BLOQUEIA — command escreve sem --apply, já listado na triagem2.** `alertar_ferias_vencendo` chama sincronização que pode gravar status e direito; não declara argumentos de apply e está no cron sem flag. `app/ferias/management/commands/alertar_ferias_vencendo.py:14-32`; `app/ferias/models.py:152-158`; `app/config/crons.py:708`. A mesma rotina também envia push e relata falha individual sem apagar o erro da saída. `app/ferias/management/commands/alertar_ferias_vencendo.py:84-88`.

## 13/11 17:46 · sessao a92e0d0adb4b · E3-FORMA-SQL · leitura · origem=Code

**Resultado: 0 regressões demonstradas pelos casos E3 escritos; os 79 ficam em “sem caso escrito para a combinação observada”.** Isso deixa a decisão normativa desses 79 em aberto; a perda de porta está demonstrada, mas sua classificação como violação não. (`v5_det.txt:1–79`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

Referências abreviadas:

- **V:n** = `v5_det.txt:n`, contando **somente as linhas `V5_DET`** fornecidas.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **J:n** = [app/chamados/juizes.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py):n.
- **F:n** = [app/chamados/tests/e3_fixtures.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/e3_fixtures.py):n.

**1–2. Classes, contagens, exemplos e cobertura.**

A classificação abaixo agrupa multiplicidade e ordem das razões; preserva seus conjuntos, módulo/status e diferenças de âncora e ações. `P=passivo_sem_linha`, `A=aceso`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`. (`V:1–79`)

Na coluna âncora: **C** = presente, `fila_ids=true/fila_cel=true`; **G** = presente, `true/false`; **X** = presente, `false/false`; **—** = nenhuma pergunta/âncora exposta nas instâncias. Isso não comprova inexistência de perguntas suprimidas pela tarefa. (`V:1–79`; `S:262–271`)

Módulos: **TA**=`turno_aberto`, **BA**=`batida_ausente`, **DS**=`disputa_supervisao`, **AC**=`ausencia_colab`, **PR**=`par_relampago`, **TA24**=`turno_aberto_24h`; status **a**=`aberto`, **e**=`em_analise`. A coluna descreve os chamados relacionados no V5; módulo/status de âncoras não relacionadas ali permanecem **[não verificado]**. (`V:1–79`)

**A coluna de pks é exaustiva; seus primeiros dois elementos são os exemplos. Classes unitárias só têm um exemplo disponível.** (`V:1–79`)

| Classe | Rota ATUAL | Módulo/status | Razões; âncora; ações | n | celula_pk — lista completa e evidência | Cobertura |
|---|---|---|---|---:|---|---|
| C01 | fila | TA/e | A,R; X; responder/colaborador/null | 1 | 141714 (V:1) | SC-R |
| C02 | fila | TA/e | A,P; C; vazias | 2 | 130162 (V:2), 135778 (V:17) | SC-P |
| C03 | fila | BA/e | A,P,R; R em X, P em X e C; responder/colaborador/null | 1 | 118931 (V:3) | SC-RP |
| C04 | fila | BA/a | A,P; C; vazias | 6 | 140522 (V:4), 140524 (V:5), 140707 (V:7), 119517 (V:10), 122073 (V:65), 128516 (V:69) | SC-P |
| C05 | fila | BA/a | P; C; vazias | 12 | 140928 (V:8), 104784 (V:9), 129384 (V:12), 104040 (V:13), 116436 (V:23), 106963 (V:33), 124364 (V:43), 124370 (V:44), 123033 (V:46), 132144 (V:61), 106350 (V:64), 124774 (V:73) | SC-P |
| C06 | fila | BA/e + DS/a | A,P; C e G, incluindo linha sintética; vazias | 1 | 139432 (V:6) | SC-P |
| C07 | fila | TA/e | A; —; vazias | 9 | 140331 (V:11), 135067 (V:18), 110506 (V:25), 138672 (V:26), 138912 (V:39), 138914 (V:40), 138916 (V:41), 140580 (V:42), 106594 (V:63) | SC-A |
| C08 | fila | BA/e | A,P; C; vazias | 14 | 133364 (V:14), 111346 (V:15), 111845 (V:16), 116432 (V:22), 129745 (V:28), 129749 (V:29), 129751 (V:30), 137166 (V:31), 33784 (V:47), 128519 (V:70), 141797 (V:74), 143313 (V:77), 143393 (V:78), 143395 (V:79) | SC-P |
| C09 | fila | AC/a | A; —; vazias | 2 | 129083 (V:19), 129090 (V:20) | SC-A |
| C10 | fila | AC/e | A; —; vazias | 1 | 129093 (V:21) | SC-A |
| C11 | fila | TA/a | A; —; vazias | 1 | 137154 (V:24) | SC-A |
| C12 | pergunta | DS/e | A,P; G; vazias | 1 | 113307 (V:27) | SC-P |
| C13 | fila | TA/a | A,R; X; responder/colaborador/null | 1 | 129598 (V:32) | SC-R |
| C14 | fila | BA/a | A; —; vazias | 9 | 130931 (V:34), 130933 (V:35), 130935 (V:36), 130939 (V:38), 129567 (V:54), 129569 (V:55), 129573 (V:57), 129577 (V:58), 130781 (V:60) | SC-A |
| C15 | fila | BA/e | A; —; vazias | 4 | 130937 (V:37), 129559 (V:50), 129561 (V:51), 130026 (V:66) | SC-A |
| C16 | fila | TA/e + DS/a | A; —; vazias | 1 | 129217 (V:45) | SC-A |
| C17 | pergunta | DS/a | P,W; G; vazias | 2 | 130346 (V:48), 130348 (V:49) | SC-PW |
| C18 | fila | BA/e + PR/a | A; —; vazias | 1 | 129563 (V:52) | SC-A |
| C19 | fila | BA/a | A,P; C, em linhas sintéticas; vazias | 1 | 129565 (V:53) | SC-P |
| C20 | fila | BA/e + DS/a | A; —; vazias | 2 | 129571 (V:56), 130044 (V:67) | SC-A |
| C21 | fila | BA/e | A,V; C; vazias | 1 | 130771 (V:59) | SC-V |
| C22 | fila | BA/a + TA24/a | A,P; C; vazias | 1 | 132052 (V:62) | SC-P |
| C23 | fila | BA/e | P; C e G; vazias | 1 | 120325 (V:68) | SC-P |
| C24 | pergunta | DS/a | P; G; vazias | 2 | 128533 (V:71), 127477 (V:72) | SC-P |
| C25 | fila | BA/e | A,R; X; responder/colaborador/null | 1 | 141810 (V:75) | SC-R |
| C26 | fila | TA/e + BA/e | A,J; C; vazias | 1 | 142013 (V:76) | SC-J |
| **Total** | | | | **79** | **V:1–79** | |

Os códigos de cobertura significam:

| Cobertura | Caso escrito confrontado | O fato que falta para aplicar o caso | Decisão normativa pelos testes E3 | [nao verificado]
|---|---|---|---|
| SC-A | `test_rota_so_fila_sem_pergunta`, T:32–38 | O teste monta marco **apagado** e exige `perguntar/sistema/emissor`; aqui todas as instâncias expostas estão acesas, sem ações. | **Sem caso escrito** para estes fatos; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-P | `test_porta_fila_quando_ancora_e_da_celula`, T:67–73; caso A/B, T:21–47 | Falta um caso que imponha porta **apesar de passivo sem linha e ações vazias**. A fixture do teste cria pergunta sem resposta/validação; não constrói passivo. (`F:65–78`) | **Sem caso escrito** para esta combinação; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-R | `test_fila_A_pergunta_sob_B`, T:21–47 | O caso exige B como âncora viva e ação com porta B; no V5 a âncora está fora de `fila_ids`, com porta da ação `null`. | **Sem caso escrito** para B fora da fila; ATUAL/FUTURO indeterminado. |
| SC-RP | Mesmos casos, T:21–47,67–73 | Faltam simultaneamente o caso de B fora da fila e o de passivo com ações vazias no mesmo dia. | **Sem caso escrito** para a combinação. |
| SC-PW | Caso A/B, T:21–47; âncora da célula, T:67–73 | Falta cenário de dia sem trabalho com passivo, âncora global e nenhuma ação; também não há A residual na fila da célula. | **Sem caso escrito** para a combinação. |
| SC-V | T:67–73 | Falta cenário de validação sem lavra vigente que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |
| SC-J | T:67–73 | Falta cenário de encerramento pelo juiz que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |

O caso de isenção não se aplica aos fatos expostos: ele exige `razao='isento_ponto'`, ausente nos 79. Os testes de relógio também exigem valores de silêncio e instantes específicos não fornecidos para essas âncoras. (`T:57–65,84–111`; `V:1–79`) [nao verificado]

**3. REGRESSÃO: lista completa demonstrável = `[]`; quantidade = 0.** Nenhuma classe reproduz integralmente um caso E3 que imponha porta. Aplicar T:67–73 apenas pela presença da âncora descartaria o passivo e as decisões que distinguem os fatos. (`T:21–127`; `F:65–78`; `V:1–79`)

O caminho concreto que elimina a porta está identificado, mesmo sem obrigação normativa demonstrada:

| Classes | Onde a cópia deixa de publicar porta |
|---|---|
| C07, C09–C11, C14–C16, C18, C20 | A linha nasce com ações vazias; marco aceso recebe `aceso` e encerra sua passagem pelo laço. (`S:258–271`) |
| C02, C04–C06, C08, C12, C17, C19, C22–C24 | Passivo devolve `acoes=None`; a tarefa mantém a lista vazia e marca `nao_sei`. Exposição administrativa segue a mesma recusa de inventar ação. (`S:130–131,249–255,308–313`) |
| C01, C13, C25 | A âncora só vira porta se estiver na fila; fora dela, a ação `responder` recebe `porta=None`. (`S:128–133`) |
| C03 | Combina os dois caminhos anteriores: resposta sem porta e passivo sem ação. (`S:128–133,308–313`; `V:3`) |
| C21 | Sem lavra vigente, retorna razão e `acoes=None`. (`S:143–146,308–313`) |
| C26 | Encerramento pelo juiz retorna ações vazias. (`S:149–151,313`) |
| Todas | `_portas_da_tarefa` aceita somente portas inteiras pertencentes a `fila_ids`; sem portas, escreve `rota=None`; `porta_da_tarefa` devolve `None`. (`S:547–565`) |

Há contratos E2 que corroboram essas distinções: passivo exige ações vazias; marco aceso dispensa ação; âncora silenciada admite `responder` com `porta=None`. O motivo concreto de exclusão das quatro âncoras V5 da fila permanece **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409,487–495,867–873`; `V:1,3,32,75`)

**4. Restantes: 79 sem caso escrito completo; 0 demonstrados como “caso E3 que manda FUTURO”.** A lista completa dos 79 é a coluna de pks das classes C01–C26, sem exclusões; a lista do segundo conjunto é `[]`. (`V:1–79`; `T:21–127`)

A conferência factual fecha em **30 somente acesos; 41 com passivo e eventual aceso; 2 com passivo e dia sem trabalho; 3 com aceso e resposta; 1 com aceso, passivo e resposta; 1 sem lavra vigente; 1 encerrado pelo juiz = 79**. Há **75 dias com ações vazias e 4 com ações não vazias**. (`V:1–79`)

**5. REGRESSÃO demonstrada com dono ESTRUTURA: 0; lista `[]`.** Isso decorre do conjunto de regressões demonstradas estar vazio, sem necessidade de redistribuir os donos registrados na volta 4. (`V:1–79`; `T:21–127`)

O agregado **ESTRUTURA 44 / CADASTRO 6 / BATIDA 29** não fornece o pareamento individual dos 79 com seus donos. Uma lista individual dos 44 ESTRUTURA, derivada apenas desse agregado, seria **[não verificado]**. (`a1v4_julga.r:16`, contando as linhas textuais do anexo fornecido)

**6. Quem escreve, quem lê e diferenças de significado.**

| Campo | Quem escreve/calcula | Quem lê |
|---|---|---|
| `ancora` exibida no V5 | A tarefa **não escreve esse campo nas instâncias**; escreve `pergunta_id` e `acoes`. Calcula localmente `anc=chamado_que_lista(p)`. A implementação do enriquecimento da sonda é **[não verificado]**. (`S:128,258–260,318–320`) | A tarefa resolve a âncora pela pergunta, sem ler `instancia['ancora']`. (`S:127–129`) |
| Identidade da âncora | `chamado_que_lista` retorna `chamado_destino_id`, com fallback para `disputa.chamado_id`. (`app/chamados/services/fio.py:97–117`) | A carga consulta a fila dessas âncoras; `_pela_decisao` usa sua pertença para preencher a porta. (`S:415–418,128–133`) |
| `acoes[].acao/de/porta` | `_acao` monta os campos; `_pela_decisao` escolhe ações; `_tarefa_pura` as publica nas instâncias. (`S:114–162,249–255,302–313`) | `_portas_da_tarefa` lê porta e dono; o backfill lê razões e ações, inclusive humanos sem porta. (`S:547–556`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`) |
| `fila_ids` / `fila_cel` | `alimentar_fila` constrói a fila global com verbo e o agrupamento por célula. (`J:1558–1567`) | C17 admite chamado da célula e âncora global; tarefa usa a fila global para autorizar porta e a fila da célula para escolher rota/subconjunto `fila`. (`J:1677–1687`; `S:548–555`) |
| Verbo do ATUAL | `particionar_verbos` calcula o mapa por módulo, créditos das perguntas e silêncio; `alimentar_fila` executa a partição sobre `qs_vivos()`. (`app/chamados/services/verbos.py:30–52`; `J:1558`) | C17 copia o mapa; a tarefa copia o mesmo mapa para as portas selecionadas pelas ações. (`J:1678,1682`; `S:549`) |
| Porta publicada | `_portas_da_tarefa` escreve rota/fila/verbos/fio; `porta_da_tarefa` projeta esses campos ou `None`. (`S:547–565`) | Folha, pauta, vigia, beco e furo sem canal consomem essa projeção. (`app/folha/fora_da_folha.py:71–89`; `app/ponto/services/pautas_dos_discordantes.py:79–84`; `app/chamados/services/vigia_fio.py:640–654`; `app/escala/services/cadastro_realidade.py:492–503`; `app/chamados/services/furo_sem_canal.py:93–101`) |

As diferenças semânticas verificáveis são:

- **Chamado do dia versus chamado da pergunta:** a folha publica identificadores somente de `porta['fila']`, isto é, chamados da célula; as demais chaves de `verbos` podem incluir âncoras externas. Isso está explicitado no leitor. (`app/folha/fora_da_folha.py:235–242`; `S:549–551`)
- **Fila global versus fila da célula:** uma âncora G pode autorizar `porta_pergunta`; não precisa pertencer a `fila_cel`. Esses conjuntos têm funções distintas no código. (`S:548–555`; `J:1677–1687`)
- **Verbo versus dono:** C17 calcula fio pelos verbos; a tarefa calcula fio pelos donos das ações. O teste admite expressamente `responder` junto de verbo `validar`; o vigia preserva essa distinção. (`J:1633–1639,1688`; `S:552–556`; `T:75–82`; `app/chamados/services/vigia_fio.py:649–653`)
- **Ausência de porta versus ninguém esperando:** a documentação da pauta diz “não estão esperando NINGUÉM”, mas seu predicado efetivo lê ausência de porta. A tarefa pode ter ação humana sem porta ou `nao_sei`; a folha já descreve essa limitação. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `S:129–133,544,562–565`; `app/folha/fora_da_folha.py:52–58`)
- **Rótulo do instrumento versus caso escrito:** `casos_do_par` chama de `so_fila_sem_pergunta` qualquer `fila and not vivas`; não exige marco apagado nem ação `perguntar`. Esse rótulo, sozinho, não comprova cobertura por T:32–38. (`app/chamados/management/commands/diff_e3_sombra.py:63–70`; `T:32–38`)

## 13/11 17:45 · sessao a0cfb94c0d12 · - · - · origem=Ronald (tmux)

Contagem conferida: **79 colab-dias, 75 com ações vazias e 4 com ações não vazias**. Os quatro são `141714`, `118931`, `129598`, `141810`. (`v5_det.txt:1,3,32,75`, contando somente linhas `V5_DET`; demais linhas V5:1–79)

A tabela preserva módulo/status e separa misturas e linhas sintéticas. Ignora somente multiplicidade e ordem das razões. Abreviações: `A=aceso`, `P=passivo_sem_linha`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`; `e=em_analise`, `a=aberto`. `mesma` significa âncora em ambas as filas; `global` significa `fila_ids=True/fila_cel=False`; `fora` significa ambos falsos. Módulos abreviados por prefixo inequívoco. (`v5_det.txt:1–79`)

| # | Rota | Módulo/status | Razões/âncora/ações | n | Lista completa com linha V5 |
|---|---|---|---|---:|---|
| 1 | fila | turno_aberto/e | A,R; R fora; responder/null | 1 | 141714:1 |
| 2 | fila | turno_aberto/e | A,P; mesma; vazias | 2 | 130162:2, 135778:17 |
| 3 | fila | batida_ausente/e | A,P,R; R fora; P fora e mesma; responder/null | 1 | 118931:3 |
| 4 | fila | batida_ausente/a | P ou A,P; mesma; vazias | 18 | 140522:4, 140524:5, 140707:7, 140928:8, 104784:9, 119517:10, 129384:12, 104040:13, 116436:23, 106963:33, 124364:43, 124370:44, 123033:46, 132144:61, 106350:64, 122073:65, 128516:69, 124774:73 |
| 5 | fila | batida_ausente/e + disputa_supervisao/a | A,P; mesma+global, sintética; vazias | 1 | 139432:6 |
| 6 | fila | turno_aberto/e | A; sem âncora; vazias | 9 | 140331:11, 135067:18, 110506:25, 138672:26, 138912:39, 138914:40, 138916:41, 140580:42, 106594:63 |
| 7 | fila | batida_ausente/e | P ou A,P; mesma; vazias | 14 | 133364:14, 111346:15, 111845:16, 116432:22, 129745:28, 129749:29, 129751:30, 137166:31, 33784:47, 128519:70, 141797:74, 143313:77, 143393:78, 143395:79 |
| 8 | fila | ausencia_colab/a | A; sem âncora; vazias | 2 | 129083:19, 129090:20 |
| 9 | fila | ausencia_colab/e | A; sem âncora; vazias | 1 | 129093:21 |
| 10 | fila | turno_aberto/a | A; sem âncora; vazias | 1 | 137154:24 |
| 11 | pergunta | disputa_supervisao/e | A,P; global; vazias | 1 | 113307:27 |
| 12 | fila | turno_aberto/a | A,R; fora; responder/null | 1 | 129598:32 |
| 13 | fila | batida_ausente/a | A; sem âncora; vazias | 9 | 130931:34, 130933:35, 130935:36, 130939:38, 129567:54, 129569:55, 129573:57, 129577:58, 130781:60 |
| 14 | fila | batida_ausente/e | A; sem âncora; vazias | 4 | 130937:37, 129559:50, 129561:51, 130026:66 |
| 15 | fila | turno_aberto/e + disputa_supervisao/a | A; sem âncora; vazias | 1 | 129217:45 |
| 16 | pergunta | disputa_supervisao/a | P,W; global; vazias | 2 | 130346:48, 130348:49 |
| 17 | fila | batida_ausente/e + par_relampago/a | A; sem âncora; vazias | 1 | 129563:52 |
| 18 | fila | batida_ausente/a | A,P; mesma, sintéticas; vazias | 1 | 129565:53 |
| 19 | fila | batida_ausente/e + disputa_supervisao/a | A; sem âncora; vazias | 2 | 129571:56, 130044:67 |
| 20 | fila | batida_ausente/e | A,V; mesma; vazias | 1 | 130771:59 |
| 21 | fila | batida_ausente/a + turno_aberto_24h/a | A,P; mesma; vazias | 1 | 132052:62 |
| 22 | fila | batida_ausente/e | P; mesma+global; vazias | 1 | 120325:68 |
| 23 | pergunta | disputa_supervisao/a | P; global; vazias | 2 | 128533:71, 127477:72 |
| 24 | fila | batida_ausente/e | A,R; fora; responder/null | 1 | 141810:75 |
| 25 | fila | turno_aberto/e + batida_ausente/e | A,J; mesma; vazias | 1 | 142013:76 |
| **Total** | | | | **79** | V5:1–79 |

**Atenção de precisão:** classes 4 e 7 agrupam conjuntos `P` e `A,P`; se “padrão” exigir igualdade exata do conjunto de razões, devem ser subdivididas. A tabela preserva os fatos no texto de cada classe e permite duas amostras nos grupos de n≥2; grupos unitários não têm segundo exemplo. (`v5_det.txt:4–10,12–16,22–23,28–31,33,43–47,61,64–65,69–70,73–74,77–79`)

Consolidação ampla útil: **30 somente A; 41 com P e eventual A; 2 P,W; 3 A,R; 1 A,P,R; 1 A,V; 1 A,J =79**. (`v5_det.txt:1–79`)

## 13/11 17:43 · sessao 0fcc9646b8a7 · - · - · origem=Ronald (tmux)

| Campo | Escritor | Leitor e significado |
|---|---|---|
| Âncora da pergunta | `app/chamados/services/fio.py:97–117`: `chamado_destino_id`, com fallback para `disputa.chamado_id`. A tarefa a calcula em variável local, `app/chamados/services/tarefa_do_dia.py:127–129`. | `_pela_decisao` atribui essa âncora à porta da ação somente quando pertence à fila recebida, `tarefa_do_dia.py:129`. A carga forma essa fila consultando `qs_na_fila` sobre as âncoras das perguntas, `tarefa_do_dia.py:415–418`. |
| Campo `ancora` do V5 | Não consta da construção das instâncias em `tarefa_do_dia.py:258–260,318–320`. O V5 o descreve como enriquecimento da sonda; implementação dessa escrita **[não verificado]**. | A tarefa não lê um campo `instancia['ancora']`; calcula a âncora pelo objeto pergunta, `tarefa_do_dia.py:127–129`. |
| `acoes[].acao/de/porta` | `_acao` monta os três campos, `tarefa_do_dia.py:114–115`; `_pela_decisao` escolhe ações conforme decisão/passivo, `:128–162`; `_tarefa_pura` publica nas instâncias, `:249–255,302–313`. | `_portas_da_tarefa` achata as ações, mantém portas inteiras pertencentes à fila global, lê o verbo do painel e calcula fio pelos donos, `:547–556`. O backfill lê razões/ações/portas, inclusive humanos com `porta=None`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`. |
| `fila_ids` e `fila_cel` | `app/chamados/juizes.py:1558–1567`: `fila_ids` contém a fila global com verbo; `fila_cel` agrupa chamados por `celula_id` nas células pedidas. | C17 lê chamados da célula e soma âncoras da pergunta pertencentes à fila global, `juizes.py:1677–1682`. A tarefa usa `fila_ids` para autorizar porta e `fila_cel` somente para determinar `porta_fila` e o subconjunto `fila`, `tarefa_do_dia.py:548–555`. |
| Verbo do ATUAL | `app/chamados/services/verbos.py:30–52`: partição usa módulo, créditos de perguntas e silêncio. `juizes.py:1558` executa sobre `qs_vivos()`. | C17 copia esses verbos em `juizes.py:1678,1682`; traduz verbo em fio por `:1633–1639,1688`. A tarefa copia o mesmo mapa para portas escolhidas pelas ações, mas calcula fio pelos donos das ações, `tarefa_do_dia.py:549,552–556`. |

Leitores de produção da porta publicada: [nao verificado]

- Folha lê `porta_da_tarefa`, fio e `fila`, sem reconstruir âncora: `app/folha/fora_da_folha.py:71–89,233–242`.
- Pauta lê o fio projetado pela folha: `app/ponto/services/pautas_dos_discordantes.py:79–84`.
- Vigia lê rota/fio; explicita que verbo do painel não decide quem age: `app/chamados/services/vigia_fio.py:640–654`.
- Beco exige ausência de porta da tarefa e mantém a âncora da pergunta separadamente: `app/escala/services/cadastro_realidade.py:492–503`.
- Furo sem canal lê presença de porta/rota e distingue `nao_sei`: `app/chamados/services/furo_sem_canal.py:93–101`.
- Instrumento de sombra compara C17 com a projeção `porta_da_tarefa`: `app/chamados/management/commands/diff_e3_sombra.py:127–130`.

Há descrição mais ampla que o predicado na pauta: “não estão esperando NINGUÉM” e “pergunta viva [...] chamado vivo JA tem dono esperando”, embora o código efetivo teste ausência de porta publicada; esta pode existir com ações humanas sem porta. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `app/chamados/services/tarefa_do_dia.py:129,133,548–565`)

A folha limita o identificador nomeado ao chamado da célula publicado em `fila`, preservando a distinção entre chamado do dia e âncora da pergunta; não nomeia todas as chaves de `verbos`. (`app/folha/fora_da_folha.py:235–242`; `app/chamados/juizes.py:1654–1657`)

Cuidado normativo: `test_porta_fila_quando_ancora_e_da_celula` constrói pergunta sem resposta/passivo e exige verbo `cobrar`; não escreve o cenário `passivo_sem_linha + acoes=[] + validar/decidir`. O caso A/B exige B vivo na fila e ação com porta B. Os quatro dias humanos do V5 têm âncora fora de ambas as filas, portanto não reproduzem esse caso. (`app/chamados/tests/test_e3_portas_tarefa.py:21–47,67–73`; `v5_det.txt:1,3,32,74`, contando somente `V5_DET`; conferir numeração com a recontagem do agente principal)

_4732 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
