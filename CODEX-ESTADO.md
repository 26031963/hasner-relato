# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 15:21** · publicado em **18/11/2026 15:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 15:21 · sessao 3a4db752191d · FILA-CODEX-1811 · leitura · origem=Code

**O classificador abaixo é executável, mas a conferência do HEAD ficou limitada:** a cópia disponível não tem metadados Git; portanto, não pude confirmar que ela corresponde ao hash informado [não verificado]. As definições e os sítios citados foram conferidos na cópia, por exemplo `app/chamados/models.py:33`, `app/chamados/models.py:1847` e `app/chamados/models.py:2593`.

A contagem das 1.588 linhas e a lista B resultante ficam [não verificado]: não executei o classificador sobre um arquivo TSV completo disponível na cópia. O texto fornecido mostra linhas do levantamento, mas não permite confirmar sua equivalência ao arquivo integral [não verificado]. [nao verificado]

A tabela usa estas abreviações. O conjunto A é limitado aos métodos da **classe dona**, aos juízes registrados indicados e às funções de porta enumeradas; uma chamada indireta em outro local não vira A só por chamar a porta.

- **CC** — métodos `ChamadoColaborador.*` em `app/chamados/models.py`; porta `_gravar_estado` em `app/chamados/models.py:207`. O registro de ciclo está em `app/core/portas.py:196` e `app/core/portas.py:221`.
- **DS** — métodos `DisputaSupervisao.*` em `app/chamados/models.py`; ciclo em `app/chamados/models.py:2017`, `:2289`, `:2326`, `:2345` e `:2413`; emissão/agregação de motivos em `app/chamados/services/disputa_emissao.py` conforme `app/core/portas.py:425` e `:433`.
- **PD** — métodos `PerguntaDisputa.*` em `app/chamados/models.py`. A porta do carimbo inclui `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote`: `app/chamados/models.py:3299`, `:3364`, `:3399`, `:3448`; declaração em `app/core/portas.py:243`.
- **Resposta** — `responder_pergunta_disputa_core`, `responder_automaticamente` e `carimbar_resposta_pela_trilha`, em `app/chamados/services/disputa_emissao.py:1439`, `:1662`, `:1692`; a declaração da porta está em `app/core/portas.py:259`.
- **Validação** — `_carimbar_validacao`, `validar_pergunta`, `recusar_pergunta` e `conferir_data_do_fato`, em `app/chamados/services/validacao.py:161`, `:685`, `:858`, `:957`; a declaração está em `app/core/portas.py:360`.
- **Desfecho** — as funções da porta de materialização enumeradas em `app/core/portas.py:275`: `_materializar_ausencia_de_resposta`, `_materializar_resposta_hora`, `_carimbar_recusa`, `_materializar_uma` e `materializar_pendentes_aprovadas`, todas em `app/chamados/services/materializacao.py`. A escrita de `pendencia_admin` aparece em `app/chamados/services/materializacao.py:1365`.
- **Lavra** — `lavrar` e `gravar_lavras_em_lote` em `app/chamados/veredito_lavrado.py:254` e `:240`; declaração dos três campos em `app/core/portas.py:332`.
- **J-resposta** — `pergunta_respondida`, registrada em `app/core/juizes.py:306` e definida em `app/chamados/juizes.py:1544`.
- **J-viva** — `pergunta_viva`, registrada em `app/core/juizes.py:291` e definida em `app/chamados/juizes.py:866`.

| Campo | Modelo dono e definição na cópia | Conjunto DONO / porta |
|---|---|---|
| `data_conferida_em` | PerguntaDisputa — `app/chamados/models.py:2645` | PD; Validação |
| `data_conferida_por` | PerguntaDisputa — `app/chamados/models.py:2646` | PD; Validação |
| `fechada_em` | DisputaSupervisao — `app/chamados/models.py:1894` | DS |
| `fechada_por` | DisputaSupervisao — `app/chamados/models.py:1903` | DS |
| `janela_expira_em` | PerguntaDisputa — `app/chamados/models.py:2783` | PD; Lavra |
| `julgado_em` | PerguntaDisputa — `app/chamados/models.py:2777` | PD; Lavra |
| `materializada_em` | PerguntaDisputa — `app/chamados/models.py:3647` | PD; Desfecho; J-viva |
| `materializada_por` | PerguntaDisputa — `app/chamados/models.py:3651` | PD; Desfecho |
| `motivo_fechamento` | DisputaSupervisao — `app/chamados/models.py:1930` | DS |
| `motivos` | DisputaSupervisao — `app/chamados/models.py:1878` | DS; emissão/agregação |
| `pendencia_admin` | PerguntaDisputa — `app/chamados/models.py:2743` | PD; Desfecho |
| `reaberturas_admin` | DisputaSupervisao — `app/chamados/models.py:1899` | DS |
| `reaberturas_colab` | DisputaSupervisao — `app/chamados/models.py:1895` | DS |
| `recusa_motivo` | PerguntaDisputa — `app/chamados/models.py:2747` | PD; Desfecho; Validação; Resposta. Escritor único [não verificado]; a declaração ressalva múltiplos escritores em `app/core/portas.py:384`. |
| `resolucao` | DisputaSupervisao — `app/chamados/models.py:1910` | DS |
| `resolvido_em` | ChamadoColaborador — `app/chamados/models.py:66` | CC |
| `respondida_em` | PerguntaDisputa — `app/chamados/models.py:2635` | PD; Resposta; J-resposta |
| `respondido_por` | ChamadoColaborador — `app/chamados/models.py:64` | CC |
| `resposta_colab` | PerguntaDisputa — `app/chamados/models.py:2634` | PD; Resposta; J-resposta |
| `status_local` | ChamadoColaborador — `app/chamados/models.py:58` | CC |
| `validada_em` | PerguntaDisputa — `app/chamados/models.py:2636` | PD; Validação |
| `validada_por` | PerguntaDisputa — `app/chamados/models.py:3631` | PD; Validação |
| `veredito_lavrado` | PerguntaDisputa — `app/chamados/models.py:2772` | PD; Lavra |
| `via_encerramento` | ChamadoColaborador — `app/chamados/models.py:60` | CC |
| `via_resolucao` | PerguntaDisputa — `app/chamados/models.py:2748` | PD; Desfecho. Escritor único [não verificado]; a declaração ressalva múltiplos escritores em `app/core/portas.py:356`. |

A regra C usa uma lista **fechada** dos commands identificados na cópia: `auditar_chamados_orfaos` e `auditar_disputas` declaram auditoria em `app/chamados/management/commands/auditar_chamados_orfaos.py:23` e `app/chamados/management/commands/auditar_disputas.py:56`; `auditar_invariantes_chamados` declara auditoria em `app/chamados/management/commands/auditar_invariantes_chamados.py:1`; `selo_leitores_no_mesmo_numero` declara selo de comparação em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:2`. **Não** incluí `estoque_l130_instante_sem_texto`: embora meça estoque, também declara uma ação de devolução, em `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:23`.

O script rejeita linhas malformadas ou campos fora da tabela, preserva duplicatas, aceita `template-indireto` e imprime cada linha B integralmente, agrupada por campo. A correspondência ignora somente o número de linha do sítio; compara caminho e função.

```python
#!/usr/bin/env python3
import collections
import sys

# Tabela literal: campo -> (classe dona, grupos de portas/juizes).
DONO = {
    "data_conferida_em": ("PerguntaDisputa", ("conferencia",)),
    "data_conferida_por": ("PerguntaDisputa", ("conferencia",)),
    "fechada_em": ("DisputaSupervisao", ()),
    "fechada_por": ("DisputaSupervisao", ()),
    "janela_expira_em": ("PerguntaDisputa", ("lavra",)),
    "julgado_em": ("PerguntaDisputa", ("lavra",)),
    "materializada_em": ("PerguntaDisputa", ("desfecho", "j_viva")),
    "materializada_por": ("PerguntaDisputa", ("desfecho",)),
    "motivo_fechamento": ("DisputaSupervisao", ()),
    "motivos": ("DisputaSupervisao", ("emissao_motivos",)),
    "pendencia_admin": ("PerguntaDisputa", ("desfecho",)),
    "reaberturas_admin": ("DisputaSupervisao", ()),
    "reaberturas_colab": ("DisputaSupervisao", ()),
    "recusa_motivo": (
        "PerguntaDisputa", ("desfecho", "validacao", "resposta")
    ),
    "resolucao": ("DisputaSupervisao", ()),
    "resolvido_em": ("ChamadoColaborador", ()),
    "respondida_em": ("PerguntaDisputa", ("resposta", "j_resposta")),
    "respondido_por": ("ChamadoColaborador", ()),
    "resposta_colab": ("PerguntaDisputa", ("resposta", "j_resposta")),
    "status_local": ("ChamadoColaborador", ()),
    "validada_em": ("PerguntaDisputa", ("validacao",)),
    "validada_por": ("PerguntaDisputa", ("validacao",)),
    "veredito_lavrado": ("PerguntaDisputa", ("lavra",)),
    "via_encerramento": ("ChamadoColaborador", ()),
    "via_resolucao": ("PerguntaDisputa", ("desfecho",)),
}

# Os pares sao (caminho, funcao), sem numero de linha.
GRUPOS = {
    "conferencia": {
        ("app/chamados/services/validacao.py", "conferir_data_do_fato"),
    },
    "validacao": {
        ("app/chamados/services/validacao.py", "_carimbar_validacao"),
        ("app/chamados/services/validacao.py", "validar_pergunta"),
        ("app/chamados/services/validacao.py", "recusar_pergunta"),
        ("app/chamados/services/validacao.py", "conferir_data_do_fato"),
    },
    "resposta": {
        ("app/chamados/services/disputa_emissao.py",
         "responder_pergunta_disputa_core"),
        ("app/chamados/services/disputa_emissao.py",
         "responder_automaticamente"),
        ("app/chamados/services/disputa_emissao.py",
         "carimbar_resposta_pela_trilha"),
    },
    "desfecho": {
        ("app/chamados/services/materializacao.py",
         "_materializar_ausencia_de_resposta"),
        ("app/chamados/services/materializacao.py",
         "_materializar_resposta_hora"),
        ("app/chamados/services/materializacao.py", "_carimbar_recusa"),
        ("app/chamados/services/materializacao.py", "_materializar_uma"),
        ("app/chamados/services/materializacao.py",
         "materializar_pendentes_aprovadas"),
    },
    "lavra": {
        ("app/chamados/veredito_lavrado.py", "lavrar"),
        ("app/chamados/veredito_lavrado.py", "gravar_lavras_em_lote"),
    },
    "emissao_motivos": {
        ("app/chamados/services/disputa_emissao.py",
         "_abrir_ou_agregar_disputa"),
        ("app/chamados/services/disputa_emissao.py",
         "criar_disputa_a_partir_de_chamados_orfaos"),
        ("app/chamados/services/disputa_emissao.py",
         "criar_disputa_ancora_de_chamado"),
    },
    "j_resposta": {
        ("app/chamados/juizes.py", "pergunta_respondida"),
    },
    "j_viva": {
        ("app/chamados/juizes.py", "pergunta_viva"),
    },
}

COMMANDS_C = {
    "app/chamados/management/commands/auditar_chamados_orfaos.py",
    "app/chamados/management/commands/auditar_disputas.py",
    "app/chamados/management/commands/auditar_invariantes_chamados.py",
    "app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py",
}

MODELS = "app/chamados/models.py"


def separar_sitio(sitio, numero_linha):
    partes = sitio.rsplit(":", 2)
    if len(partes) != 3 or not partes[0] or not partes[1]:
        raise ValueError(f"linha {numero_linha}: sitio invalido: {sitio!r}")
    caminho, funcao, linha = partes
    if not linha.isdecimal():
        raise ValueError(f"linha {numero_linha}: numero de sitio invalido")
    return caminho, funcao


def eh_c(caminho, contexto):
    componentes = caminho.split("/")
    return (
        contexto != "producao"
        or any(x in componentes for x in ("tests", "bin", "migrations"))
        or caminho in COMMANDS_C
    )


def eh_a(campo, caminho, funcao):
    classe, grupos = DONO[campo]
    if caminho == MODELS and funcao.startswith(classe + "."):
        return True
    return any((caminho, funcao) in GRUPOS[g] for g in grupos)


def main():
    cabecalho = sys.stdin.readline()
    if cabecalho.rstrip("\r\n") != "## NAO_COBERTO":
        raise ValueError("primeira linha deve ser ## NAO_COBERTO")

    contagem = collections.Counter()
    linhas_b = collections.defaultdict(list)

    for numero, linha in enumerate(sys.stdin, 2):
        texto = linha.rstrip("\r\n")
        if not texto:
            raise ValueError(f"linha {numero}: linha vazia")
        colunas = texto.split("\t", 6)
        if len(colunas) != 7:
            raise ValueError(f"linha {numero}: esperadas 7 colunas")
        campo, forma, sitio, tipo, marcador, contexto, trecho = colunas
        if campo not in DONO:
            raise ValueError(f"linha {numero}: campo desconhecido: {campo!r}")
        caminho, funcao = separar_sitio(sitio, numero)

        if eh_c(caminho, contexto):
            classe = "C"
        elif eh_a(campo, caminho, funcao):
            classe = "A"
        else:
            classe = "B"
            linhas_b[campo].append(texto)
        contagem[classe] += 1

    total = sum(contagem.values())
    for classe in ("A", "B", "C"):
        print(f"{classe}\t{contagem[classe]}")
    print(f"SOMA\t{total}")
    print("## B POR CAMPO")
    for campo in sorted(linhas_b):
        print(f"## {campo}\t{len(linhas_b[campo])}")
        for linha in linhas_b[campo]:
            print(linha)


if __name__ == "__main__":
    try:
        main()
    except (ValueError, BrokenPipeError) as exc:
        print(f"erro: {exc}", file=sys.stderr)
        sys.exit(1)
```

## 18/11 15:19 · sessao 9b47aedfec37 · FILA-CODEX-1811 · leitura · origem=Code

**Não encontrei gerador de AFD nem de AEJ na leitura do código.** O glossário declara a exportação AFD pendente, e o registro de trabalho afirma que não há gerador para nenhum dos dois; portanto, não há função, view, comando ou chamador de geração a apontar na busca realizada. ([glossário:155](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/core/glossario.html:155); [RELATO.md:3591](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3591)) [não verificado]

A rota `importar-aej/` chama `importar_aej`, mas ela responde HTTP 410; o importador Nexti está preservado como legado desativado. `ImportacaoAEJ` guarda arquivo e metadados **de importação**, não de geração. ([urls.py:9](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/urls.py:9); [views.py:54](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:54); [legacy/README.md:3](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/legacy/README.md:3); [models.py:200](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:200))

**Limites da leitura:** o diretório entregue não contém metadados Git utilizáveis; não foi possível confirmar localmente que seu conteúdo corresponde ao hash informado como HEAD. [não verificado] A leitura foi estática, sem consulta ao banco; assim, os campos abaixo indicam capacidade do modelo, não preenchimento dos dados. [não verificado]

O repositório consultado não fornece o leiaute normativo completo por tipo de registro; o próprio registro local diz que o requisito da Portaria não foi fornecido. As linhas abaixo são **categorias solicitadas para inventário**, não códigos ou afirmações de conformidade com a Portaria: **[nv] leiaute não está no repo**. ([RELATO.md:3591](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3591))

| Arquivo / categoria | Dado guardado hoje ou lacuna |
|---|---|
| **AFD — cabeçalho** | Dados potencialmente aproveitáveis: `Empresa.razao_social` e `cnpj`; **NÃO GUARDA identificado** um cabeçalho AFD gerado. ([colaboradores/models.py:40](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:40); [glossário:155](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/core/glossario.html:155)) |
| **AFD — marcações** | `Batida.colaborador`, `timestamp`, `tipo`, `sequencia`, `terminal`, `tipo_registro`, `origem`, `canal`, `criado_em`, além de timestamp do dispositivo e sincronização. `Colaborador` guarda CPF, nome e matrícula. ([ponto/models.py:22](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:22); [ponto/models.py:45](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:45); [ponto/models.py:75](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:75); [colaboradores/models.py:302](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:302)) |
| **AFD — NSR** | **NÃO GUARDA campo identificado como NSR em `Batida`**. Existe `sequencia`, mas o modelo não a identifica como NSR; não há base para equipará-los. ([ponto/models.py:8](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:8); [ponto/models.py:22](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:22)) |
| **AFD — identificação do REP/PTRP** | `Batida.terminal` e `tipo_registro` são texto; **NÃO GUARDA identificador explicitamente denominado REP ou PTRP em `Batida`**. A equivalência desses campos ao identificador fiscal permanece [não verificada]. ([ponto/models.py:22](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:22)) |
| **AFD — hash/assinatura** | **NÃO GUARDA hash ou assinatura fiscal explicitamente identificados em `Batida`**. `PingGeo.assinatura` existe para um ping geográfico; sua equivalência a assinatura de AFD ou de marcação fiscal está [não verificada]. ([ponto/models.py:8](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:8); [ponto/models.py:1005](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:1005)) |
| **AEJ — cabeçalho e identificação do trabalhador** | `Empresa.razao_social/cnpj`; `Colaborador.nome`, `cpf`, `pis`, matrículas, `cod_esocial`, `empresa`, `data_admissao` e `data_demissao`. **NÃO GUARDA identificado** cabeçalho AEJ gerado. ([colaboradores/models.py:40](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:40); [colaboradores/models.py:302](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:302); [colaboradores/models.py:335](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:335); [BACKLOG.md:2342](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2342)) |
| **AEJ — marcações** | A mesma `Batida` guarda instante, trabalhador, tipo, origem e campos de aprovação; não há registro AEJ gerado a comparar com esses dados. ([ponto/models.py:22](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:22); [ponto/models.py:62](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:62); [BACKLOG.md:2342](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2342)) |
| **AEJ — ajustes e inclusões** | `Batida.origem` admite `manual`, e guarda `justificativa`, `aprovada_por` e `aprovada_em`; `Justificativa` relaciona o pedido à `batida_gerada`. No caminho de veredito do admin, o serviço chama `registrar_batida(..., 'manual', autoridade_humana=True, aprovada_por=autor)`. ([ponto/models.py:13](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:13); [ponto/models.py:56](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:56); [ponto/models.py:167](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:167); [veredito_celula.py:321](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/veredito_celula.py:321)) |
| **AEJ — desconsiderações / retratação** | `Batida.retratada_em` guarda o instante enquanto retratada; a porta também marca `aprovada=False`. Na retratação humana, motivo e observação vão para `LogAuditoria.valor_depois`, junto à identificação da ação e do objeto; desfazer limpa `retratada_em`. ([ponto/models.py:62](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:62); [registro_batida.py:287](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/registro_batida.py:287); [retratar_humano.py:177](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/retratar_humano.py:177); [core/models.py:295](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/core/models.py:295); [retratar_humano.py:207](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/retratar_humano.py:207)) |
| **AEJ — vínculos** | `HistoricoVinculo` guarda histórico e vigências; `EscalaColaborador` associa colaborador, posto e escala, com início, fim e estado ativo. A correspondência desses vínculos com um registro fiscal específico está [nv]. ([colaboradores/models.py:548](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:548); [escala/models.py:871](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:871)) |
| **AEJ — horários contratuais** | `TipoEscala` guarda código, minutos de jornada, início, fim e até dois pares de intervalos; há também agenda e blocos de horário, além do vínculo de escala com vigência. ([escala/models.py:46](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:46); [escala/models.py:1399](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1399); [escala/models.py:1478](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1478); [escala/models.py:871](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:871)) |
| **AEJ — ausências e banco** | `Ausencia` guarda colaborador, tipo, datas, minutos, horas parciais, descrição e documento; `FechamentoMensal` guarda horas de falta, minutos abonados e `saldo_banco_horas`. A suficiência desses campos para o registro fiscal está [nv]. ([ponto/models.py:743](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:743); [ponto/models.py:425](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:425); [ponto/models.py:436](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:436); [ponto/models.py:468](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:468)) |
| **AEJ — assinatura** | **NÃO GUARDA assinatura de AEJ gerado identificada na leitura**. O campo `DecisaoProposta.assinatura` identifica o conteúdo de uma proposta de escala, não é apresentado pelo modelo como assinatura de arquivo fiscal. ([BACKLOG.md:2342](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2342); [escala/models.py:1891](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1891)) |

**Derivação:** como não foi identificado gerador de AFD ou AEJ, não há derivação *no momento da geração desses arquivos* demonstrável. Fora desse contexto, o modelo de escala descreve intervalo por duração deduzido das batidas, e `FechamentoMensal` guarda resultados com carimbo de derivação; isso não prova como um futuro arquivo fiscal deveria ser montado. ([RELATO.md:3591](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3591); [escala/models.py:94](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:94); [ponto/models.py:454](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:454))

## 18/11 15:19 · sessao a29096498b81 · FILA-CODEX-1811 · leitura · origem=Code

**Limite da conferência:** a cópia recebida não tem `.git`; portanto, não consegui confirmar que seu conteúdo corresponde ao hash informado. [não verificado] Os anexos `logs/...` foram fornecidos no enunciado, mas não estão disponíveis como arquivos nesta cópia; uso seus resultados como relato histórico, e não como medição atual. [não verificado]

## 1. O275 — forma A × forma B

A escolha continua aberta: a **forma A** alcança todos os ciclos exceto intermitente; a **forma B** acrescenta ao leitor a guarda dos ciclos `6x1`, `5x2` e `semanal`. O registro diz que a A estava construída na raia, enquanto a B exigiria código e testes diferentes; não há escolha registrada nessa passagem. [RELATO-ARQUIVO.md:746](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:746), [RELATO-ARQUIVO.md:750](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:750), [RELATO-ARQUIVO.md:753](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:753).

O cadastro novo é previsto sem default ligado; o feriado passaria a ser dia previsto pelo leitor de `EscalaColaborador`, sem um segundo gerador de previsto. O item permanece interrompido na escolha A/B. [BACKLOG.md:1679](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:1679).

**Os quatro tipos SDF:** o censo fornecido identifica **quatro tipos `12x36` com `dias_trabalho_semana='5,6'`**, mas não traz os quatro IDs ou nomes individualmente; não é possível atribuir um resultado individual a cada tipo. O censo também relata seis vínculos desses tipos, dois vigentes. [não verificado — identificação individual no dado fornecido]; [RELATO-ARQUIVO.md:750](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:750).

| Tipo ou universo | Forma A | Forma B | Medida e limite |
|---|---|---|---|
| Cada um dos quatro tipos `12x36` com `5,6` | Alcançável pelo flag, se ligado; pode converter feriado de folga de fase em previsto. | Fora da guarda de ciclos; o flag não o alcança enquanto permanecer `12x36`. | O censo fornecido mede dois vínculos vigentes nesses quatro tipos, mas não apresenta DIFF A/B por tipo. **[nv]** Sonda: relacionar os quatro IDs aos vínculos e comparar, por data e vínculo, célula, previsto e horas nas duas árvores, com o flag ligado. [RELATO-ARQUIVO.md:750](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:750) |
| `6x1` com texto SDF | Alcançável, se ligado. | Também alcançável, se ligado. | O censo fornecido relata um vínculo vigente, com foto de folgas; não mede o DIFF A/B dele. **[nv]** Sonda: comparar o mesmo vínculo e feriados nas duas árvores, incluindo a foto e as células. [RELATO-ARQUIVO.md:752](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:752) |
| Intermitentes com texto SDF | Excluídos. | Excluídos. | O censo fornecido relata seis tipos e quatro vínculos vigentes nessa categoria; não mede um DIFF por pessoa. **[nv]** Sonda: comparar células e horas dos vínculos intermitentes nas duas árvores. [BACKLOG.md:1679](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:1679) |
| `semanal` com sábado/domingo | Alcançável, se ligado. | Alcançável, se ligado. | O censo histórico relata zero tipos `semanal` entre 361; é o cadastro usado no RED descrito, não um alvo medido da frota. [RELATO-ARQUIVO.md:752](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:752) |

O **tipo de ciclo** é decidido no cadastro `TipoEscala.tipo_ciclo`; o método do tipo devolve `None` para `12x36` e `24x48`, e só o ramo `semanal` lê `dias_trabalho_semana`. No vínculo, a foto do mês ou a âncora determinam a fase; na cópia atual, o caminho com âncora retorna diretamente antes do bloco final de feriado. Essa diferença deve entrar no rebase da raia: não basta presumir que inserir um bloco no final alcançará os plantões com âncora. [models.py:816](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:816), [models.py:1271](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1271), [models.py:1289](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1289), [models.py:1363](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1363).

O censo fornecido descreve, nos três vínculos não intermitentes observados, quartas alternando entre trabalho e folga; isso mede a grade histórica, não o efeito de ligar o flag. O relato registra um exemplo calculado com a função real: feriado previsto na folga de fase reduz o intervalo entre plantões de **36,0 h para 12,0 h**; esse exemplo não é um DIFF medido para cada um dos quatro tipos. [RELATO-ARQUIVO.md:748](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO-ARQUIVO.md:748); [não verificado — medições individuais além do censo fornecido].

O censo histórico dizia que a precedência lia diretamente o flag do vínculo. **Na cópia atual isso mudou:** a precedência lê a célula quando existe e consulta `feriado_vira_folga`, que usa o lugar do vínculo do dia. Assim, aquela conclusão específica do censo precisa ser reavaliada sobre esta cópia. [precedencia.py:357](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/precedencia.py:357), [precedencia.py:369](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/precedencia.py:369), [models.py:1373](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1373).

## 2. SUITE-POR-IMPACTO — PRONTO-5 [nao verificado]

O pendente registrado é **“! aval PRONTO-5”**, de Ronald; a obra está interrompida por PRONTO-5, com “mapa/replay do PRONTO 5 + rebase” ainda indicados. O relato da mesa diz que esse `!` **solta OTIMIZA-1**, cujo próprio item espera o pouso da SUITE-POR-IMPACTO. O texto literal completo da pergunta do pendente não foi localizado na cópia: **[nv]**. [BACKLOG.md:1156](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:1156), [RELATO.md:3622](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3622), [BACKLOG.md:2302](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2302).

O contrato do PRONTO-5 é capturar os **cinco FAIL da O261(C)**, com mapa gerado no hash pai da regressão. O relato registra que a geração no pai teve sete falhas em dois módulos de smoke, não promoveu o mapa e que o replay exige mapa completo; a causa dos sete não estava nomeada. [PROMPTS.md:1682](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/PROMPTS.md:1682), [PROMPTS.md:1683](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/PROMPTS.md:1683), [RELATO.md:3047](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3047), [RELATO.md:3049](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3049).

Para fechar o que está registrado, faltam:

1. **Aval do PRONTO-5 e definição da base do replay** diante do mapa do pai com sete falhas; o relato registra como alternativas uma base atual com escape reproduzível ou mapa regenerado, sem registrar a escolha. [RELATO.md:3047](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3047).
2. **Mapa completo e replay que demonstre os cinco FAIL**; o replay pretendido era `270f75b7..7a8e4604`, selecionando os três módulos associados aos cinco FAIL. O resultado desse replay é **[nv]**. [RELATO.md:3061](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3061).
3. **Rebase da implementação e certificação para pouso:** a base registrada é `wt-si-2235 @ 2d91d5905`; o relato anterior localiza construção também em `wt-suite-impacto-1659 @ 6ece04a7`. Essas raias e refs não podem ser conferidas nesta cópia sem metadados Git: **[nv]**. [BACKLOG.md:1156](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:1156), [RELATO.md:3777](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3777).
4. **Para o pouso da obra**, além do PRONTO-5: suíte inteira da raia, verificador e pouso próprio; o relato registra PRONTO-6 construído na raia, não sua certificação final. [RELATO.md:3057](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:3057).

## 3. QUARENTENA-DO-COLAB

A **porta de quarentena por período não está implementada nesta cópia**: o BACKLOG a registra interrompida após reprovação e manda reaplicar a SPEC em cópia nova; os arquivos da implementação antiga citados no verificador não aparecem na árvore consultada. Assim, não é possível dizer que os defeitos daquela implementação foram curados no HEAD: a maior parte é **não aplicável ao código ausente**, mantendo-se pendente para a reconstrução. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887), [BACKLOG.md:2723](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2723).

| Achado do verificador fornecido | Conferência na cópia |
|---|---|
| 1. Desligar isenção **levantava** toda a janela, em vez de fechá-la preservando o passado. | A função de janela citada não está presente; **não procede como código atual**, e a regra de fechamento ainda não está demonstrada nesta cópia. O BACKLOG mantém “fechar e não levantar” entre as curas. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887) |
| 2. Vigia contava itens legítimos anteriores à janela e não chegava a zero. | A implementação de vigia por janela não está presente; o código atual é **vassoura que escreve silenciamento**, não o vigia da cópia reprovada. A incidência atual dos exemplos de produção é **[nv]**. [silencio_isentos.py:20](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/silencio_isentos.py:20), [silencio_isentos.py:34](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/silencio_isentos.py:34) |
| 3. Admin podia levantar janela causada por isenção com a flag ainda ligada. | A janela e sua porta não existem aqui; **não aplicável à implementação atual**, sem prova de cura da futura porta. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887) |
| 4. Migration sem reversão efetiva e com import de serviço vivo. | A migration específica da cópia reprovada não está presente; **não aplicável aqui**, sem prova de migration substituta reversível ou congelada. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887) |
| 5. Linha de checklist dos dois commits incorreta ou ausente. | Os commits e suas mensagens não são verificáveis sem `.git`: **[nv]**. | [nao verificado]
| 6. Reordenação de `retencao` sem RED para dia ausente e chamado já no app. | A reordenação por quarentena não está presente no `retencao` atual; a cobertura do RED antigo e de eventual reconstrução é **[nv]**. [pergunta_no_app.py:136](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/pergunta_no_app.py:136) |
| 7. Sonda DIFF duplicava a consulta da migration. | Sonda e migration antigas não estão disponíveis como arquivos na cópia; **[nv]** quanto a uma sonda substituta. O BACKLOG conserva a cura “DIFF pela função da migration”. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887) |
| 8. Semeadura usava a primeira ligação da flag, cobrindo indevidamente intervalo entre desliga e religa. | A função antiga não está presente; **não aplicável ao código atual**, sem prova de tratamento da última ligação na futura implementação. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887) |

O item 9 do verificador era explicitamente uma **não verificação** de escritas por cron em produção após deploy; isso continua **[nv]** no material recebido. [não verificado] [nao verificado]

### Matriz da porta existente no HEAD

Esta é a matriz do **véu por chamado** existente, não da quarentena por período prevista no aval. [BACKLOG.md:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:887), [arquivo.py:141](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/arquivo.py:141).

| Estado → ato → estado | Quem pode / condição | Gravação e trilha |
|---|---|---|
| Sem véu ou com véu → **silenciar/renovar** → véu até prazo | Pela view: usuário autenticado com `gerir_chamados`, POST e permissão de área; a porta exige autor com `pk`. | Grava prazo, autor, motivo e atualização; registra evento com antes/depois e ator. Prazo, motivo e autor idênticos com prazo explícito são no-op. [views_arquivar.py:23](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views_arquivar.py:23), [arquivo.py:141](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/arquivo.py:141) |
| Chamado vivo de isento sem véu → **vassoura/evento** → véu | Serviço seleciona pela flag atual `isento_ponto` e usa usuário de sistema; não filtra por início de janela. | Chama a mesma porta por chamado, com motivo e trilha próprios. [silencio_isentos.py:20](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/silencio_isentos.py:20), [silencio_isentos.py:34](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/silencio_isentos.py:34) |
| Véu → **desarquivar** → véu limpo, resgate carimbado | Usuário autenticado com `gerir_chamados`, POST e permissão de área; para encerramento por competência trancada, a reabertura pode recusar antes de levantar o véu. | Limpa prazo, autor e motivo, carimba primeiro resgate e registra antes/depois; a view também anexa ocorrência. [views_arquivar.py:51](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views_arquivar.py:51), [arquivo.py:177](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/arquivo.py:177) |
| Véu → **resposta do colaborador** → véu limpo, resgate carimbado | O serviço recebe o usuário que respondeu e encontra o chamado associado à pergunta. | Chama `levantar` com ator, motivo e ID da pergunta na trilha. [materializacao.py:1192](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/materializacao.py:1192) |
| Véu vigente → **passagem do prazo** → deixa de ser excluído pelo juiz da fila | Relógio de leitura; nenhum ato de escrita é demonstrado por essa transição. | `qs_na_fila` exclui apenas `silenciado_ate` futuro; outros filtros e leitores ainda podem afetar a exibição. [painel.py:159](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/painel.py:159), [painel.py:190](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/painel.py:190) |

## 18/11 15:19 · sessao 38b928ec5b32 · FILA-CODEX-1811 · leitura · origem=Code

**A classificação integral ficou [não verificado].** Não concluí a contagem A/B/C das 1.588 ocorrências informadas na tabela nem a lista nominal completa de B; portanto, a soma e a diferença para 1.588 também ficam [não verificado]. A identificação da cópia como o HEAD informado está [não verificado]: `git rev-parse HEAD` não encontrou um repositório Git.

Na amostra conferida na cópia, o resultado é:

| Campo | A | B | C | Base |
|---|---:|---:|---:|---|
| `data_conferida_em` | 3 | 2 | 0 | [materializacao.py:169](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/materializacao.py:169), [materializacao.py:1888](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/materializacao.py:1888), [validacao.py:994](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:994), [modal_fio.html:170](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/chamados/partials/modal_fio.html:170), [modal_fio.html:171](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/chamados/partials/modal_fio.html:171) |
| `data_conferida_por` | 0 | 1 | 0 | [modal_fio.html:171](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/chamados/partials/modal_fio.html:171) |
| `materializada_por` | 0 | 1 | 0 | [procedencia_batida.py:72](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/procedencia_batida.py:72) |

Lista **parcial** de B, agrupada por campo:

- `data_conferida_em`: [modal_fio.html:170](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/chamados/partials/modal_fio.html:170) e [linha 171](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/chamados/partials/modal_fio.html:171), `<template>`.
- `data_conferida_por`: [modal_fio.html:171](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/chamados/partials/modal_fio.html:171), `<template>`.
- `materializada_por`: [procedencia_batida.py:72](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/procedencia_batida.py:72), `procedencia`.

As demais ocorrências e a eventual presença delas em B permanecem **[não verificado]**.

## 18/11 15:18 · sessao 40f541e8bed6 · - · - · origem=Ronald (tmux)

- O repositório registra que não há gerador de AEJ em [app/docs/BACKLOG.md:2342](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2342). A rota `importar-aej/` chama `importar_aej`, que responde HTTP 410; o importador antigo está desativado em [app/ponto/urls.py:9](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/urls.py:9), [app/ponto/views.py:54](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:54) e [app/ponto/legacy/README.md:8](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/legacy/README.md:8).
- `ImportacaoAEJ` guarda arquivo importado, status, totais, log e autor; isso não é geração em [app/ponto/models.py:200](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:200).
- Como não há gerador, não há derivação **durante a geração de AEJ** a inventariar em [app/docs/BACKLOG.md:2342](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2342). O modelo informa que a agenda de escalas fixas é calculada sob demanda, fora de geração AEJ, em [app/escala/models.py:1399](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1399).
- Dados disponíveis incluem empresa e CNPJ, identificação do colaborador, vínculo e vigência, horários e intervalos de escala, agenda flexível, ausências e saldo mensal de banco de horas em [app/colaboradores/models.py:40](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:40), [app/colaboradores/models.py:302](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:302), [app/escala/models.py:871](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:871), [app/escala/models.py:46](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:46), [app/escala/models.py:1478](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1478), [app/ponto/models.py:660](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:660) e [app/ponto/models.py:468](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:468).
- O diretório disponibilizado não possui metadados Git; a correspondência do snapshot com o hash informado está **[não verificada]**.

## 18/11 15:18 · sessao f3ea8697e4a1 · - · - · origem=Ronald (tmux)

Achados enviados ao agente principal, com referências para geração, parser legado e modelos de identificação, vínculo, jornada, ausência e banco de horas. [app/docs/RELATO.md:3591](app/docs/RELATO.md#L3591)

## 18/11 15:18 · sessao 3e25729c3a0d · - · - · origem=Ronald (tmux)

O glossário informa que a exportação AFD ainda está pendente; não identifiquei gerador, view, comando ou chamador de geração AFD na busca realizada. ([app/templates/core/glossario.html:150](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/core/glossario.html:150), [app/templates/core/glossario.html:155](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/templates/core/glossario.html:155)) [não verificado]

| Dado potencial | O que o modelo guarda |
|---|---|
| Marcação | `Batida.colaborador`, `timestamp`, `tipo`, `sequencia`, `terminal` e `tipo_registro`. `sequencia` não está identificado como NSR. ([app/ponto/models.py:22](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:22)) |
| Inclusão humana | `origem` admite `manual`; uma materialização por veredito grava essa origem e `aprovada_por=autor`. ([app/ponto/models.py:13](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:13), [app/ponto/services/veredito_celula.py:321](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/veredito_celula.py:321)) |
| Retração | `Batida.retratada_em` guarda o instante; a porta grava também `aprovada=False`. ([app/ponto/models.py:62](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:62), [app/ponto/registro_batida.py:289](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/registro_batida.py:289)) |
| NSR, hash e assinatura fiscais | Não há campo explicitamente identificado como tal em `Batida`; não há evidência para equiparar `sequencia` a NSR. ([app/ponto/models.py:8](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/models.py:8)) |
| Identificação parcial | `Empresa.razao_social` e `cnpj`; `Colaborador.matricula_hasner`, `matricula_nexti`, `cpf` e `pis`. ([app/colaboradores/models.py:40](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:40), [app/colaboradores/models.py:302](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:302), [app/colaboradores/models.py:335](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:335)) |

O leiaute específico por tipo de registro do AFD não foi comprovado nas referências encontradas; derivação durante geração AFD também não foi demonstrada. [nv]

## 18/11 15:17 · sessao 6a66938c222e · FILA-CODEX-1811 · leitura · origem=Code

A leitura **não fecha a atribuição das espúrias aos 11 sítios nem produz contagens**: o texto fornecido contém o pré-mapa, mas não as linhas da autópsia com as órfãs medidas e seus identificadores; o formato e o conteúdo desses anexos estão [não verificado]. A cópia também não tem metadados Git para conferir o hash informado. O código abaixo é uma **sonda parcial de correlação**, não uma sonda que prove quantas espúrias cada sítio explica. [não verificado; app/docs/BACKLOG.md:2091]

O registro distingue o juiz do dia da batida, `turnos_do_colab`, do juiz da posição da batida, `_match_marcos`, e do juiz de períodos, `periodos_do_dia`. Portanto, apontar o mesmo juiz como substituto obrigatório de todos os sítios exigiria provar a pergunta de cada um. [app/core/juizes.py:216; app/core/juizes.py:221; app/core/juizes.py:1207]

**Dono ESTRUTURA — achados de código candidatos, ainda sem medida de produção:** a rota alternativa de `views_core` deriva dia civil para gravar contexto; os dois sítios de `folha/porta_export.py` derivam agrupamento por dia civil e seus resultados podem chegar a uma métrica gravada. A divergência concreta em cada órfã e sua atribuição causal permanecem [não verificado]. [app/api/urls_core.py:14; app/api/views_core.py:713; app/folha/porta_export.py:104; app/folha/porta_export.py:538; app/folha/porta_export.py:779]

### Os 20 registros marcados no pré-mapa

Os caminhos abaixo demonstram caminhos possíveis sob as guardas indicadas; execução efetiva em produção é [não verificado]. “Grava” descreve o efeito do caminho, não necessariamente uma escrita da função auxiliar. O registro de `calcular_mes:2408` também aparece como vivo em outra linha do próprio dado fornecido. [app/docs/BACKLOG.md:2091; não verificado]

1. **`escala/utils.py::_alinhar_por_papel:529`.** Alinha papéis de batidas a marcos; é parte de `_match_marcos`, o próprio juiz registrado para a posição do marco. Caminho: `_match_marcos` → montagem da grade por turno → `reconciliar_grade` command; a grade alimenta o plano de reconciliação e pode alcançar escrita, conforme modo e guardas. A classificação desta implementação interna como juiz externo paralelo é [não verificado]. [app/escala/utils.py:511; app/escala/utils.py:623; app/escala/utils.py:1712; app/escala/utils.py:2098; app/ponto/management/commands/reconciliar_grade.py:118; app/core/juizes.py:221]

2. **`escala/utils.py::_papeis_por_alternancia:496`.** Infere papéis por alternância de uma sequência de pelo menos quatro batidas; `_match_marcos` a chama quando não recebe turno e `derivar` está ativo. O juiz de posição é `_match_marcos`, que contém essa chamada. **Zero chamador provado até ponto de entrada para a linha 496**; gravação ou exibição pelo ramo específico [não verificado]. Um caminho com apenas duas batidas não alcança o laço por causa da guarda. [app/escala/utils.py:489; app/escala/utils.py:496; app/escala/utils.py:606; app/core/juizes.py:221]

3. **`ponto/calculador/nucleo.py::minutos_do_oraculo:246`.** Sem pares fornecidos, pareia por sequência. Caminho: command `e6_oraculo` → `InsumosDoDono` → `minutos_do_oraculo`; o comando exibe e, com `--csv`, escreve arquivo. O juiz registrado para períodos é `periodos_do_dia`; a troca direta desse ramo por ele é [não verificado]. Os chamadores que passam `pares` não provam a execução deste ramo. [app/ponto/management/commands/e6_oraculo.py:284; app/ponto/management/commands/e6_oraculo.py:328; app/ponto/management/commands/e6_oraculo.py:153; app/ponto/calculador/nucleo.py:110; app/ponto/calculador/nucleo.py:246; app/ponto/management/commands/e6_oraculo.py:442; app/core/juizes.py:1208]

4. **`ponto/turnos.py::_pares_marcados:812`.** Lê marcas e compõe pares dentro da lógica do juiz de intervalo. Caminho: `_pares_marcados` → `realizado_dos_turnos` → `realizado_do_dia` → `turnos_longos_suspeitos` → command `termometro_regua`; com `--lavrar`, o resultado pode integrar lista gravada. `intervalos_do_turno` é o juiz registrado para intervalo; a necessidade de substituir esta implementação interna é [não verificado]. [app/ponto/turnos.py:545; app/ponto/turnos.py:756; app/ponto/turnos.py:795; app/escala/regua_defesa.py:363; app/escala/management/commands/termometro_regua.py:75; app/escala/management/commands/termometro_regua.py:92; app/core/juizes.py:223]

5. **`ponto/motor_calculo_v2.py::MotorBase.calcular_mes:2408`.** A herança por gap participa do cálculo mensal; o registro a associa às fatias de par e dia, sem provar que cada execução produza divergência. Caminho: view de fechamento → `recalcular_fechamento_mes` → motor `calcular_mes`; **PAGA/GRAVA** pelo fechamento. Para dia de batida o registro aponta `turnos_do_colab`; a substituição exata da herança [não verificado]. [app/ponto/views.py:1255; app/ponto/services/fechamento.py:286; app/ponto/motor_calculo_v2.py:2408; app/core/juizes.py:217; app/docs/BACKLOG.md:2051]

6. **`api/views_core.py::api_bater_ponto:713`.** Usa a data civil local da batida no contexto de chamado. Existe caminho por `config.urls_core` → `api.urls_core` → view, que pode gravar chamado; a seleção desse URLconf em produção é [não verificado]. O juiz registrado para pertença da batida ao turno é `turnos_do_colab`. **GRAVA**, sob as guardas da view. [app/config/urls_core.py:6; app/api/urls_core.py:14; app/api/views_core.py:713; app/api/views_core.py:723; app/config/settings/saas.py:76; app/core/juizes.py:217]

7. **`chamados/juizes.py::_dia_da_batida:54`.** Usa `localdate` da batida. É chamado de `dia_do_fato`, dentro da própria autoridade de dia do chamado; existe caminho pela fábrica de disputa chamada pelo command `processar_alertas_turno`, com possível gravação de pergunta. O juiz de pertença da batida ao turno é `turnos_do_colab`; equivalência e substituição direta neste degrau [não verificado]. [app/chamados/juizes.py:51; app/chamados/juizes.py:128; app/chamados/services/disputa_emissao.py:795; app/ponto/management/commands/processar_alertas_turno.py:382; app/core/juizes.py:217]

8. **`chamados/juizes.py::_dia_do_contexto_fraco:170`.** Extrai data de timestamp do contexto como fonte fraca; é chamado por `dia_do_fato` e `dias_do_fato`. Caminho demonstrado: command `arquivar_competencia_encerrada` → `dias_do_fato` → auxiliar; com `--apply`, pode silenciar chamados selecionados. Para dia do chamado, o registro aponta `data_do_chamado`, e o auxiliar está na implementação equivalente de `dia_do_fato`; troca específica [não verificado]. [app/chamados/juizes.py:139; app/chamados/juizes.py:162; app/chamados/juizes.py:230; app/chamados/management/commands/arquivar_competencia_encerrada.py:93; app/chamados/management/commands/arquivar_competencia_encerrada.py:102; app/chamados/management/commands/arquivar_competencia_encerrada.py:133; app/core/juizes.py:290]

9. **`chamados/juizes.py::_plantio_na_hora_do_slot:275`.** Usa a data do evento ou a data local do instante para procurar plantio no slot, inclusive na janela do dia seguinte. Caminho: command `backfill_veredito_lavrado` → verificação de materialização → auxiliar; com `--apply`, pode gravar veredito. É termo interno da avaliação da pergunta; o registro nomeia juiz para pergunta viva, mas a substituição desse teste pelo juiz de dia da batida é [não verificado]. [app/chamados/juizes.py:254; app/chamados/juizes.py:275; app/chamados/juizes.py:764; app/chamados/management/commands/backfill_veredito_lavrado.py:82; app/chamados/management/commands/backfill_veredito_lavrado.py:105; app/chamados/management/commands/backfill_veredito_lavrado.py:138; app/core/juizes.py:291]

10. **`chamados/juizes.py::dias_do_fato:204`.** Resolve dias de fatos em lote, incluindo data local da batida; é a forma em lote do julgamento do dia do chamado. Caminho e efeito: command `arquivar_competencia_encerrada` → `dias_do_fato` → seleção de alvos → **GRAVA** silenciamento com `--apply`. Juiz de dia do chamado e eventual aplicação do juiz de turno ao ramo são os mesmos limites do item 7. [app/chamados/juizes.py:175; app/chamados/juizes.py:204; app/chamados/management/commands/arquivar_competencia_encerrada.py:102; app/chamados/management/commands/arquivar_competencia_encerrada.py:127; app/chamados/management/commands/arquivar_competencia_encerrada.py:141; app/core/juizes.py:217; app/core/juizes.py:290]

11. **`chamados/services/materializacao.py::_plantio_preenche_furo_na_data:434,455`.** Seleciona dia da célula/pergunta ou evento e usa instante local no cálculo da posição; não é simplesmente data civil em todos os ramos. Caminho demonstrado: rota de chamados → view → ações de disputa → validação → materialização → guarda; pode influenciar **GRAVAÇÃO** de plantio. O código já consulta `dia_sem_trabalho`; aplicação adicional de `turnos_do_colab` a estes ramos [não verificado]. [app/chamados/urls.py:14; app/chamados/views.py:483; app/chamados/services/acoes_disputa.py:13; app/chamados/services/validacao.py:786; app/chamados/services/materializacao.py:2125; app/chamados/services/materializacao.py:1926; app/chamados/services/materializacao.py:1391; app/chamados/services/materializacao.py:418; app/chamados/services/materializacao.py:434; app/chamados/services/materializacao.py:455; app/core/juizes.py:217; app/core/juizes.py:303]

12. **`escala/detector_proposta.py::medir_padrao:58`.** Agrupa batidas por data local para medir padrão. Caminho: command `propor_escala_limbo` → `propor_escala` → `medir_padrao`; nesse caminho **EXIBE** proposta. O juiz registrado para dia de turno é `turnos_do_colab`; sua aplicabilidade a uma proposta de escala em limbo e efeitos de outros chamadores [não verificado]. [app/escala/management/commands/propor_escala_limbo.py:22; app/escala/management/commands/propor_escala_limbo.py:37; app/escala/management/commands/propor_escala_limbo.py:41; app/escala/detector_proposta.py:58; app/escala/detector_proposta.py:120; app/core/juizes.py:217]

13. **`folha/porta_export.py::dias_fora_do_dia_pago:104`.** Agrupa minutos pela data local da entrada e os compara com `DiaPago`. Caminho: command `selo_leitores_no_mesmo_numero` → `medir` → auxiliar → conclusão/lavra da métrica; **GRAVA** métrica quando a lavra ocorre. O juiz registrado para pertença da batida é `turnos_do_colab`; como transportar sua resposta para cada período do resultado está [não verificado]. [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:219; app/folha/porta_export.py:104; app/folha/porta_export.py:597; app/folha/porta_export.py:779; app/folha/porta_export.py:1255; app/core/juizes.py:217]

14. **`folha/porta_export.py::medir:538`.** Usa a data local da entrada para agrupar trabalhado, HE e intervalo na verificação de rubricas. O mesmo command do item anterior chama `medir`, cujo resultado pode ser lavrado em `MetricaSnapshot`: **GRAVA**, condicionadamente. Juiz de pertença da batida e limite de aplicabilidade: item 13. [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:219; app/folha/porta_export.py:538; app/folha/porta_export.py:560; app/folha/porta_export.py:779; app/folha/porta_export.py:1255; app/core/juizes.py:217]

15. **`ponto/calculador/nucleo.py::minutos_do_oraculo:117,141,235`.** Conta ou atribui dias aos períodos; em `:141`, há fallback para data da entrada quando falta o terceiro elemento do par. Caminho: view de fechamento → fechamento → alimentação do calculador → núcleo; a lavra pode gravar `DiaPago` de versão oráculo sob as guardas de competência. O juiz registrado para períodos é `periodos_do_dia`, e para dia da batida é `turnos_do_colab`; execução concreta do fallback e substituição exata [não verificado]. [app/ponto/views.py:1255; app/ponto/services/fechamento.py:1265; app/ponto/services/fechamento.py:1271; app/ponto/services/fechamento.py:1277; app/ponto/calculador/alimentacao.py:449; app/ponto/calculador/nucleo.py:117; app/ponto/calculador/nucleo.py:141; app/ponto/calculador/nucleo.py:235; app/core/juizes.py:217; app/core/juizes.py:1208]

16. **`MotorBase._intervalo_descontado_no_dia:1313`.** Usa data como chave para limite de intervalo; consulta também o juiz de intervalo. Caminho: view de fechamento → fechamento → `calcular_mes` → `Motor12x36.calcular_periodo` → `_limite_com_suprimido` → auxiliar; **PAGA/GRAVA** pelo fechamento. O juiz do intervalo já é consultado; qual autoridade deve fornecer a chave de dia nesse ramo [não verificado]. [app/ponto/views.py:1255; app/ponto/services/fechamento.py:286; app/ponto/motor_calculo_v2.py:2458; app/ponto/motor_calculo_v2.py:2840; app/ponto/motor_calculo_v2.py:1803; app/ponto/motor_calculo_v2.py:1313; app/ponto/motor_calculo_v2.py:1321; app/core/juizes.py:223]

17. **`MotorBase._janela_do_dia:1528`.** Usa dia civil da entrada para montar janela e decisões associadas. Caminho: view → fechamento → `calcular_mes` → `Motor12x36.calcular_periodo` → `_janela_do_par` → auxiliar; **PAGA/GRAVA** pelo fechamento. O registro de dia da batida aponta `turnos_do_colab`; correspondência precisa da chave usada aqui [não verificado]. [app/ponto/views.py:1255; app/ponto/services/fechamento.py:286; app/ponto/motor_calculo_v2.py:2458; app/ponto/motor_calculo_v2.py:2759; app/ponto/motor_calculo_v2.py:1760; app/ponto/motor_calculo_v2.py:1528; app/core/juizes.py:217]

18. **`MotorBase._previsto_do_dia:1227`.** Usa data como chave do previsto; consulta a autoridade do previsto. Caminho: view → fechamento → `calcular_mes` → `Motor12x36.calcular_periodo` → `_jornada_do_dia` → auxiliar; **PAGA/GRAVA** pelo fechamento. O juiz da chave de dia aplicável a esse ramo [não verificado]. [app/ponto/views.py:1255; app/ponto/services/fechamento.py:286; app/ponto/motor_calculo_v2.py:2458; app/ponto/motor_calculo_v2.py:2835; app/ponto/motor_calculo_v2.py:1273; app/ponto/motor_calculo_v2.py:1227; app/ponto/motor_calculo_v2.py:1237]

19. **`ponto/motor_calculo_v2.py::_periodos_pelo_marco:578,686,687`.** Agrupa por dia e usa mapa do juiz com fallback civil para filtrar períodos. Caminho: view → fechamento → `calcular_mes` → `montar_periodos` → `_periodos_via_autoridade` → auxiliar; depende das guardas do ramo e pode participar do cálculo **PAGA/GRAVA**. O ramo consulta `periodos_do_dia`, juiz registrado para períodos; a correção específica do fallback de dia [não verificado]. [app/ponto/views.py:1255; app/ponto/services/fechamento.py:286; app/ponto/motor_calculo_v2.py:2354; app/ponto/motor_calculo_v2.py:2748; app/ponto/motor_calculo_v2.py:531; app/ponto/motor_calculo_v2.py:578; app/ponto/motor_calculo_v2.py:611; app/ponto/motor_calculo_v2.py:686; app/core/juizes.py:1208]

20. **`ponto/propositor.py::dia_posicional_completo:364`.** Seleciona marcas pela data local e verifica completude posicional. Caminho parcial: `processar_alertas_turno` → `turnos_abertos_de` do reconciliador → auxiliar; o resultado participa da decisão que pode levar à retratação. O elo do `handle` até esse ramo específico está [não verificado]; portanto, **zero chamador provado até ponto de entrada para esse ramo**. Os juízes registrados de dia e de marco são, respectivamente, `turnos_do_colab` e `_match_marcos`; equivalência com o predicado completo [não verificado]. [app/ponto/management/commands/processar_alertas_turno.py:204; app/ponto/management/commands/processar_alertas_turno.py:216; app/chamados/reconciliador.py:83; app/chamados/reconciliador.py:113; app/ponto/propositor.py:341; app/ponto/propositor.py:364; app/core/juizes.py:217; app/core/juizes.py:221]

### Texto de sonda parcial — não executar agora

**Entrada esperada:** lista transcrita da autópsia, uma linha por órfã, com identificador da medição, `colaborador_id` numérico, data ISO da medição e PK da batida. O formato real do anexo e a disponibilidade dessas quatro chaves estão [não verificado]. A sonda consulta apenas as batidas identificadas e `DiaPago` do mesmo colaborador e data; imprime `colN`, datas e PKs. Os campos consultados existem nos modelos. [app/ponto/models.py:22; app/ponto/models.py:23; app/ponto/models.py:315; app/ponto/models.py:319]

Ela **não satisfaz ainda** a parte que pede os 11 cruzamentos por escritor: não identifica qual sítio gravou uma linha, não compara a decisão histórica com o juiz, não classifica uma coincidência como causa e imprime as contagens por sítio como `[não verificado]`. A relação `Batida.pergunta_origem` é um vínculo possível, mas seu próprio modelo admite ausência em batidas de chão e em plantios antigos; ela não prova autoria para todas as gravações. [app/ponto/models.py:30; app/ponto/models.py:40]

```python
# Texto para Django shell, em uma sombra PostgreSQL. NÃO EXECUTADO.
# Preencher somente com linhas da autópsia conferida:
# (id_da_medicao, colaborador_id, "AAAA-MM-DD", batida_pk)
ORFAS = []

from datetime import date
from django.db import connection, transaction
from ponto.models import Batida, DiaPago

estado = "INCOMPLETA"
try:
    if not ORFAS:
        raise ValueError("Autópsia não fornecida: ORFAS está vazia")

    chaves = set()
    for medida, col_id, data_iso, batida_pk in ORFAS:
        if not isinstance(col_id, int) or not isinstance(batida_pk, int):
            raise ValueError("colaborador_id e batida_pk devem ser inteiros")
        data = date.fromisoformat(data_iso)
        chave = (medida, col_id, data, batida_pk)
        if chave in chaves:
            raise ValueError("Linha duplicada na entrada: %r" % (chave,))
        chaves.add(chave)

    if connection.vendor != "postgresql":
        raise RuntimeError("Sonda requer PostgreSQL em transação read-only")

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION READ ONLY")

        for medida, col_id, data, batida_pk in sorted(chaves):
            batida = (
                Batida.objects.filter(pk=batida_pk, colaborador_id=col_id)
                .values("pk", "timestamp", "tipo", "pergunta_origem_id")
                .first()
            )
            dias = list(
                DiaPago.objects.filter(colaborador_id=col_id, data=data)
                .values_list("pk", "data", "versao", "tipo")
                .order_by("pk")
            )
            print(
                "ORFA", medida, "col%s" % col_id, "data", data,
                "batida_pk", batida_pk, "batida", batida,
                "diapago_pks_datas_versoes_tipos", dias,
                "CAUSA_POR_SITIO=[não verificado]",
            )

        print("CONTAGEM_ESPURIAS_POR_SITIO=[não verificado]")
        estado = "CORRELACAO_PARCIAL"

except Exception as exc:
    print("ERRO_SONDA", type(exc).__name__, str(exc))
finally:
    print("FIM_SONDA", estado)
```

Para completar o item (ii), faltam as linhas da autópsia e uma prova, para cada um dos 11 sítios pretendidos, do vínculo entre a decisão daquele sítio e o registro histórico observado. Sem esses vínculos, contagens por data ou colaborador seriam apenas coincidências, e o conjunto exato dos 11 também fica [não verificado]. [app/ponto/models.py:40; não verificado]

## 18/11 15:17 · sessao 14cb2755f941 · FILA-CODEX-1811 · leitura · origem=Code

**O número real de B e a classificação dos 1.588 leitores ficam [não verificado].** A tabela fornecida declara 1.588 ocorrências `NAO_COBERTO`, mas a seção nominal do resumo está limitada a “só direto+template, produção”; ela não traz as ocorrências indiretas necessárias para classificar cada item. O próprio instrumento também associa derivados por nome de método e pode casar campos homônimos. [não verificado: anexos fornecidos no pedido, sem arquivo local numerado]

A cópia recebida não contém os anexos como arquivos locais e não possui `.git`; portanto, não consegui conferir o hash do HEAD nem citar linhas do resumo. [não verificado] Também não seria correto chamar toda linha de `models.py` de A: por exemplo, `PerguntaDisputa` consulta `DisputaSupervisao.fechada_em` em [models.py:2860](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2860). Do mesmo modo, duas ocorrências de `recusa_motivo` são leituras de `request.POST`, não do campo do modelo: [acoes_disputa.py:83](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/acoes_disputa.py:83) e [fio.py:565](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:565). Essas ocorrências exigem uma regra para falsos positivos, além dos três baldes solicitados. **[nv]**

Amostra conferida na cópia: a leitura do próprio `fechada_em` por `DisputaSupervisao.aberta` é A ([models.py:1961](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:1961)); a exibição de `status_local` pela API é B como leitura direta ([views_mensageria.py:1085](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/api/views_mensageria.py:1085)); o auditor de invariantes é C pela finalidade de auditoria ([auditar_invariantes_chamados.py:574](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/management/commands/auditar_invariantes_chamados.py:574)). A lista nominal completa de B por campo permanece **[não verificado]**.

**D3.** A função mora em [validacao.py:408](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:408). Na busca da cópia, o chamador de produção identificado é `Command.handle` do comando Django `estoque_l130_instante_sem_texto`, que chama a função no ramo `--apply` ([comando:25](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:25), [comando:51](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:51), [comando:105](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:105)). O catálogo o descreve como ato de uma vez, não rotina ([crons.py:791](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/config/crons.py:791)); execução efetiva ou outros chamadores dinâmicos em produção: **[não verificado]**.

O achado “sem ato” precisa de precisão: D3 preserva, se faltar, uma linha `RESPOSTA` da projeção antiga, limpa o par e o carimbo pela porta de lote e grava `LogAuditoria` com antes/depois; não grava uma `DecisaoPergunta` **compensatória da limpeza** ([validacao.py:434](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:434), [models.py:4260](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4260), [models.py:3485](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3485)). A falha do evento provoca exceção dentro da transação ([validacao.py:449](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:449)).

A L-130 manda devolver esse estoque a pendente **com trilha** e limpar a resposta inteira; D3 tem a trilha de auditoria descrita acima, de modo que não se comprova ausência total de trilha nem violação dessa cláusula só pela falta de ato compensatório ([LEIS.md:151](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:151), [validacao.py:449](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:449)). Há, porém, uma **lacuna na sequência de decisões**: seu contrato diz que cada escritor da projeção registra a decisão no mesmo ato, enquanto D3 só preserva a resposta anterior e documenta a ausência deliberada de compensação ([models.py:4134](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4134), [validacao.py:414](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:414)). A seção 0 item 7 exige escritor canônico e trilha para ação crítica; o uso da porta de lote e do evento, isoladamente, não prova infração dessa redação ([CLAUDE.md:32](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/CLAUDE.md:32)). Se o corte compensatório específico de ressuscitação alcança **também D3**, fica **[nv]**: o texto citado o formula para ressuscitar ([models.py:1671](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:1671)).

A sonda abaixo **não foi executada**. Conta eventos D3 ainda presentes na trilha, tanto ocorrências quanto perguntas distintas; não demonstra eventos removidos, anteriores a essa instrumentação ou ocorridos noutros schemas. A ação e o modelo vêm de [validacao.py:449](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:449); os campos consultados são de [core/models.py:285](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/core/models.py:285).

```bash
python app/manage.py shell <<'FIM_SONDA'
from django.db import connection, transaction
from core.models import LogAuditoria

with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco, schema = cursor.fetchone()

    eventos = LogAuditoria.objects.filter(
        acao="l130_instante_sem_texto_devolvida",
        modelo="PerguntaDisputa",
    )
    print("banco=", banco, "schema=", schema)
    print("eventos_d3_na_trilha=", eventos.count())
    print("perguntas_distintas=", (
        eventos.exclude(objeto_id__isnull=True)
        .order_by().values("objeto_id").distinct().count()
    ))
    print("eventos_sem_objeto_id=", eventos.filter(objeto_id__isnull=True).count())
FIM_SONDA
```

Para concluir o item (i), falta a listagem **integral** das 1.588 linhas `NAO_COBERTO`, incluindo indiretos e instrumentos, com linhas numeráveis; o total B e sua lista nominal seguem pendentes. **[não verificado]**

## 18/11 15:17 · sessao 44b4e0d39ff6 · FILA-CODEX-1811 · leitura · origem=Code

A cerca de competência **só julga escritas dentro dos quatro atos que a abrem**; fora desse escopo, a guarda retorna sem consultar entrega. Nas seis portas abaixo, a entrega, por si, não aciona essa cerca; cada porta ainda pode recusar por suas próprias regras. `PerguntaDisputa.save` também não chama a guarda na criação de uma instância. ([fio.py:1397](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1397), [models.py:3674](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3674))

**Limite da entrega:** a cópia não contém `.git`, de modo que o hash efetivo do HEAD está **[não verificado]**. A pasta dos anexos também não está na cópia; os resultados da sonda abaixo vêm exclusivamente do texto raspado fornecido e não foram reproduzidos. Os testes citados foram lidos, não executados. **[não verificado]** [nao verificado]

### Matriz das portas

A coluna “juiz” identifica quem decide a transição da porta; a guarda de entrega é a ressalva comum acima. Onde há mais de um ramo, os campos e retornos são condicionais.

| Porta | Estado de entrada lido | Juiz consultado | Operação | O que grava | Retorno ao usuário | Prova |
|---|---|---|---|---|---|---|
| **P8 — recusar pergunta** | Motivo, resposta, validação e estado fora do fato. [validacao.py:886](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:886) | `pergunta_respondida` e `fora_do_fato`; validação e motivo também são verificados. [validacao.py:898](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:898) | Recusa e reabre a resposta, na mesma transação. [validacao.py:917](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:917) | `PerguntaDisputa.recusa_motivo`; reabertura zera `respondida_em` e `resposta_colab`, com decisão e trilha de recusa. [validacao.py:922](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:922) | Sucesso: “Resposta recusada e devolvida ao colaborador.” Há mensagens para motivo ausente, pergunta pendente, validada ou fora do fato. [validacao.py:887](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:887), [validacao.py:953](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:953) | Testes da porta e da reabertura. [test_porta_validacao_pergunta.py:247](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_porta_validacao_pergunta.py:247) |
| **P9 — conferir data** | POST, permissão, justificativa, datas candidatas e conferência anterior. [views.py:1307](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views.py:1307), [validacao.py:983](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:983) | `data_do_fato_de` decide a divergência. [validacao.py:977](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:977) | Confere a data divergente e registra a justificativa. [validacao.py:990](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:990) | `PerguntaDisputa.data_conferida_em/por` e log com datas e justificativa. [validacao.py:996](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:996) | “Data conferida: vale … A pergunta liberou para materialização”; recusas têm mensagens próprias. [validacao.py:984](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:984), [validacao.py:1005](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:1005) | Testes da conferência. [test_porta_validacao_pergunta.py:308](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_porta_validacao_pergunta.py:308) |
| **P16 — responder pergunta** | Titular, pergunta viva, resposta, tipo e opções. [views.py:1227](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views.py:1227), [disputa_emissao.py:1446](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1446) | `pergunta_viva` na entrada e validação por tipo no core da resposta. [views.py:1237](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views.py:1237), [disputa_emissao.py:1449](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1449) | Registra resposta; conforme o escape, pode materializar ausência. [disputa_emissao.py:1517](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1517), [disputa_emissao.py:1560](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1560) | `PerguntaDisputa.resposta_colab/respondida_em`; pode limpar `validada_em/por` e carimbo, ou gravar `recusa_motivo`; registra `DecisaoPergunta` de resposta. [disputa_emissao.py:1480](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1480), [disputa_emissao.py:1527](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1527) | PWA: “Resposta registrada” ou erro; API retorna JSON de sucesso ou erro. [views.py:1284](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views.py:1284), [api/views.py:2610](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/api/views.py:2610) | Testes de resposta e de limpeza da validação anterior. [test_porta_resposta_pergunta.py:174](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_porta_resposta_pergunta.py:174), [test_porta_resposta_pergunta.py:225](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_porta_resposta_pergunta.py:225) |
| **X4 — aceitar folga** | Colaborador, dia da pergunta e existência de folga; o ramo é selecionado na validação. [validacao.py:499](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:499), [validacao.py:661](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:661) | `o_dia_ja_e_folga` e `checar_declaracao`; a declaração torna a verificar. [validacao.py:511](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:511), [validacao.py:536](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:536) | Carimba e fecha a pergunta; se necessário e aceito, declara folga e chama recálculo por evento. [validacao.py:545](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:545), [validacao.py:590](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:590) | `PerguntaDisputa.validada_em/por` e carimbo de fechamento; declaração de folga no ramo novo. Modelo e campos completos da declaração **[não verificado]**. [validacao.py:551](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:551) | Diz que a folga já existia, foi aceita ou foi recusada; sem dia, pede conferência. [validacao.py:501](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:501), [validacao.py:575](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:575) | Trilha de aceite e chamada de recálculo no código; teste específico **[não verificado]**. [validacao.py:561](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/validacao.py:561) |
| **A9b — admin encerra** | Motivo, usuário, fechamento anterior, respostas sem veredito e perguntas vivas. [models.py:2041](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2041), [models.py:2106](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2106) | `respostas_sem_veredito` e `pergunta_viva`; motivo obrigatório. [models.py:2048](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2048), [models.py:2057](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2057) | Fecha perguntas vivas sem resposta e a disputa, em transação; resposta sem veredito impede o fechamento. [models.py:2048](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2048), [models.py:2066](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2066) | Carimbo e via da `PerguntaDisputa`; `DecisaoPergunta` de silêncio; `DisputaSupervisao.fechada_em/por`, `resolucao` e `motivo_fechamento`. [models.py:2080](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2080), [models.py:2095](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2095) | Método devolve `True`, `False` idempotente ou `ValueError`; rota e tela de produção **[não verificado]**. [models.py:2041](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2041), [models.py:2104](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2104) | Eventos de pergunta encerrada e disputa fechada; teste específico **[não verificado]**. [models.py:2090](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2090) |
| **X11 — declarar hora** | Módulo do chamado, hora, motivo opcional, dia e marcos faltantes. [declaracao_texto.py:320](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:320) | `_dia_do_fato`, `_marcos_faltantes`, `_casar_hora` e juiz da resposta já existente. [declaracao_texto.py:331](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:331), [declaracao_texto.py:418](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:418) | Reaponta pergunta órfã ou abre pergunta; responde pelo core P16. [declaracao_texto.py:361](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:361), [declaracao_texto.py:420](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:420) | Conforme ramo, `PerguntaDisputa.chamado_destino`, `data_evento`, resposta e decisão; ocorrência com autor e motivo informado ou texto de ausência de motivo. [declaracao_texto.py:371](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:371), [declaracao_texto.py:423](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:423) | JSON 400 com erro ou sucesso “Horário informado. Aguardando a supervisão validar”; a view envia push ao DP. [views.py:1089](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views.py:1089), [declaracao_texto.py:427](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:427) | Ocorrência e log da declaração; teste específico **[não verificado]**. [declaracao_texto.py:423](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:423), [views.py:1097](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/views.py:1097) |
| **Clique mudo — aprovação / S143** | Ato descendente tenta escrever pergunta de competência entregue; a aprovação ou fechamento externo tem seu próprio estado. [test_e4c_b2_cerca_retratar.py:418](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4c_b2_cerca_retratar.py:418) | `vigiar_escrita_de_perguntas` → `veto_de_competencia`; dentro da cerca, entregue **ou dia indecidível** veta. [fio.py:1358](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1358), [fio.py:1424](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1424) | O abridor desfaz o savepoint do ato cercado e devolve empresas vetadas; uma ação externa pode permanecer concluída. [fio.py:1431](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1431), [test_e4c_b2_cerca_retratar.py:429](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4c_b2_cerca_retratar.py:429) | A escrita cercada é desfeita; `LogAuditoria` recebe a retenção com empresas vetadas, sujeita à deduplicação. [reconciliador.py:459](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:459) | A sonda raspada relata HTTP 204 e toast de sucesso nos dois casos humanos, apesar da retenção; reprodução e mensagem exata no HEAD **[não verificado]**. | O teste do S143 verifica pergunta intacta, chamado encerrado e uma trilha; não é prova do toast HTTP. [test_e4c_b2_cerca_retratar.py:418](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4c_b2_cerca_retratar.py:418) |

A sonda raspada também relata `False` nos atos de beco e HIST. Há testes na cópia que esperam `False` e uma trilha de retenção nesses casos; a execução da sonda e seus nove conjuntos de tabelas permanecem **[não verificado]**. [test_e4c_b2_cerca_retratar.py:435](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4c_b2_cerca_retratar.py:435), [test_e4c_b2_cerca_retratar.py:450](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4c_b2_cerca_retratar.py:450)

### C1–C4: escritores e leitores existentes

C1–C4 são **candidatas do pré-desenho**, não implementações identificadas nesta cópia. Hoje a guarda recebe apenas `perguntas`; seu contexto contém `vetadas`, `memo` e `empresa`. Ela é chamada no `save` de pergunta existente e no lote, que reconstrói o queryset. Assim, escritores e leitores dos possíveis novos valores `origem/autor/motivo` de cada candidata são **[não verificado]**: não há implementação dessas candidatas a censar. [fio.py:1389](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1389), [fio.py:1445](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1445), [models.py:3478](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3478), [models.py:3674](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3674)

| Candidata do anexo | Relação comprovada com o código existente |
|---|---|
| **C1 — parâmetros explícitos** | A assinatura atual não recebe os três valores; em X11, o reaponte mostrado não recebe `user`. Escritores e leitores dos parâmetros propostos **[não verificado]**. [fio.py:1389](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1389), [declaracao_texto.py:371](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/declaracao_texto.py:371) |
| **C2 — `ContextVar` do ato** | `_CERCA` é um contexto existente, mas seu estado não identifica origem, autor ou motivo. Escritores e leitores do contexto humano proposto **[não verificado]**. [fio.py:1381](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1381), [fio.py:1445](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1445) |
| **C3 — atributo transitório** | O lote entrega à guarda instâncias reconstruídas por queryset; transporte e leitura de um atributo proposto **[não verificado]**. [models.py:3478](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3478) |
| **C4 — campo ou registro gravado** | A guarda do `save` roda antes da gravação; esquema, escritores e leitores de um campo ou registro de ato proposto **[não verificado]**. [models.py:3674](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3674) |

O inventário seguinte cobre os campos existentes identificados no pré-desenho como relacionados à guarda e às portas. **Não é o censo de todos os escritores e leitores solicitado**: os exemplos foram localizados, inclusive fora de `app`, mas a completude de cada linha permanece **[não verificado]**.

| Campo existente | Quem escreve — localizado | Quem lê — localizado |
|---|---|---|
| `PerguntaDisputa.data_evento` | Nascimento e gravação do dia do fato. [models.py:3205](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3205), [fio.py:840](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:840) | Guarda, juiz do dia, materialização e consumidor da fila em `mensageria`, via resposta do core. [fio.py:1409](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1409), [juizes.py:338](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/juizes.py:338), [materializacao.py:625](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/materializacao.py:625), [ferramentas.py:116](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/mensageria/nucleo/ferramentas.py:116) |
| `PerguntaDisputa.conversa` | Nascimento. [models.py:3205](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3205) | Guarda, juiz do dia e seleção de perguntas do fato. [fio.py:1409](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1409), [juizes.py:338](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/juizes.py:338), [fabrica_da_celula.py:148](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fabrica_da_celula.py:148) |
| `PerguntaDisputa.chamado_destino` | Nascimento/reuso e reaponte. [models.py:3174](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3174), [fio.py:974](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:974) | Seleção do fio e materialização. A guarda **não o lê diretamente**. [fio.py:135](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:135), [materializacao.py:620](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/materializacao.py:620), [fio.py:1409](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1409) |
| `PerguntaDisputa.materializada_em` | Porta do carimbo e limpeza em lote. [models.py:3293](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3293), [models.py:3485](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3485) | Estado da pergunta e materialização. A guarda **não o lê diretamente**. [models.py:3238](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3238), [materializacao.py:875](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/materializacao.py:875), [fio.py:1409](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fio.py:1409) |
| `DisputaSupervisao.colaborador_id` | Escritores completos **[não verificado]**. [models.py:2046](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:2046) | Guarda usa o ID para obter `Colaborador.empresa_id`; há consulta

[truncado em 20000 de 22927 caracteres]

## 18/11 15:16 · sessao c4d256fdf559 · FILA-CODEX-1811 · leitura · origem=Code

A cópia disponível **não tem metadados Git**, então a identidade do HEAD informado (`dfb866cf8`) fica **[não verificado]**. As linhas abaixo são as da cópia disponível; nela, `_protesta` ainda usa apenas `trabalha` e marcos acesos, enquanto o anexo descreve a alteração DST-L2. [cartorio.py:1458](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/cartorio.py:1458)

**1. DST-L2 × JZ1/S128**

**Quem escreve e quem lê.**

- Na cópia, `_protesta` lê `cel.trabalha` e os marcos da ata do julgamento; o cartório usa os que não protestam e, se não adiados, chama `transicionar_com_pauta(..., ato=x.reconciliar)`. `reconciliar` consulta o gate e grava o novo estado; a porta de pauta coleta eventuais recusas do fechamento. [cartorio.py:1458](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/cartorio.py:1458), [cartorio.py:1533](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/cartorio.py:1533), [models.py:654](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:654), [fecho_runtime.py:146](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fecho_runtime.py:146)
- A S128 acrescenta ao mesmo fecho a retenção BUG 31, a trava e a cerca; no corpo, lê o ponteiro e o status da justificativa, rejeita a vinculada pendente, chama o resolvedor de perguntas e anexa ocorrência. A retenção também cobre justificativa alheia pendente durante uma aprovação. [reconciliador.py:439](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:439), [reconciliador.py:522](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:522), [reconciliador.py:563](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:563)
- A criação ou o save da justificativa dispara o julgamento do dia; os três caminhos citados criam ou obtêm a justificativa antes de chamar `apontar_justificativa`. Essa porta escreve o primeiro ponteiro e `atualizado_em`, mas não testa o estado do chamado nem rejeita a justificativa. [signals.py:161](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/signals.py:161), [api/views.py:1999](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/api/views.py:1999), [ponto/views.py:1638](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:1638), [processar_alertas_turno.py:394](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/processar_alertas_turno.py:394), [vinculos.py:81](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vinculos.py:81)
- `reconciliar_grade` seleciona chamados `OPEN` e, quando aplica a retratação, chama S128. O selo JZ1 exige chamado encerrado, pergunta sem pendência de materialização, disputa fechada, followup de sistema e justificativa rejeitada; seu fixture cria a justificativa antes de gravar o ponteiro. [reconciliar_grade.py:207](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/reconciliar_grade.py:207), [reconciliar_grade.py:303](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/reconciliar_grade.py:303), [test_jz1_autoridade_unica.py:41](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/tests/test_jz1_autoridade_unica.py:41)

**Casos que distinguem as opções.**

1. **Ponteiro já existe no momento do fecho.** Dado chamado vivo, justificativa vinculada pendente e ausência de retenção ou veto: com o fecho cru, o trecho `_porm` só invoca a transição; com S128, o ato também rejeita a justificativa, chama o resolvedor e anexa ocorrência. O resultado das perguntas depende das guardas do resolvedor; não se deve inferir que toda pergunta será carimbada. [cartorio.py:1533](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/cartorio.py:1533), [reconciliador.py:563](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:563)
2. **Irmã respondida e não validada.** Dado o mesmo fecho, mas com irmã que ativa BUG 31: S128 recusa a retratação por retenção. O fecho cru não passa por essa retenção específica, embora continue sujeito ao gate de `reconciliar`; se esse gate permitir ou recusar no caso concreto, o resultado é **[não verificado]**. [reconciliador.py:439](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:439), [reconciliador.py:538](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:538), [models.py:654](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:654)
3. **Justificativa chega depois do fecho.** Dado chamado encerrado sem ponteiro e, depois, justificativa automática pendente: Fix A já executado não pode ter lido esse ponteiro futuro; a porta atual o grava sem rejeitar, e a grade não seleciona o chamado encerrado. Na opção de rejeição na porta, o resultado pretendido é ponteiro gravado e justificativa rejeitada; a definição exata de estados abrangidos, atomicidade e trilha dessa opção é **[não verificado]**. Na opção de limitar JZ1 a chamado aberto, esse caso fica fora do selo; outro responsável pela rejeição tardia é **[não verificado]**. [reconciliador.py:573](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/reconciliador.py:573), [vinculos.py:81](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vinculos.py:81), [reconciliar_grade.py:207](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/reconciliar_grade.py:207)

**Lei já escrita.** A L-061 diz que o consumidor lê a célula/ata; o corte do juiz `dia_sem_trabalho` inclui `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho, mas registra expressamente `_protesta` com a primeira cláusula. O comentário da O261-E-F1 descreve a ordem lavrar → E2 → reconciliar para o lote adiado com fio. Essas fontes delimitam o julgamento e a ordem existente; não encontrei nelas decisão explícita para as duas alternativas de S128 e do ponteiro tardio **[não verificado]**. [LEIS.md:95](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:95), [CORTES.md:459](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/CORTES.md:459), [cartorio.py:1521](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/cartorio.py:1521)

Os números do anexo — 35 chamados na classe da ata gravada e zero justificativas pendentes vinculadas nessa classe — são o dado raspado fornecido, não uma medição reproduzida nesta leitura **[não verificado]**. Eles não demonstram o resultado para a ata do julgamento futuro **[não verificado]**.

**2. F3 — marcos da lavra e decisão retroativa**

**Quem escreve e quem lê.**

- O escritor canônico da resposta humana, `responder_pergunta_disputa_core`, grava texto e `respondida_em` na projeção e uma linha `RESPOSTA` no mesmo ato; uma resposta nova desfaz a validação anterior. [disputa_emissao.py:1439](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1439), [disputa_emissao.py:1474](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1474), [disputa_emissao.py:1527](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1527)
- O segundo escritor relevante é `carimbar_resposta_pela_trilha`: para texto já existente sem carimbo, recebe o instante testemunhado pela nota, grava `respondida_em` e acrescenta uma `RESPOSTA` com esse `ato_em`, preservando a validação. Ele duplica o **efeito de escrever o instante e o ato de resposta**, mas não é equivalente à resposta humana: trata meio-estado legado e não escreve novamente o texto nem desfaz validação. O comando de cura o chama. [disputa_emissao.py:1692](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1692), [curar_perguntas_em_limbo.py:67](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/management/commands/curar_perguntas_em_limbo.py:67)
- `DecisaoPergunta.registrar` insere as linhas; o modelo guarda `ato_em`, mas não declara instante de inserção. `leitura` ordena os atos por `ato_em` e `pk` para reconstruir resposta e validação vigentes. [models.py:4166](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4166), [models.py:4295](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4295), [models.py:4329](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4329)
- `marcos_do_lote` lê datas de batida, célula e projeção da pergunta, inclusive `respondida_em`, mas não consulta `DecisaoPergunta`; `esta_stale` compara o marco com `julgado_em`, além das guardas da própria lavra e do relógio. `lavrar` é a porta dos campos do veredito. [veredito_lavrado.py:151](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:151), [veredito_lavrado.py:177](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:177), [veredito_lavrado.py:201](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:201), [veredito_lavrado.py:213](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:213), [veredito_lavrado.py:254](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:254)

**Casos que distinguem as opções.**

1. **Inserção retroativa depois da lavra.** Dada lavra em T2 e, depois, cura de uma resposta cujo instante testemunhado é T1 < T2, sem outro marco posterior: em **A**, o `respondida_em=T1` não ultrapassa `julgado_em=T2`, e a mera inserção da decisão não entra nos marcos. Em **B**, um marcador de inserção que tenha sido definido e comparado com um snapshot anterior poderia detectar a inserção; o resultado concreto de B permanece **[não verificado]**, pois o material não define esse marcador nem seu snapshot. [disputa_emissao.py:1692](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1692), [veredito_lavrado.py:206](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:206), [veredito_lavrado.py:230](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:230)
2. **Resposta nova após a lavra.** Dada resposta humana em T3 > T2: em A, `respondida_em=T3` pode tornar a lavra stale pelo marco já existente. Em B, a resposta também insere decisão, mas se isso acrescenta alguma invalidação ou altera o resultado depende da definição de B **[não verificado]**. [disputa_emissao.py:1527](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/disputa_emissao.py:1527), [veredito_lavrado.py:206](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:206)
3. **Consulta histórica.** Dado um ato com `ato_em=T1` inserido depois de T2, uma consulta limitada por `ato_em ≤ T2` não consegue, somente por esse campo, distinguir se a linha já existia em T2. Uma coluna de inserção poderia fornecer outro instante para essa distinção; sua semântica, backfill e uso pela F3 são **[não verificado]**. [models.py:4166](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4166), [veredito_lavrado.py:151](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/veredito_lavrado.py:151)

**Lei já escrita.** A L-007 exige um escritor canônico por estado e trilha para ação crítica; a L-130 define resposta por instante **e** texto não vazio. Elas delimitam a escrita e o significado de resposta, mas não estabelecem se F3 deve escolher A ou B **[não verificado]**. [LEIS.md:65](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:65), [LEIS.md:151](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:151)

O dado fornecido rotula **244 inversões em 5.708 decisões** como proxy, não como censo por escritor nem como número de lavras afetadas por B **[não verificado]**. A cópia confirma que a sombra mascara `ato_motivo`; a causa dos zero logs de auditoria e o impacto de B permanecem **[não verificado]**. [sombra_regras_pessoais.py:142](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/bin/sombra_regras_pessoais.py:142)

## 18/11 15:16 · sessao 4b32d17fa48b · FILA-CODEX-1811 · leitura · origem=Code

# CLASSE-D-NA-FRENTE-DA-E4D — material para corte

## 1. Pergunta literal

> “qual dono (CADASTRO, BATIDA ou ESTRUTURA) responde por FATO_EM_AUSENCIA?” [app/docs/RELATO.md:409](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:409)

A pergunta sobre o dono permanece registrada como lei aberta. O RELATO registra como **respondida outra pergunta**: `FATO_EM_AUSENCIA` deve chegar dia a dia ao juiz de dono. [app/docs/RELATO.md:409](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:409) [app/docs/RELATO.md:392](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:392)

## 2. Opções e efeito no código de hoje

O juiz `dono_da_divergencia` **ainda não recebe `FATO_EM_AUSENCIA` como insumo**. Hoje ele retorna CADASTRO para folga não definida ou ausência de vínculo; depois BATIDA para batida fora de turno ou quantidade ímpar; depois CADASTRO para fase deslocada ou dia na lista do motor; e ESTRUTURA como resíduo. Portanto, as opções abaixo descrevem o efeito dos donos nas portas existentes, não um encaminhamento já implementado especificamente para essa classe. [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/e6_oraculo.py:53) [app/ponto/management/commands/e6_oraculo.py:98](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/e6_oraculo.py:98)

| Opção | Efeito nas portas existentes |
|---|---|
| **CADASTRO** | Em uma assinatura diária que use `_porta_pelo_dono`, a ação é a porta de cadastro própria da assinatura. Se o dia constar na lista com esse dono, o vigia pode classificá-lo como `fio_do_admin`, respeitada a precedência das demais condições do vigia. [app/escala/services/cadastro_realidade.py:731](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:731) [app/chamados/services/vigia_fio.py:254](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vigia_fio.py:254) |
| **BATIDA** | Na mesma porta diária, a ação é “Regularizar a batida do dia”. Se o dia constar na lista com esse dono, o vigia pode classificá-lo como `fio_do_admin`, sob as mesmas condições. [app/escala/services/cadastro_realidade.py:738](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:738) [app/chamados/services/vigia_fio.py:200](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vigia_fio.py:200) |
| **ESTRUTURA** | A porta diária mostra “Com o sistema: em cura (nao e do cadastro)”, sem URL; esse dono não satisfaz o predicado `dia_do_admin` do vigia. A L-099 reserva a fatia de código a ESTRUTURA. [app/escala/services/cadastro_realidade.py:743](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:743) [app/chamados/services/vigia_fio.py:200](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vigia_fio.py:200) [app/docs/LEIS.md:122](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:122) |
| **Sem dono lavrado / sem resposta** | É um estado técnico já previsto, não um quarto dono da L-099: a porta diária informa que a próxima lavra trará o dono; `dias_na_lista` pode devolver `None` para donos inconciliáveis, e o vigia mantém contagem de dias sem resposta da lista. [app/escala/services/cadastro_realidade.py:738](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:738) [app/escala/services/cadastro_realidade.py:1020](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:1020) [app/chamados/services/vigia_fio.py:166](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vigia_fio.py:166) |

**Limite da rota atual:** A9 é uma assinatura de ausência com batida que o esmeril coleta por dia; ao montar a lista, a assinatura agregada é apresentada com `ocorrencias`, sem campo `data`, e recebe a ação padrão “em cura”. `dias_na_lista` só incorpora assinaturas com `data` e lê `dono_do_dia`. A9 e `FATO_EM_AUSENCIA` não devem ser presumidos universos idênticos: A9 usa `ausencia_id` e pelo menos duas batidas, enquanto o supra-juiz também tem ramo de batida sem par. [app/ponto/services/esmeril_espelho.py:133](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/esmeril_espelho.py:133) [app/escala/services/cadastro_realidade.py:922](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:922) [app/escala/services/cadastro_realidade.py:728](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:728) [app/escala/services/cadastro_realidade.py:1043](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/services/cadastro_realidade.py:1043) [app/ponto/supra_juiz.py:450](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/supra_juiz.py:450)

**Placar:** o R1 apresenta divergências do E6 por três donos; o contador de soma compara as partes com o total. Os números publicados pertencem ao universo divergente do E6, não são contagem específica de `FATO_EM_AUSENCIA`. [app/core/placar_estrutural.py:115](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/core/placar_estrutural.py:115) [app/core/placar_estrutural.py:123](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/core/placar_estrutural.py:123)

## 3. Onde o fato nasce e é lido

- `classificar_dia` produz `FATO_EM_AUSENCIA` no ramo de fato em dia encerrado coberto por ausência aprovada; parcial sem período vira `PARCIAL_SEM_PERIODO`, e período fora segue outro ramo. Há também produção para batida sem par em parcial coberta, com período diferente de `fora`. [app/ponto/supra_juiz.py:387](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/supra_juiz.py:387) [app/ponto/supra_juiz.py:450](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/supra_juiz.py:450)
- O cartório chama `classificar_dia` e recolhe os códigos; `derivar_veredito` mapeia o código para `fato_em_ausencia`, sujeito à precedência do mapa. A célula inclui esse veredito entre os que acusam. [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/cartorio.py:1464) [app/escala/models.py:1611](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1611) [app/escala/models.py:1767](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1767)
- A tarefa do dia lê o **veredito lavrado** e, nas condições indicadas no código, expõe ação `VALIDAR` para o admin quando não há pergunta viva. [app/chamados/services/tarefa_do_dia.py:352](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:352)
- A fábrica consulta `VEREDITOS_ACUSAM`; a folha também lê esse conjunto na apuração dos motivos de retenção. [app/chamados/services/fabrica_da_celula.py:120](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/fabrica_da_celula.py:120) [app/folha/export.py:1273](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/folha/export.py:1273)

## 4. Números disponíveis e faltantes

- O RELATO registra **10 casos `FATO_EM_AUSENCIA` na classe D e 57 na frota**; na mesma linha, registra **14 `PARCIAL_SEM_PERIODO`** em outra população, dos quais 10 na classe D. São retratos registrados, não contagens atuais verificadas. [app/docs/RELATO.md:904](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:904)
- Outro recorte registra **21 dias com ausência aprovada**, divididos em 10 `PARCIAL_SEM_PERIODO`, 10 `FATO_EM_AUSENCIA` e 1 colaborador de teste; registra que a frota além desses 21 não foi medida naquele recorte. [app/docs/RELATO.md:834](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:834)
- O placar registra, para a medição de 06/11, **303 dias divergentes na competência 10** — 26 ESTRUTURA, 78 CADASTRO, 199 BATIDA — e **201 na 11** — 9, 72 e 120, respectivamente. A fonte ressalta que os colaboradores por dono não somam o total, pois o total é a união. [app/core/placar_estrutural.py:127](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/core/placar_estrutural.py:127)
- **[não verificado]** Total atual de `FATO_EM_AUSENCIA`, interseção com A9, distribuição por dono e quantos chegam à lista diária ou ao `fio_do_admin`. **Sonda que mediria:** leitura por `(colaborador, dia)` do código/veredito da célula, das datas A9, do resultado de `InsumosDoDono.dono` e de `dias_na_lista`, com cruzamento dos resultados do vigia; registrar separadamente ausência de resposta. [app/escala/models.py:1619](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/models.py:1619) [app/ponto/services/esmeril_espelho.py:133](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/esmeril_espelho.py:133) [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/e6_oraculo.py:182) [app/chamados/services/vigia_fio.py:166](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/vigia_fio.py:166)
- O RELATO registra cruzamento de **0 arquivos** de `cadastro_realidade.py` e `e6_oraculo.py` com B2 e **0** com F2; registra `vigia_fio.py` nas duas e a obra fechada enquanto o encaminhamento precisar editá-lo. [app/docs/RELATO.md:379](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:379)

## 5. Leis vizinhas

- **L-099:** separa estrutural de dado, exige coerência com o cadastro existente e nomeia ESTRUTURA, CADASTRO e BATIDA; só ESTRUTURA é fatia de código. [app/docs/LEIS.md:122](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:122)
- **L-040:** define uma lista única Cadastro x Realidade, com assinatura e ação, e o PDF como a mesma lista. [app/docs/LEIS.md:88](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:88)
- **L-130:** define pergunta respondida como instante **e** texto não vazio, com um juiz para as finalidades listadas; trata da resposta à pergunta, não atribui por si o dono de `FATO_EM_AUSENCIA`. [app/docs/LEIS.md:151](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:151)
- **L-020, célula soberana:** dia e marcos são dado com DNA congelado e irretroativo; a exceção formal para passado errado por cadastro passa pela porta e a célula denuncia o ato. [app/docs/LEIS.md:71](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/LEIS.md:71)

## 18/11 15:16 · sessao 1008fd345092 · - · - · origem=Ronald (tmux)

- O juiz registrado para a pergunta “a que turno pertence a batida?” é `ponto/turnos.py::turnos_do_colab`; o recorte por batida é `dia_do_turno_da_batida`. [app/core/juizes.py:216-217; app/ponto/turnos.py:2939-2948]

- Os dois sítios de `folha/porta_export.py` têm caminho que **grava**: o comando chama `medir` e `concluir`, que chama `lavrar`; o snapshot inclui as listas produzidas pelos dois sítios. A gravação depende da tentativa ainda vigente. [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:219-220; app/folha/porta_export.py:1255-1274; app/folha/porta_export.py:779-795]

- `_periodos_pelo_marco` participa do cálculo do fechamento, com condições para uso da ata; o próprio código consulta `periodos_do_dia`, juiz registrado para períodos e intervalo. [app/ponto/motor_calculo_v2.py:559-578; app/ponto/motor_calculo_v2.py:611-616; app/core/juizes.py:1207-1209; app/ponto/motor_calculo_v2.py:530-531; app/ponto/motor_calculo_v2.py:2748; app/ponto/motor_calculo_v2.py:2354; app/ponto/services/fechamento.py:286]

- `_previsto_do_dia`, `_intervalo_descontado_no_dia` e `_janela_do_dia` usam a data do argumento ou entrada como chave; seus resultados chegam ao cálculo de período e ao fechamento. A autoridade específica para converter cada argumento em dia de jornada permanece [não verificado]. [app/ponto/motor_calculo_v2.py:1227; app/ponto/motor_calculo_v2.py:1313; app/ponto/motor_calculo_v2.py:1528; app/ponto/motor_calculo_v2.py:1273; app/ponto/motor_calculo_v2.py:1803; app/ponto/motor_calculo_v2.py:1760; app/ponto/motor_calculo_v2.py:2835; app/ponto/motor_calculo_v2.py:2840; app/ponto/motor_calculo_v2.py:2759; app/ponto/motor_calculo_v2.py:2458; app/ponto/services/fechamento.py:286]

- O detector de proposta agrupa por data local e declara a limitação para turnos noturnos. Há caminho de exibição pelo comando `propor_escala_limbo`; o efeito de todos os demais consumidores permanece [não verificado]. [app/escala/detector_proposta.py:18-25; app/escala/detector_proposta.py:55-58; app/escala/detector_proposta.py:118-120; app/escala/management/commands/propor_escala_limbo.py:22-48; app/escala/services/escala_auto.py:119-120]

## 18/11 15:16 · sessao 1d3a2603441f · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, incluindo a rota de `views_core`, as condições de alcance dos ramos do calculador e a identificação de `_match_marcos` como juiz registrado. [app/config/urls_core.py:6; app/core/juizes.py:221; app/ponto/calculador/nucleo.py:110]

## 18/11 15:16 · sessao b796513d1d0b · FILA-CODEX-1811 · leitura · origem=Code

**Achado ESTRUTURA — dono: ESTRUTURA.** Há duas derivações para minutos fora da janela: o motor registra minutos por período; o espelho monta pontas a partir da grade e acrescenta relatos do motor. A Gestão de HE lê o retrato produzido a partir das pontas do espelho. Isso permite que os dois números diverjam. [motor_calculo_v2.py:2787](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2787), [espelho.py:721](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:721), [he_pendente_lavrado.py:132](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/he_pendente_lavrado.py:132)

O anexo `logs/CONFRONTO-PILOTO/he_fora_janela.r` não está presente na cópia; os resultados da sonda abaixo são transcritos do dado raspado fornecido, sem verificação independente nem linha de arquivo disponível. **[não verificado]**

### 1. As duas contas, passo a passo

| Etapa | Motor | Espelho e retrato |
|---|---|---|
| **Entrada** | Calcula períodos a partir de pares de batidas e intervalos. No 12x36, um período sem saída retorna aberto antes de calcular as pontas; há tratamento posterior que pode creditar partes fechadas em período aberto. [motor_calculo_v2.py:2739](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2739), [motor_calculo_v2.py:2549](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2549) | Parte das células de realizado atribuídas à grade; depois acrescenta pontas traduzidas do relato do motor e dias sem previsto publicados pelo motor. [espelho.py:684](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:684), [espelho.py:727](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:727) |
| **Vigência e janela** | Exige empresa com janela ativa e data de vigência alcançada. Busca os marcos do dia da entrada; folga por par, ausência de marco e guarda L-084 podem afastar o recorte. Dia autorizado não é clipado. [motor_calculo_v2.py:1389](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1389), [motor_calculo_v2.py:1520](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1520), [motor_calculo_v2.py:1691](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1691) | Também exige janela ativa e vigente, mas `_marcar_pontas` recebe as células e não a decisão ou a guarda L-084 do motor. A decisão viva é agregada depois, na Gestão. [espelho.py:721](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:721), [gestao_he.py:270](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/gestao_he.py:270) |
| **Piso e extra declarada** | Mede chegada antes e, se ligada, saída depois; subtrai a extra declarada daquela ponta. Se a sobra é `<= piso`, registra zero; se é maior, registra **toda a sobra**, sem subtrair o piso. [janela_he.py:92](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:92), [janela_he.py:143](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:143) | Usa a mesma comparação de piso e subtração da extra, mas sobre o `delta` da célula atribuída. O relato traduzido do motor já traz a sobra e não desconta a extra outra vez. [janela_he.py:199](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:199), [espelho.py:736](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:736) |
| **Arredondamento** | A diferença entre instantes usa segundos divididos por 60; o campo soma as duas sobras como `float`. A tolerância diária de 10 minutos não é aplicada diretamente nesse campo. [janela_he.py:94](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:94), [motor_calculo_v2.py:2787](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2787) | A grade calcula `delta` com `int(round(...))`; a ponta marcada também recebe `int(round(...))`. O retrato soma os inteiros das pontas. Portanto, o arredondamento pode alterar inclusive a passagem pelo piso e produzir zero de um lado e positivo do outro. [utils.py:325](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/escala/utils.py:325), [janela_he.py:215](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:215), [he_pendente_lavrado.py:68](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/he_pendente_lavrado.py:68) |
| **Meia-noite** | A entrada é comparada ao marco no seu dia; o marco de saída é um `datetime` resolvido com a regra de turno que cruza meia-noite. [motor_calculo_v2.py:1543](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1543) | Usa o dia e a atribuição da grade. Para recuperar ponta do relato, exige célula de ponta `missing` e mesmo marco; célula ausente, atribuída ou com marco diferente é recusada. Assim, diferenças na atribuição ao dia ou marco podem impedir a tradução. [janela_he.py:267](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:267), [janela_he.py:293](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:293) |
| **Intervalo** | As pontas do par são recortadas primeiro; os intervalos são recortados à janela efetiva e entram na duração trabalhada. O campo de ponta soma as sobras de entrada e saída. [motor_calculo_v2.py:2759](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2759) | A marcação escolhe a primeira célula de entrada e a última de saída dos **marcos**, excluindo as células intermediárias do intervalo. Em dia sem previsto, traduz os minutos já publicados pelo motor, cujo cálculo considera o intervalo. [janela_he.py:181](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:181), [motor_calculo_v2.py:1434](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1434) |
| **Batida retratada** | Usa os instantes reais do par que calcula; pode somar contribuições de mais de um período. [motor_calculo_v2.py:2750](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2750), [motor_calculo_v2.py:2550](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2550) | Retrata a batida atribuída à célula extrema; a recuperação pelo relato admite no máximo uma ponta por sentido, sob suas guardas. Uma batida fora do raio de atribuição pode ser recuperada, mas não em todos os formatos de grade. [janela_he.py:194](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:194), [janela_he.py:234](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:234) |

No caso especial de folga ou feriado elegível com previsto zero, sem autorização, o motor transfere os minutos trabalhados calculados para `minutos_fora_janela`; o espelho traduz a lista publicada como ponta de sentido `dia`, sem marco. As guardas de elegibilidade não equivalem simplesmente a “qualquer dia sem previsto”. [motor_calculo_v2.py:1440](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1440), [motor_calculo_v2.py:1471](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1471), [espelho.py:743](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:743)

### 2. O que explica os 952 e os 236

A medida fornecida conta **952 dias com motor positivo e montador zero** e **236 no sentido inverso**. A anatomia por causa dos 236 não foi medida. Para os 952, o dado não identifica a causa individual dos zeros; arredondamento, atribuição de batida, marcos/dia, guardas e multiplicidade de períodos são mecanismos a sondar, não contagens atribuíveis à medida. **[não verificado]**

A lista fornecida acrescenta:

- **64 dias** com motor acima de 10 minutos, fora da tela e sem decisão: **60** têm montador zero e `pontas=[]`; os outros **4** têm ponta `dia` positiva e são de desligados. A lavratura filtra colaboradores ativos, o que sustenta a explicação do filtro para esses quatro, embora o dado diga que o filtro não foi medido individualmente. [he_pendente_lavrado.py:92](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/he_pendente_lavrado.py:92) **[não verificado quanto à medida]**
- Isso exige uma ressalva: a afirmação do anexo de que os **952 fora da tela são exatamente os 952 com montador zero** não acomoda, literalmente, os quatro listados com montador positivo e fora da tela. A relação exata entre os conjuntos e denominadores precisa ser conferida; não há base para ajustar os números. **[não verificado]**
- O piloto `col719` em 22/10 tem motor 48,31 e Gestão 48; em 21/10 ambos os derivadores dão zero para os 9,6 minutos antes do marco. Isso ilustra o efeito do piso e da representação inteira, sem provar uma causa para os conjuntos de divergência. **[não verificado]**

**Desenho da sonda:** para cada pessoa-dia dos dois sentidos, registrar entradas e saídas reais dos pares, dia atribuído, intervalos, marcos e vigência, piso, extras declaradas, decisão, guardas L-084 e folga, minutos `float` por ponta/período; registrar também células da grade com `tipo`, `missing`, batida, marco, `delta` antes/depois do arredondamento, pontas marcadas, relatos candidatos e motivo de cada guarda de rejeição, lista sem previsto, soma do montador e inclusão no retrato de ativos. Classificar causas com possibilidade de múltiplas causas e publicar tanto contagens por motivo quanto interseções; comparar separadamente presença no montador e presença na tela. Os números por causa até essa sonda são **[não verificado]**. As guardas a instrumentar estão em [janela_he.py:267](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:267) e o filtro de lavratura em [he_pendente_lavrado.py:92](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/he_pendente_lavrado.py:92).

### 3. Os três números de “pequeno demais”

| Número | Onde e valor | Quem lê |
|---|---|---|
| **Tolerância diária de HE** | Constante `TOLERANCIA_HE_MIN_DIA = 10`. [motor_calculo_v2.py:51](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:51) | Leituras de cálculo no próprio motor: [2845](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:2845), [3151](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:3151), [3179](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:3179) e [3414](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:3414); calculadores [periodo.py:418](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/calculador/periodo.py:418) e [regras.py:774](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/calculador/regras.py:774); calendário em [calendario.py:74](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/services/calendario.py:74). |
| **Piso por ponta da janela** | `Empresa.janela_he_piso_min`, padrão **0**. [models.py:69](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:69) O dado informa **10 na emp1 e 0 nas emp2/3/4**. **[não verificado]** | Leitores diretos de execução: motor [motor_calculo_v2.py:1649](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/motor_calculo_v2.py:1649), montador [espelho.py:388](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/espelho.py:388) e calendário [calendario.py:378](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/services/calendario.py:378). A comparação compartilhada está em [janela_he.py:41](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:41). |
| **Limite de decisão do dia** | `Empresa.limite_decisao_he_min`, padrão **15**. [models.py:82](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/models.py:82) O dado informa **15 nas quatro empresas**. **[não verificado]** | Views da Gestão em [views.py:596](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:596), [669](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:669), [808](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:808) e [945](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/views.py:945); lavratura [lavrar_he_pendente.py:54](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/lavrar_he_pendente.py:54); recusa automática [recusar_ponta_pequena.py:92](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/management/commands/recusar_ponta_pequena.py:92); autorização por período [autorizacao_he_periodo.py:276](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/autorizacao_he_periodo.py:276); trava de exportação [porta_export.py:685](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/folha/porta_export.py:685); administração exibe o campo em [admin.py:9](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/colaboradores/admin.py:9). O predicado compartilhado compara o total inteiro do dia com `> limite`. [he.py:277](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/portas/he.py:277) |

**Achado ESTRUTURA — dono: ESTRUTURA:** os três números respondem a perguntas distintas e operam em pontos distintos da cadeia: tolerância sobre excedente, piso por ponta e limite de decisão sobre o total retratado do dia. A medida fornecida registra **15 dias com motor ≤10 minutos ainda como pendência na Gestão**; ela não mede aqui o efeito monetário da tolerância. [calculador/periodo.py:416](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/calculador/periodo.py:416), [janela_he.py:215](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/janela_he.py:215), [gestao_he.py:274](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/ponto/services/gestao_he.py:274) **[não verificado quanto à medida]**

## 18/11 15:15 · sessao 52a5bf2c602c · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal com cadeias, efeitos e referências de arquivo:linha. Os pontos em que a correspondência com um juiz registrado ou a causalidade histórica não foi provada estão marcados como [não verificado].

## 18/11 15:15 · sessao 1056077d10a0 · FILA-CODEX-1811 · leitura · origem=Code

A definição das três classes está na cópia, mas **não é possível entregar uma sonda completa e verificável para as exceções indiretas** com o material disponível: falta a implementação ou um contrafactual fiel da F5 para comparar os dois estados e atribuir causalidade a uma PK. A sonda parcial abaixo mede o confronto entre ato e L-130; não a executei. [não verificado] [nao verificado]

**(A) Predicados e associação**

- **(a) SILÊNCIO:** último ato aplicável `SILENCIO`; a regra manda classificá-lo como histórico. `_pela_decisao` retorna `silencio_decidido` e nenhuma ação, salvo a precedência de `passivo_sem_linha`. A carga ordena os atos por instante e PK, corta em `agora` e exclui atos marcados como ressuscitação antes de escolher o último. [CORTES.md:561](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/CORTES.md:561), [tarefa_do_dia.py:154](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:154), [tarefa_do_dia.py:421](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:421)

- **(b) RESPOSTA vazia:** último ato aplicável `RESPOSTA` e `texto_vazio(ato_texto)`; a regra determina pendente no universo de perguntas vivas e disputa aberta. `_pela_decisao` chama o juiz de texto e devolve `respondida_sem_texto` com `responder/colaborador`, se o passivo não tiver precedência. O aval transcrito determina expressamente que o critério para a lista é o **ato**. [CORTES.md:561](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/CORTES.md:561), [tarefa_do_dia.py:154](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:154); aval transcrito no pedido [não verificado em arquivo].

- **(c) ABSORÇÃO rejeitada:** último ato aplicável `ABSORCAO` cuja ausência ligada tem `status='rejeitada'`; a regra determina pendente e `_pela_decisao` devolve `absorcao_rejeitada` com `responder/colaborador`, sujeito à mesma precedência do passivo. [CORTES.md:561](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/CORTES.md:561), [tarefa_do_dia.py:178](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:178)

- **L-130:** `chamados.juizes.pergunta_respondida(p)` exige `respondida_em` e `resposta_colab` não vazio após `strip`; lê a projeção da pergunta, não o texto do último ato. Portanto, o contador precisa tratar os dois resultados separadamente. [juizes.py:1526](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/juizes.py:1526), [juizes.py:1544](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/juizes.py:1544)

- **Indireto e causador:** a tarefa agrega as ações de todas as instâncias e só publica a porta do colaborador quando uma instância tem `responder/colaborador` **e** sua PK está entre as pendentes do app. Se a rota ficar nula e o calendário oferecer `resolver_dia`, acrescenta essa ação à primeira instância — que pode pertencer a outra pergunta — e publica `admin/porta_acao`. A ligação causal exige comparar antes/depois mantendo os demais dados iguais e identificar qual alteração de pendência de PK (a), (b) ou (c) produziu a diferença; compartilhar dia, por si só, não prova causa. O aval transcrito determina vermelho para mudança indireta sem essa causa nominal. [tarefa_do_dia.py:642](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:642), [tarefa_do_dia.py:656](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:656), [tarefa_do_dia.py:689](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:689); aval transcrito [não verificado em arquivo].

- **`sem_associacao`:** quando nenhuma instância da tarefa aponta para a PK, esse marcador representa o par conservado da pergunta, não a tarefa inteira do dia. Por exemplo, `fora_vinculo` pode retornar sem instâncias, e a exposição de motivo fora do mapa não necessariamente associa a pergunta. [tarefa_do_dia.py:208](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:208), [tarefa_do_dia.py:342](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:342); marcador no aval transcrito [não verificado em arquivo].

**(B) Texto de sonda parcial — contador ato × L-130**

Este trecho usa um único `agora`, as funções reais de texto e do juiz, e abre transação PostgreSQL somente de leitura. Compara **todos os registros de pergunta**, não apenas as vivas no app; por isso seus números não são diretamente os da matriz. A seleção do último ato segue a ordenação e a exclusão usadas pela carga da tarefa. Não imprime texto, nome ou CPF. [tarefa_do_dia.py:421](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/services/tarefa_do_dia.py:421), [juizes.py:1544](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/juizes.py:1544)

```python
# Django shell; PostgreSQL; executar apenas quando a sonda tiver a vaga.
from django.db import connection, transaction
from django.utils import timezone
from chamados.models import PerguntaDisputa, DecisaoPergunta as D
from chamados.juizes import texto_vazio, pergunta_respondida

agora = timezone.now()

if connection.vendor != "postgresql":
    raise RuntimeError("Sonda exige transacao PostgreSQL somente leitura")

with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")

    ultimos = {}
    atos = (D.objects.filter(ato_em__lte=agora)
            .order_by("pergunta_id", "ato_em", "pk")
            .iterator(chunk_size=2000))
    for ato in atos:
        if not D.es_da_ressuscitacao(ato):
            ultimos[ato.pergunta_id] = ato

    # Chaves: (ultimo ato e RESPOSTA vazia, juiz diz respondida).
    grupos = {(False, False): [], (False, True): [],
              (True, False): [], (True, True): []}
    perguntas = PerguntaDisputa.objects.all().order_by("pk").iterator(chunk_size=2000)
    for p in perguntas:
        ato = ultimos.get(p.pk)
        b = bool(ato and ato.ato == D.RESPOSTA and texto_vazio(ato.ato_texto))
        juiz = pergunta_respondida(p)
        grupos[(b, juiz)].append(p.pk)

    for b, juiz in ((True, True), (False, False),
                    (True, False), (False, True)):
        pks = grupos[(b, juiz)]
        print("ATO_%d_JUIZ_%d CONTADOR %d" % (b, juiz, len(pks)))
        for pk in pks:
            print("ATO_%d_JUIZ_%d PK %d" % (b, juiz, pk))

print("FIM_SONDA")
```

Os quatro grupos evitam uma ambiguidade: comparando literalmente os booleanos `ato_vazio` e `pergunta_respondida`, as discordâncias são `ATO_1_JUIZ_0` e `ATO_0_JUIZ_1`; comparando **“ato vazio” com “juiz diz não respondida”**, as discordâncias são `ATO_1_JUIZ_1` e `ATO_0_JUIZ_0`. Os PKs e contadores de ambos ficam disponíveis na saída. [juizes.py:1544](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/juizes.py:1544)

**(C) Limites [nv]**

- [nv] A lista nominal completa (a)/(b)/(c), seus cruzamentos e as exceções indiretas com e sem causador. O resumo recebido informa que o contrafactual trocou `_pendentes_app` em memória e não executou a F5 real; o código dessa sonda não foi fornecido. A mudança de pendência sozinha não demonstra todas as diferenças da F5. [não verificado]
- [nv] A sonda parcial não foi executada nem testada contra o banco; não prova que uma sonda completa consiga percorrer todas as funções reais sem tentativa de escrita ou relógio adicional. O próprio registro da matriz assinala uma possível escrita *best-effort* durante leitura. [RELATO.md:30](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:30)
- [nv] O hash Git do HEAD: esta cópia não contém `.git`, portanto não pude verificá-lo. [não verificado]
- [nv] A fidelidade da sonda da matriz anterior: seu código não foi fornecido, conforme o próprio anexo `r2`. [não verificado]

O **número principal a medir** é o de perguntas conservadas cujo par `razão/ações` muda fora das exceções nominais autorizadas, incluindo os dias indiretos sem PK causadora: **esperado zero pela regra; valor medido [nv]**. A matriz anterior registra **368 perguntas**, distribuídas em **263 colaborador, 68 admin e 37 ninguém em 16 colaboradores**; essa classificação usa precedência própria e não equivale necessariamente à associação da PK na tarefa. Não há, nos dados fornecidos, divergência nominal medida entre essa matriz e uma sonda nova. [RELATO.md:26](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:26), [RELATO.md:30](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:30)

## 18/11 15:15 · sessao 4335686bde57 · - · - · origem=Ronald (tmux)

Leitura de X4, A9b, X11 e do clique mudo concluída e enviada ao agente principal, com referências ao código e aos testes localizados. Os testes não foram executados. [app/chamados/tests/test_e4_expand_orm.py:63](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4_expand_orm.py:63)

## 18/11 15:15 · sessao 156e23fc3fa6 · - · - · origem=Ronald (tmux)

Levantamento enviado ao agente principal, com os pontos de escrita e leitura localizados, inclusive consumidores fora de `app`, e com a ressalva de que o censo completo permanece **[não verificado]**.

## 18/11 15:14 · sessao 1a24b3e4db43 · - · - · origem=Ronald (tmux)

Leitura de P8, P9 e P16 enviada ao agente principal, com estados de entrada, juízes, operações, campos, retornos e testes identificados. Os testes foram apenas lidos, não executados [não verificado]. [nao verificado]

A cópia não contém `.git`; portanto, a identidade exata do HEAD ficou **[não verificado]**.

_7013 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
