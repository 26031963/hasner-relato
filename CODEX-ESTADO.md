# CODEX-ESTADO

> Ultima resposta processada: **23/11/2026 17:05** · publicado em **23/11/2026 17:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 23/11 17:05 · sessao 4fa4050bce72 · NOTURNO-NAO-APURADO · leitura · origem=Code

**332 dias.** Em **327**, a linha `PERIODO` exibida tem uma ponta em `[22:00, 05:00)` no horário local. São **310 saídas** e **17 entradas de períodos abertos**. Das 310 saídas, **295** passam das 22h e **15** aparecem exatamente às `22:00:00`. A sonda imprime horas até o segundo, sem frações. `ramo2_linhas.txt:1–664,666`, `noturno_ramo_frota.py:38–43,82–83,133–142`.

| Minutos de relógio em 22h–05h no par fechado exibido | Dias | Fonte |
|---|---:|---|
| Mais de 0 até 10 | **240** | `ramo2_linhas.txt:1–664` |
| Mais de 10 até 30 | **8** | `ramo2_linhas.txt:1–664` |
| Mais de 30 até 60 | **8** | `ramo2_linhas.txt:1–664` |
| Mais de 60 | **39** | `ramo2_linhas.txt:1–664` |
| Zero na precisão exibida | **19** | `ramo2_linhas.txt:1–664` |
| Duração indeterminada, único período exibido em aberto | **18** | `ramo2_linhas.txt:1–664` |

Os **19** são pares fechados diurnos pelos horários exibidos, **15** com saída às `22:00:00` e **4** com saída anterior. Exemplo de `10:00:00–22:00:00`, **col769 2026-10-30**. O total de jornadas inteiras com **zero legal real**, considerando eventuais períodos omitidos e frações de segundo, é **[nao verificado]**. `ramo2_linhas.txt:1–664,209–210`, `noturno_ramo_frota.py:82–83,133–147`.

### 1. O que a sonda testa

- **Universo C1 reproduzido pela sonda.** Exige uma batida apurável de **qualquer tipo** às 22h ou depois, atribuída à data local, ou antes das 05h, atribuída à véspera. Cruza esse conjunto com `DiaPago` de tipo dia, versão `motor`, realizado positivo e noturno gravado igual a zero, na competência que contém a data. `batidas_apuraveis` exclui batidas retratadas. O filtro mede **presença de marcação na faixa**, inclusive entrada solta ou saída às 22h exatas. O predicado original `noturno > 0` de `g3.py` é **[nao verificado]**. `noturno_ramo_frota.py:32–57`, `app/ponto/turnos.py:2896–2904`, `app/ponto/janelas.py:60–63`.
- **`periodo_fechado_sem_noturno`.** A captura tem períodos, a soma de `minutos_noturnos_legais` dá zero, existe pelo menos um período com `minutos_trabalhados > 0` e **todos os positivos** têm saída e `turno_aberto=False`. O ramo de crédito de aberto é testado antes. Um período aberto com zero trabalhado pode coexistir no dia. `noturno_ramo_frota.py:86–112,119–124,132–147`.
- **De onde vem o valor comparado.** A sonda lê o `DiaPago.horas_noturnas` gravado e multiplica por 60. Na reapuração, intercepta `dia_pago.lavrar` da versão `motor` e soma o `p.minutos_noturnos_legais` dos períodos dos quatro argumentos. Esse campo vem de `MotorBase.calcular_noturnas`, por `_escrever_noturnas`. A sonda compara o gravado com a saída da reapuração do motor. Um cálculo independente do noturno legalmente devido é **[nao verificado]**. `noturno_ramo_frota.py:45–56,60–79,119–144`, `app/ponto/services/fechamento.py:1200–1204,1227`, `app/ponto/motor_calculo_v2.py:969–1005`.

### 2. Onde o noturno vira zero

1. **O par é recortado antes do cálculo do noturno.** O fechamento escolhe o motor pelo cadastro da fatia. `Motor12x36`, `MotorComercial` e `MotorTurnoPartido` guardam as pontas originais em `periodo.entrada/saida`, usam `_janela_do_par` para obter `e/s` efetivos e passam estes a `_escrever_noturnas`. O regime de cada linha é **[nao verificado]**. `app/ponto/services/fechamento.py:724–741`, `app/ponto/motor_calculo_v2.py:2750–2765,2785–2792,2877–2896,3215–3238,3256–3260`.
2. **A L-097 aplica o corte.** O motor lê os marcos do dia, a vigência e configuração da janela por empresa, a extra declarada e a decisão de HE. Acima do piso aplicável, `entrada_efetiva` devolve o limite de entrada e `saida_efetiva` devolve o limite de saída. Uma autorização preserva as pontas originais. A L-084 também afasta a janela quando ambas as pontas excedem três horas de distância dos marcos. `app/docs/LEIS.md:115–116`, `app/ponto/motor_calculo_v2.py:1389–1395,1529–1571,1582–1629,1641–1653,1691–1712,1733–1763,1930–1949,2290–2291`, `app/ponto/janela_he.py:94–103,145–153`.
3. **O zero é calculado sobre o trecho que sobrou.** Se a saída efetiva de um par diurno termina às 22h, `inter_ini < inter_fim` falha para a faixa noturna e a soma fica zero. Se ainda houver interseção, `calcular_noturnas` desconta os intervalos julgados que caem nela, limita o resultado a zero e aplica o fator da hora noturna. A atribuição final está na linha **1005**. O teste de 5/10 min do art. 58 §1 trata variações de marcação. No motor, as guardas de pontualidade e HE atuam nas próprias rubricas, enquanto o corte das pontas usa o **piso cadastrado da janela de HE**. `app/ponto/motor_calculo_v2.py:98–118,983–1005,1647–1653,2005–2010,2842–2848,3151–3152,3412–3416`, `app/ponto/janela_he.py:149–153`, `app/ponto/tests/test_o280_bateria_da_lei.py:3294–3299`.
4. **O bloqueio integral é outro caminho.** Depois de `calcular_periodo`, `_dia_sem_previsto_fora_da_janela` exige folga ou feriado, janela ativa, previsto igual a zero e ausência de autorização. Folga sem escala certa fica fora do bloqueio. As linhas **1473–1476** zeram **trabalhado e noturno juntos**. Portanto, **zero** dos períodos positivos que sustentam o ramo pode ter sido zerado por esse caminho. A presença de outros períodos bloqueados nesses dias é **[nao verificado]**. `app/ponto/motor_calculo_v2.py:1440–1483,2457–2461`, `noturno_ramo_frota.py:92–111`.

### 3. Onde realizado e noturno se separam

O **realizado** vem do juiz de turnos, que usa as pontas dos pares fechados truncadas ao minuto e desconta a pausa batida ou, quando aplicável, a pausa fixa declarada. A ata grava esse número, a grade o lê e `por_dia_da_grade` aplica `min(realizado, previsto)`. Já o **noturno** vem dos períodos que o motor monta e pode substituir pelo julgamento dos marcos da ata, **depois** do recorte de HE em `_janela_do_par`. A lavratura recebe as duas saídas separadas. Em **col704 2026-10-10**, são **637 min** de realizado da grade, **519,96 min** trabalhados no período, **116,41 min** fora da janela e **zero** noturno. A identidade do pareamento original em cada dia é **[nao verificado]**. `app/ponto/turnos.py:725–771`, `app/escala/utils.py:2056–2069`, `app/ponto/services/cartorio.py:757–759`, `app/escala/services/leitor_celula.py:378–379,418–424`, `app/ponto/services/dia_pago.py:174–179,288–292,386–402`, `app/ponto/motor_calculo_v2.py:530–531,632–645,669–688,1760–1763`, `app/ponto/services/fechamento.py:1201–1217`, `ramo2_linhas.txt:181–182`.

### 4. Subramos verificáveis nos 332

| Dias | Mecanismo e exemplo | Regra ou defeito |
|---:|---|---|
| **295** | **Par com tempo após as 22h e recorte registrado.** Em **col877 2026-10-03**, a saída foi `23:10:41` e o fora da janela foi `70,69728 min`. A fração em segundos desse total só é compatível com recorte na saída, pois os limites efetivos ficam em minutos inteiros. Subtraído o recorte, a saída efetiva é `22:00:00`. A contribuição de cada ponta e de eventual pausa nos demais casos é **[nao verificado]**. `ramo2_linhas.txt:1–664,351–352`, `app/ponto/janela_he.py:20–30,94–103,145–153`, `app/ponto/motor_calculo_v2.py:1361–1362,2785–2792,2893–2896,3256–3260`. | O corte é a implementação da **L-097**. Dos 295, **193** têm até 5 min após as 22h, **47** têm mais de 5 até 10 e **55** passam de 10. O art. 58 §1 não explica os 55 e a saída isolada não comprova a tolerância diária nos 193. Se houve trabalho noturno devido no trecho retirado, zerar também seu adicional é defeito de apuração pelo art. 73. A CCT citada na **L-036** muda o fator no 12x36 e mantém o adicional. A **L-035** disciplina a prorrogação após as 05h. Trabalho efetivo na ponta e CCT aplicável por dia são **[nao verificado]**. `ramo2_linhas.txt:1–664`, `app/docs/LEIS.md:83–84,115`, `app/ponto/tests/test_o280_bateria_da_lei.py:3294–3299`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:4–6`. |
| **19** | **Par fechado sem duração noturna visível.** Exemplo **col769 2026-10-30**, `10:00:00–22:00:00`. `ramo2_linhas.txt:1–664,209–210`. | Zero é o resultado correto para o trecho exibido se ele termina até as 22h e não há outro trabalho noturno. As frações de segundo dos 15 casos de borda e o veredito legal da jornada inteira são **[nao verificado]**. `app/ponto/motor_calculo_v2.py:98–118`, `noturno_ramo_frota.py:82–83`. |
| **18** | **Só aparece um período aberto com zero trabalhado.** Em **col081 2026-10-03**, a entrada exibida é `23:55:15`, sem saída, mas o `DIA` soma **156,05295 min** trabalhados. Pelo predicado, precisou existir um fechado positivo cujas pontas não aparecem no anexo. `ramo2_linhas.txt:1–664,3–4`, `noturno_ramo_frota.py:92–111,132–147`. | O período aberto retorna com os campos noturnos no zero inicial. O mecanismo e o veredito legal do fechado positivo são **[nao verificado]**. `app/ponto/motor_calculo_v2.py:208–215,2750–2755,2877–2882,3215–3220`. |

Há ainda **1** lacuna de períodos dentro dos 295. Em **col889 2026-10-23**, o par mostrado tem **60 min** trabalhados e o `DIA` soma **119,43067 min**. O histograma usa o par visível. A duração noturna dos períodos omitidos é **[nao verificado]**. `ramo2_linhas.txt:415–416`, `noturno_ramo_frota.py:133–147`.

## 23/11 17:03 · sessao 0216e5a53935 · E4C-F5 · sonda · origem=Code

```text
D_ABERTAS 476
COLABS_COM_2+_D_ABERTAS 18
COLABS_COM_D_ABERTA 476
CARTOES_COM_PERGUNTA 10252 DIVERGEM_RENDER_X_JUIZ 25
P_EM_D_ABERTA_COM_DESTINO_DE_OUTRO_COLAB 0 []
```

Transcrição literal do dado raspado. Não encontrei `sonda_82.py` na cópia. Reexecução e lógica dos contadores [nao verificado].

**A comparação mede a D exibida no cartão contra a D de um helper.** `ch.disputa_obj` dá o número exibido e alimenta o estado de aplicação usado para mostrar o botão. O POST do modal envia `pergunta_id` e valida com a D da própria pergunta. O lote da tela envia `refs`. [fio.py:285](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:285) · [fio.py:375](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:375) · [modal_fio.html:123](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/chamados/partials/modal_fio.html:123) · [modal_fio.html:274](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/chamados/partials/modal_fio.html:274) · [modal_fio.html:306](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/chamados/partials/modal_fio.html:306) · [fio.py:642](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:642) · [views_cobrar.py:374](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/views_cobrar.py:374)

**O inline tem seleção própria.** A lista oferece o botão por `item.verbo == "validar"` e envia o ID do chamado. As duas consultas priorizam D própria aberta. Por destino, `_disputa_do_chamado` pega a D da P de maior PK, sem filtro de colaborador. `validar_inline` pega a D aberta mais recente do colaborador entre as que têm P apontando para o chamado. Os `25` são divergências reportadas entre render e helper. Divergências com o inline real [nao verificado]. [_lista_chamados.html:81](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/chamados/partials/_lista_chamados.html:81) · [fio.py:130](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:130) · [views_cobrar.py:160](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/views_cobrar.py:160) · [models.py:1941](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/models.py:1941)

**Exemplos do recorte**

- `26065`, `20303`, `18388`, `17239`, `18151` têm `D_render_aberta=None`. Pelo código, um cartão com P carregada recebe uma D. Se o campo impresso guarda só a D aberta, o `None` indica D fechada no render. Se era própria ou herdada da primeira P carregada [nao verificado]. [fio.py:265](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:265) · [fio.py:285](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:285)
- `16657`, `12164`, `33679`, `33678`, `33677`, `13314` têm dois IDs distintos de D aberta no recorte. O render prefere D própria e, na ausência dela, herda a D da primeira P carregada. Se cada cartão era sem D própria e tinha P das duas Ds abertas [nao verificado]. [fio.py:220](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:220) · [fio.py:265](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:265) · [fio.py:285](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/chamados/services/fio.py:285)

## 23/11 16:59 · sessao 6ef4ffdffe7b · UI-EDITAR-RAIZ · verificador · origem=Code

**UI-EDITAR-RAIZ — o diff atende à edição da raiz por inspeção.** O lápis usa o wizard da filha, e a lista relê o `apelido` salvo. A guarda protege as células reconhecidas como lavradas. Há uma ressalva no carimbo de `FechamentoMensal`. `app/templates/escala/tipos_lista.html:58` (diff), `app/escala/views_wizard.py:95-126`, `app/escala/services/cadastro_tipo.py:184-190,353-364`, `app/ponto/portas/celula.py:284-288`, `app/ponto/services/fechamento.py:1648-1656`.

Os dois arquivos locais correspondem às versões anteriores do diff. Reconstituí as versões propostas em memória e conferi os hashes dos dois blobs. As referências ao lápis novo e a `test_ui_editar_raiz.py` vêm do diff colado. HEAD `f82d53543`, execução dos cinco testes, clique no navegador e efeitos em produção [nao verificado].

| id | procede? | arquivo:linha | consequencia | cura minima |
|---|---|---|---|---|
| 1a · Mesma edição | Sim | `app/templates/escala/tipos_lista.html:58,85` (diff), `app/escala/urls.py:25-27`, `app/escala/views_wizard.py:63-92,95-109`, `app/templates/escala/wizard_tipo_escala.html:15-16` | Raiz e filha usam `wizard_tipo_editar` no GET e `wizard_salvar_editar` no POST, com o PK da própria linha. Ambos exigem login e `editar_escalas`. | Nenhuma. |
| 1b · Sistema e permissão | Sim, o lápis segue visível para quem só tem leitura | `app/templates/escala/tipos_lista.html:58,85,93-96` (diff), `app/escala/views.py:77-83`, `app/colaboradores/mixins.py:322-346`, `app/escala/views_wizard.py:63-70`, `app/escala/services/cadastro_tipo.py:144-146`, `app/core/respostas.py:63-71` | Com `ver_escalas` e sem `editar_escalas`, a pessoa vê o lápis da raiz editável e o de qualquer filha. A porta recusa com toast de erro em HTMX ou 302 para o painel no acesso comum. A raiz `editavel=False` mostra `sistema` sem lápis. A filha `editavel=False` mostra ambos e, com permissão de edição, recebe 400 no GET. O serviço também recusa a gravação. | Nenhuma para o aval. |
| 1c · Nome exibido | Sim | `app/templates/escala/wizard_tipo_escala.html:22,66-72,461-465`, `app/escala/views_wizard.py:18-23`, `app/escala/services/cadastro_tipo.py:184-190,306-307`, `app/templates/escala/tipos_lista.html:49,242-246` (cópia), `app/escala/views.py:83,114-115` | O campo visível grava `apelido`, com até 12 caracteres. A lista consulta de novo e renderiza o rótulo da raiz. `template_nome` vem do campo oculto e recebe o apelido em edição quando está vazio. | Ajustar a instrução «clique no filho para editar» para indicar o lápis em `app/templates/escala/tipos_lista.html:26`. |
| 1d · Herança | Sim | `app/escala/views_wizard.py:102-109`, `app/escala/services/cadastro_tipo.py:152-180,306-307,332-378`, `app/escala/models.py:771-775`, `app/templates/escala/tipos_lista.html:72-88` (cópia), `app/ponto/management/commands/gerar_celulas.py:85-112`, `app/ponto/portas/celula.py:225-229,284-311` | Editar preserva PK e pai. O rótulo da filha lê `f`, e o DNA lê o tipo do vínculo. A regeneração seleciona vínculos do tipo editado. No trecho histórico, uma sobreposição pode trocar a geradora de célula aberta de outro vínculo. Ocorrência em produção [nao verificado]. | Nenhuma na herança. |
| 1e · Lavra e aviso | Sim para as células guardadas, com ressalva no fechamento | `app/folha/export.py:597-628`, `app/ponto/portas/celula.py:218-221,251-254,272-288,395-400`, `app/escala/views_wizard.py:113-126`, `app/core/respostas.py:63-66`, `app/templates/escala/tipos_lista.html:242-246` (cópia), `app/escala/services/cadastro_tipo.py:353-364`, `app/ponto/services/fechamento.py:1648-1656` | A porta pula datas em janelas lavradas por TXT entregue ou holerite publicado. O wizard inclui a contagem no aviso, e o sucesso HTMX recarrega a lista imediatamente. Legibilidade do toast [nao verificado]. A invalidação histórica pode zerar `derivado_em` e atualizar `atualizado_em` de uma competência lavrada, mesmo quando sua célula foi pulada. | Se a proibição abrange o carimbo do fechamento, excluir as competências lavradas dessa invalidação usando o juiz existente. |
| 2 · Clique | Sim, abre só o drawer | `app/templates/escala/tipos_lista.html:45-46,58,64-65,85` (diff), `app/templates/escala/tipos_lista.html:129-148,226-234` (cópia) | `stopPropagation()` impede que o clique chegue ao cabeçalho e execute `toggleRaiz`. `form-raiz` é um `div` vazio e oculto, sem chamada pelo lápis. Os dois lápis usam `#wiz-drawer-body`, `innerHTML` e `abrirWizDrawer()`. | Nenhuma. |
| 3a · Redirect da cópia | Sim | `app/templates/escala/tipos_lista.html:90` (cópia), `app/escala/views.py:139-151` (diff), `app/escala/urls.py:12,34`, `app/core/respostas.py:63-71`, `app/templates/base.html:334,388`, `app/templates/core/_dialogo.html:46-54,78-87` | O botão navega via `location.href`. `resposta_acao` grava Django messages e usa `redirect_url` no 302 para a lista, que exibe a mensagem. Com `HX-Request: true`, devolve 204 com `HX-Trigger` para o toast e ignora o destino. O `reverse` é avaliado antes de chamar o helper nos dois caminhos. | Nenhuma. |
| 3b · «Edite-a.» | Sim, como instrução manual | `app/escala/views.py:146-149` (diff), `app/escala/services/cadastro_tipo.py:397-418`, `app/escala/models.py:114,479-487`, `app/templates/escala/tipos_lista.html:81-85` (diff) | A cópia é editável, fica sob o mesmo pai e tem lápis. O nome com `(copia)` vai para `template_nome` e `descricao`, enquanto o rótulo normal usa o `apelido`, que nasce vazio na cópia. A mensagem pode deixar a linha ambígua. | Indicar o `codigo` visível da cópia e a ação «use o lápis na lista». |
| 3c · Censo de URLs | Uma órfã na cópia local | `app/escala/urls.py:6-40`, `app/escala/views.py:149` (cópia), `app/config/urls.py:27`, `app/config/urls_ci.py:8` | Cruzei 26 nomes literais `escala:*` usados em reversões, redirects e tags `{% url %}` sob `app/` com os 27 nomes registrados. Só `editar_tipo` ficou sem rota. Os nomes montados pelos helpers de smoke conferidos também existem. `app/escala/tests/test_smoke_clique_wizard_preview.py:137-179`, `app/escala/tests/test_smoke_clique_folga_calendario.py:267-424`. | A troca por `lista_tipos` remove a referência órfã. |
| 4a · Tirar o bloco | Morde | `app/escala/tests/test_ui_editar_raiz.py:27-31,75-82` (diff), `app/templates/escala/tipos_lista.html:58` (cópia) | O teste falha pela ausência do `hx-get` de edição no cabeçalho da raiz. A asserção de `hx-target` pode passar pelo botão `+`, mesmo se o lápis perder o alvo. | Verificar `href`, `hx-target`, `hx-swap` e `onclick` no próprio link. |
| 4b · Tirar o `if editavel` | Morde | `app/escala/tests/test_ui_editar_raiz.py:51-54,84-88` (diff) | Se o lápis ficar incondicional, a URL de edição aparece no cabeçalho da raiz de sistema e derruba `assertNotIn`. | Nenhuma para esse mutante. |
| 4c · Voltar o reverse antigo | Morde | `app/escala/tests/test_ui_editar_raiz.py:118-125` (diff), `app/escala/views.py:145-151` (cópia), `app/escala/services/cadastro_tipo.py:397-418` | A cópia já foi criada quando o `reverse` falha. A contagem sozinha passa. `Erro ao copiar` derruba a asserção de mensagem. | Afirmar também o destino exato do 302. |
| 4d · Renomear e preservar filha | Mordem as regressões próprias | `app/escala/tests/test_ui_editar_raiz.py:34-38,90-116` (diff) | Um teste pega perda do apelido gravado ou exibido e mudança do pai da raiz. O outro pega alteração dos campos conferidos ou do HTML da filha. Os três mutantes pedidos são cobertos por 4a–4c. Os cinco casos usam superusuário e ficam sem prova de clique, recusa de RBAC e aviso de lavra. `app/escala/tests/test_ui_editar_raiz.py:43-66,75-125` (diff). | No teste da filha, confirmar também que os horários da mãe mudaram. Para o selo de front, cobrir o clique da raiz na casca admin e demonstrar o alcance pelo grafo. `CLAUDE.md:203-204`, `app/templates/escala/tipos_lista.html:1`. |
| 5a · `editavel` | Escritores e leitores localizados | `app/escala/models.py:177-180`, `app/escala/services/cadastro_tipo.py:145-146,175-178,382-385,414-418,435-446`, `app/escala/admin.py:7-15`, `app/escala/views_wizard.py:67-70`, `app/escala/views.py:160-162`, `app/templates/escala/tipos_lista.html:58,93-96` (diff) | O default é `True`. Criação pelo wizard e cópia gravam `True`. O admin grava o objeto do formulário, e a derivação carrega o valor do original. Leem o campo o lápis da raiz, o selo e a exclusão da filha, e as guardas de GET, gravação e exclusão. Origem do valor em registros específicos de produção [nao verificado]. | Nenhuma. |
| 5b · `t.pk` | Escritor e leitores localizados | `app/escala/migrations/0001_initial.py:16-20`, `app/escala/models.py:798-799`, `app/escala/services/cadastro_tipo.py:399-418,435-446`, `app/templates/escala/tipos_lista.html:43-47,58-64` (diff), `app/escala/urls.py:25-27`, `app/escala/views_wizard.py:67,102` | O `id` é `BigAutoField`, atribuído na inserção pelo ORM/banco. A cópia insere outro registro e a derivação limpa o PK antes de inserir. A lista lê o PK para IDs da linha, expansão e URLs. As portas de edição leem o `tid` e buscam por `pk`. | Nenhuma. |
| 5c · `apelido` | Escritores e leitores localizados | `app/escala/models.py:114,420-425,479-486,525-531`, `app/escala/services/cadastro_tipo.py:189-190,306-307,382-385,442-446`, `app/escala/views_wizard.py:22,226-240,263-266`, `app/templates/escala/wizard_tipo_escala.html:69`, `app/core/templatetags/hasner_filters.py:64-89,201-213`, `app/escala/views.py:228`, `app/colaboradores/services/ficha.py:67-83`, `app/relatorios/views.py:865` | O wizard escreve pelo serviço, o admin salva pelo formulário, a derivação zera o campo e o preview o altera só em memória. Leem o campo o preenchimento do wizard, os rótulos canônicos, o dropdown e sua busca, a ficha do vínculo e o relatório. | Nenhuma. |
| 5d · `except` novo | **0** | `app/escala/views.py:145-151` (cópia e diff) | O `except Exception` da cópia já existia. | Nenhuma. |

## 23/11 16:49 · sessao d292997b399a · E4C-F5 · verificador · origem=Code

**E4C-F5, volta 7 — REPROVADA na revisão estática.** O inline passou a consultar o juiz registrado, mas pode validar uma disputa diferente da identificada no cartão. O `--apply` do CLI também passou a manter as travas da validação durante a varredura da disputa, que pode adquirir outro chamado depois da primeira trava de chamado. `app/chamados/services/fio.py:267–294`, `app/chamados/views_cobrar.py:214–222`, `app/chamados/management/commands/validar_classe_a.py:105–117`, `app/chamados/services/validacao.py:813–830`.

Execução da suíte, dos mutantes e ocorrência em produção [nao verificado]. Nas tabelas, os caminhos partem de `app/chamados/` e **V7** é `tests/test_f5_v7_guarda_sob_trava.py:1`.

### Os oito bloqueantes

| id | procede? | dentro-dos-8 ou fora | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|---|
| 1-corrigir | **Sim** | dentro-dos-8 | `services/acoes_disputa.py:440–453`, `services/validacao.py:742–754`, V7:49–79 | O segundo ato recebe o cartão original e o confere após a trava e o refresh. O RED reaponta antes de entrar no validador e exige **1 RESPOSTA e 0 VALIDACAO**. O controle exige 1 VALIDACAO. | Manter a guarda e o RED. |
| 8.3 | **Sim** | dentro-dos-8 | `views.py:493–502`, `services/acoes_disputa.py:20–24`, `services/validacao.py:742–754`, V7:98–126 | O detalhe transmite o PK do cartão. O validador relê cartão e D aberta. Os dois REDs exigem **0 VALIDACAO** após reapontar ou fechar a D. O controle exige 1. | Manter as duas guardas e os dois REDs. |
| 1-recusar | **Sim** | dentro-dos-8 | `views.py:511–519`, `services/fio.py:696–702`, `services/validacao.py:974–1000`, V7:134–154 | Com cartão esperado, a D fechada devolve `nao_encontrada` antes de gravar motivo ou reabrir a P. O RED exige resposta original preservada, `recusa_motivo` vazio e **0 RECUSA**. | Manter a guarda sob C. |
| 2/8.1 | **Parcial** | dentro-dos-8 | `services/materializacao.py:2165–2205,2222–2231,2269–2278`, `management/commands/backfill_fio_morto_e4b.py:192–251`, V7:277–299 | O conjunto de P prontas e a expansão para candidatas à irmã e Ch relacionados têm fonte comum. O RED despacha `_executar_dia` real e pega o Ch relacionado terminal adquirido tarde se a expansão sair. Ainda há sobreposição na enumeração das D sementes e de pai/destino. O RED cobre um relacionado da própria semente. | Compartilhar a seleção das D sementes entre planejamento e despacho. Acrescentar ao RED outra P pronta da mesma D e uma candidata à irmã com Ch fora do universo padrão. |
| 10 | **Sim** | dentro-dos-8 | `management/commands/backfill_fio_morto_e4b.py:378–386,475–491`, V7:307–317 | O replan recusado grava `recusa:...`, incrementa `erros` e desfaz o dia. O RED injeta a recusa e exige **`CommandError` e `FIM E4B rc=1`**. Remover o incremento deixa esse caso sem a exceção esperada. | Manter o incremento no ramo. |
| RED-aplicar | **Sim** | dentro-dos-8 | `services/declaracao_texto.py:723–740`, `services/validacao.py:745–762`, V7:203–231 | O RED troca a resposta pela porta real entre os dois atos e exige **`resposta_mudou` e 0 VALIDACAO**. O controle valida a resposta aplicada. O delta deste item é de teste. | Manter a asserção do estado e da trilha. |
| 8.2 | **Parcial** | dentro-dos-8 | `../docs/JUIZES.md:43`, `services/fio.py:130–137,267–294`, `views_cobrar.py:160–168,214–222`, V7:164–185 | O seletor próprio saiu e o juiz registrado é chamado nas duas leituras. O RED distingue a eleição por PK da eleição por `aberta_em`, mas observa a chamada à casca mockada. As duas D da fixture estão sem chamado-pai, condição que as exclui do render do fio. `services/fio.py:216–219`. | Selar a concordância com o cartão renderizado e com a VALIDACAO gravada. Se o clique deve atuar sobre uma D inteira, fixar esse universo no desenho e identificar a D no clique. |
| CLI-validar_classe_a | **Parcial** | dentro-dos-8 | `management/commands/validar_classe_a.py:62–70,105–118`, `services/validacao.py:755–765`, V7:241–268 | Relê e reclassifica sob C. O RED troca `07:13` por `07:41`, usa um classificador mockado e exige **0 VALIDACAO**. A guarda preexistente ato × projeção fica só na seleção. Uma divergência introduzida no intervalo ainda pode passar se a projeção continuar classe A, pois o juiz lê o texto da coluna. `models.py:4449–4457`, `juizes.py:2131–2149`. | Reler também a guarda ato × projeção sob C e testar a divergência introduzida entre seleção e apply. Cobrir “Não estava” pelo classificador real. |

### Inventário, tela e classes de reprovação

| id | procede? | dentro-dos-8 ou fora | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|---|
| 2/8.1-inventário | **Sim, com sobreposição** | dentro-dos-8 | `services/materializacao.py:2165–2205,2222–2231,2269–2278`, `management/commands/backfill_fio_morto_e4b.py:153–156,202–220,235–251`, `services/passivo_validado.py:393–399` | A expansão de P prontas, irmãs e relacionados tem uma fonte. Plantio, backfill e lote do passivo chegam à expansão comum. O backfill ainda enumera D/pai/destino das sementes no inventário genérico e resolve as D de `materializar` de novo no despacho. Aquisição inicial e replan usam o mesmo `_universo_dos_pids`. | Compartilhar a resolução das D sementes e manter explícito que o backfill ainda precisa da D e do pai para a cauda de fechamento, inclusive quando faltam P prontas. `services/materializacao.py:2125–2158,2293–2294`. |
| Ordem-materializar | **Sim, corrigida no caso coberto** | dentro-dos-8 | `models.py:2938–2965`, `management/commands/backfill_fio_morto_e4b.py:216–241`, V7:277–299 | A primitiva segue **C → D → Ch → P**. O delta antecipa D/Ch explícitos na aquisição do backfill. O inventário compartilhado inclui o Ch relacionado terminal no primeiro conjunto de Ch. | Manter o RED com a porta real. |
| Ordem-perguntar | **Sim, furo preexistente** | fora | `management/commands/backfill_fio_morto_e4b.py:101–105,151–156,235–251`, `services/tarefa_do_dia.py:260–267,325–336`, `services/fabrica_da_celula.py:145–171,217–219`, `services/relavra_via.py:171–183,229–241` | Com Ch já travado, uma P materializada e retratada numa D fechada pode ficar fora da identidade do fato e o dia pedir `perguntar`. A consulta ampla do escritor ainda recebe essa P e tenta travar a **D fechada depois de Ch**, antes de descartá-la pelo predicado de elegibilidade. | Usar o predicado canônico de elegibilidade na coleta anterior à trava e selar a ordem pelo ramo `perguntar` real. |
| 8.2-tela / segundo juiz | **Sim** | dentro-dos-8 | `services/fio.py:130–137,267–294,634–648`, `../templates/chamados/partials/modal_fio.html:123–125,158`, `../templates/chamados/partials/_lista_chamados.html:81–82`, `views_cobrar.py:163–168,206–222` | Com duas D abertas do mesmo colaborador e sem D própria do cartão, o modal pode identificar **D1**, da primeira P por PK, e o inline escolher **D2**, da maior PK. O botão envia só `cid`. O seletor também pode eleger D2 por uma P muda ou morta e deixar a resposta de D1 sem ação. Eleita D2, o inline percorre as P vivas respondidas de D2 inteira, inclusive de outro cartão. | Fazer a tela e o POST partilharem o universo acionável e reconferir as P desse universo sob C. |
| 8.2-dono | **Sim, para linha legada incompatível** | dentro-dos-8 | `services/fio.py:134–137,216–219,1254–1265`, `views_cobrar.py:155–168,210–222`, `utils.py:75–83`, `models.py:3110–3113` | A busca por destino perdeu o filtro de colaborador do seletor v6. Se existir uma P legada apontada a cartão de outro colaborador, o inline pode agir na D alheia após conferir a área do cartão. A porta de nascimento atual rejeita esse vínculo. Presença desse dado em produção [nao verificado]. | Conferir o dono no universo compartilhado pelo cartão e pela ação. |
| CLI-fronteira / ordem | **Sim, efeito novo** | fora | `management/commands/validar_classe_a.py:105–117`, `services/validacao.py:742–745,813–830`, `services/materializacao.py:2168–2169,2269–2278`, `../ponto/services/fechamento.py:1798–1824` | O `atomic` novo abrange também a varredura da D. Outra P pronta da mesma D com Ch relacionado terminal pode adquirir esse Ch após Ch/P da primeira P, dentro da mesma transação. A fronteira também passa a adiar callbacks de commit e permite que uma exceção na varredura desfaça a validação inicial. | Fazer a guarda do CLI no atomic inicial da porta canônica e preservar a fronteira da varredura posterior. Selar a ordem com duas P prontas. |
| ALLOWLIST / carimbo cru | **Sim, muda o selo. Não há predicado cru adicional.** | dentro-dos-8 | `tests/test_contract_leitores_vitalidade.py:134,152–153,348–364`, `services/materializacao.py:2222–2231,2265–2269,2285–2288`, `management/commands/backfill_fio_morto_e4b.py:216–219`, `juizes.py:1661–1676` | O scanner dispensa os dois `__isnull` movidos para o helper. O backfill é um consumidor novo desse filtro de elegibilidade. As duas faces de resposta seguem nomeadas em L-130. A dispensa é por função e também cobriria futuras comparações cruas acrescentadas nela. | Descrever a entrada como elegibilidade compartilhada. Se o selo tiver de acusar novas leituras no helper, fixar os dois campos e suas ocorrências esperadas. |
| RED-guarda-sob-trava | **Parcial** | dentro-dos-8 | V7:49–74,98–122,139–154,203–226, `tests/test_f5_v5_guarda_sob_trava.py:161–170`, `services/materializacao.py:2165–2169,2207–2217` | Os REDs de corrigir, detalhe, recusar e aplicar intercalam antes de entrar na porta testada. Pegam a retirada da guarda ou do argumento. Uma guarda movida para antes da trava pode passar. O a3 da v5 cobre a escrita da **correção**, antes da segunda porta. | Interpor na aquisição da trava da porta testada. No reapontamento, escolher um destino já presente no conjunto de Ch para a reconferência do plantio deixar a guarda do cartão decidir. |
| Segundo escritor / relógio / `except` | **Não no delta. `except` novos: 0.** | fora | `services/validacao.py:164–178,742–762,974–1000`, `services/acoes_disputa.py:24–25,452–454`, `management/commands/backfill_fio_morto_e4b.py:204–208,378–410`, `services/disputa_emissao.py:1911–1921`, `models.py:1595–1599`, `juizes.py:633–643,1563–1564`, V7:1–318 | As escritas seguem pelas portas existentes. O relógio implícito de 8.4 continua alcançável por `perguntas_a_ressuscitar` → `pergunta_viva(_q)` sem `hoje`. Os `except` visíveis nos hunks são contexto do delta. | Nenhuma cura de escritor ou `except` nesta volta. Manter 8.4 na fila. |

### Quem fornece e quem lê

| id | procede? | dentro-dos-8 ou fora | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|---|
| `cartao_esperado` | **Sim, PK com dois efeitos** | dentro-dos-8 | **Fornecem** `views.py:502,519,537`, `services/fio.py:702–703`. **Repassam** `services/acoes_disputa.py:24,96,453`. **Leem** `services/acoes_disputa.py:401,424`, `services/validacao.py:747–754,980–988`. | O valor é o PK do cartão. Sua presença exige simultaneamente que a P continue listada nele e que a D esteja aberta. A validação individual do fio tem seu próprio contrato para D fechada e omite o argumento. `services/fio.py:649–673`. | Documentar os dois efeitos da presença do argumento. |
| `resposta_esperada` | **Sim, um significado** | dentro-dos-8 | **Fornecem** `services/acoes_disputa.py:376,440–453`, `services/declaracao_texto.py:729–739`. **Lê** `services/validacao.py:755–762`. **Expõem a recusa** `services/acoes_disputa.py:461–463`, `services/declaracao_texto.py:740`, `views.py:478–481`. | É o texto gravado ou aceito no ato anterior. A comparação usa o texto relido com `strip()`. Uma nova escrita com o mesmo texto satisfaz a guarda. | Manter o contrato de igualdade textual. |
| `nao_encontrada` | **Sim, duas causas com a mesma resposta** | dentro-dos-8 | **Produzem** `services/validacao.py:726–733,753–754,938–945,987–988` e, na conferência, `:1057–1059`. **Leem** `services/acoes_disputa.py:33–35,97–99,461–463`, `services/declaracao_texto.py:740`, `management/commands/validar_classe_a.py:118`. | Busca inválida ou sem resultado e D fechada sob a trava convergem para a mesma recusa no validar/recusar com cartão. O fechamento ganha a resposta pública “Pergunta inválida”, conforme o escopo. | Preservar a equivalência pública e testar o estado retornado nas duas portas. |
| `erros` do backfill | **Sim, um contador** | dentro-dos-8 | **Escrevem** `management/commands/backfill_fio_morto_e4b.py:330,379–383,406–410`. **Leem** `:489–491`. O relatório lê `p['res']` em `:446–450,475–479`. A lista de erros do materializador sai de `services/materializacao.py:2296–2301` e a chamada em `backfill_fio_morto_e4b.py:153–156` descarta esse retorno. | O contador reúne exceções por dia e agora a recusa do replan. Ele governa o RC e o `CommandError`. `p['res']` dá o motivo no relatório. A lista de erros do materializador é outro retorno, preexistente. | Manter o incremento no ramo de replan e o RED do RC. |

## 23/11 16:45 · sessao 4200cb8374ce · PISTA-2-CERTIFICA · verificador · origem=Code

**REPROVA a volta 1.** Um run com `--parallel 9` pode receber `CERTIFICA` e `rc 0`. A leitura de `AMBIENTE` também aceita uma segunda linha indentada e pode usar um digest emitido por um `grep` que termina com erro. `bin/pista_certifica.sh:36–40`, `bin/pista_certifica.sh:119–121`, `bin/pista_certifica.sh:140–175`.

As referências usam a numeração após o diff anexado. Conferi os casos do certificador e seus mutantes em memória, com entradas sintéticas e dublês. O hash Git da cópia, a execução integral dos selos e o comportamento em produção ficam [nao verificado].

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| 1a · Certificar sem comparar | Não, no fluxo local analisado | `bin/pista_certifica.sh:23–24`, `bin/pista_certifica.sh:36–40`, `bin/pista_certifica.sh:163–175`, `bin/recursos.sh:154–162` | Sem o juiz, sai `2`. Sem medição da VM, inclusive quando o Docker imprime e falha, registra falha. Linha canônica ausente, vazia, `ausente` ou duplicada também impede `rc 0`. Com os dois valores presentes, o teste de desigualdade é executado e divergência impede `CERTIFICA`. | Manter as guardas. |
| 1b · Linha repetida e erro de leitura | Sim | `bin/pista_certifica.sh:36–40`, `bin/pista_certifica.sh:119–121`, `bin/pista_certifica.sh:167–175` | Uma linha canônica igual à VM e uma segunda ` AMBIENTE imagem_camadas=ausente` dentro da janela dão `rc 0`. A janela reconhece o espaço inicial, mas `amb` ignora a segunda linha. `amb` também lê duas vezes, ignora o rc da contagem e usa `grep \| sed` sem `pipefail`. No caso simulado, o `grep` emitiu o digest e retornou `2`, e o run certificou. Nos dois casos, houve comparação do valor extraído. | Em uma única leitura com rc verificado, exigir exatamente uma ocorrência da chave e a forma canônica da linha. |
| 2a · Bytes e prefixo | Sim, para JSON aceito com exatamente um LF final | `bin/recursos.sh:147–162`, `bin/tests/test_pista_certifica.sh:40–44`, `bin/tests/test_pista_certifica.sh:354–363` | `$(...)` remove os LFs finais e `<<<` repõe um. Com um LF na saída original, o hash é o mesmo do pipe cru. Com zero ou dois LFs, os bytes divergem. O valor comparado é `sha256:` seguido dos 64 hex do hash. A saída literal de `sha256sum` inclui também o marcador `  -`. | No lançador, produzir o digest completo no formato `sha256:<64 hex>` e verificar o rc do `inspect`. |
| 2b · Referência e falha no lançador remoto | [nao verificado] | `bin/suite.sh:351–369`, `bin/pista_certifica.sh:163`, `bin/recursos.sh:143–151`, `app/docs/BACKLOG.md:1994` | A suite da VM resolve `:latest` para `.Id`, mede as camadas por esse Id e roda pelo mesmo Id. O certificador mede `:latest` da VM no ato. O comentário sobre `imagem=5397516d7fb4` não prova qual ref o remoto mediu ou rodou, nem o rc do `inspect`. Se o remoto usar o pipe cru sem `pipefail`, um `inspect` que emita a lista inteira e falhe ainda pode fornecer um digest igual ao da VM. | No remoto, fixar o Id que vai rodar, medir esse Id e só lançar após confirmar o rc da medição. |
| 3 · Escritores, leitores e juiz | Não apareceu segundo juiz na cópia com o diff | `bin/suite.sh:353–369`, `app/core/tests/test_ambiente_do_run.py:50–51`, `bin/pista_certifica.sh:163–175`, `bin/tests/test_pista_certifica.sh:54–55` | `suite.sh` passa `HASNER_IMAGEM_CAMADAS` e `HASNER_IMAGEM_ID`. O teste de ambiente lê as duas e escreve `AMBIENTE imagem_camadas=` e `AMBIENTE imagem_id=`. O certificador lê `imagem_camadas` e compara com `camadas_da_imagem`. `imagem_id` fica como registro. O outro escritor encontrado gera fixtures do selo. Escrita e leitores no remoto [nao verificado]. | Preservar o teste de ambiente como escritor do log do run e a comparação de camadas como decisão de identidade. |
| 4 · Teto de 8 | **Sim, bloqueante** | `bin/pista_certifica.sh:137–153`, `bin/pista_certifica.sh:173–175`, `bin/suite.sh:162–173`, `bin/suite.sh:371` | `--parallel 9`, `--parallel=9`, `auto` e opções repetidas passam pelo parser. Com os demais dados válidos, os casos em memória deram `rc 0`. Na VM, `suite.sh --seco` também mostrou `--parallel 9` chegando ao argv. | Validar as duas grafias no parser do certificador. Exigir valor explícito de 1 a 8 quando a opção for usada, e resolver `auto` antes de lançar. No lançador, aplicar o teto antes de iniciar o job. |
| 5 · `exit 3` em run legítimo | Sim | `bin/suite.sh:353–364`, `bin/suite.sh:369`, `bin/recursos.sh:154–162`, `bin/trava_teste.sh:35–42` | Mesmo com Id válido, o segundo `inspect` pode falhar ou estourar seu próprio prazo de 30 s. A suite sai `3` **antes** de chamar a trava e antes do `docker run`. O Id fixado protege a identidade da imagem durante a espera. Incidência na VM [nao verificado]. | Manter a recusa sem digest e distinguir timeout, erro de `inspect` e formato inválido no diagnóstico. |
| 6a · Selo / RED | Os mutantes de certificação mordem. Falta selo do teto | `bin/tests/test_pista_certifica.sh:97–112`, `bin/tests/test_pista_certifica.sh:133–138`, `bin/tests/test_pista_certifica.sh:267–281`, `bin/tests/test_suite_motivo.sh:38`, `bin/tests/test_suite_motivo.sh:177–182`, `bin/tests/test_suite_motivo.sh:245–256` | Em memória, `mut1`, `mut1b` e `mut2` passaram de `rc 1` no original para `rc 0` com, respectivamente, linha ausente, camadas divergentes e Postgres divergente. No selo da suite, os três novos mutantes têm observáveis próprios no código: env ausente, consulta por `:latest` recusada pelo dublê e chegada indevida à trava. O selo de argv cobre `--parallel x`, mas deixa passar `9`. | Selar `8` aceito e `9` recusado nas duas grafias, com mutante que retire o teto. Cobrir também a duplicata indentada e o erro de leitura de 1b. |
| 6b · Relógio e erro engolido | Não para relógio de negócio novo. Sim para o erro de leitura de 1b | `app/docs/PLANO-PADROES.md:97–103`, `bin/pista_certifica.sh:54–67`, `bin/pista_certifica.sh:36–40`, `bin/recursos.sh:154–162`, `bin/suite.sh:358–359`, `bin/tests/test_pista_certifica.sh:268` | As datas vêm da linha de fim. O novo caminho usa prazo relativo para o Docker. O rc do juiz aciona a recusa na suite e no certificador. O `|| true` novo está na geração do mutante. A perda de rc relevante está em `amb`. | Corrigir `amb` conforme 1b. |

## 23/11 16:41 · sessao 111adbda65de · VIGIA-DO-TMUX · verificador · origem=Code

**Reprova na volta 3.** O vigia e o Stop compartilham `_julga_clear`, mas o vigia pode enviar `/clear` com o transcript de outra sessão. Também pode enviar quando a trava do orçamento impede o Stop de chegar ao CLEAR. O refactor do B4 muda o comportamento do Stop para um estado JSON que o leitor aceita. `bin/vigia_tmux.py:319–337,380–389` · `bin/hook_stop_fila1.py:1029–1044,1071–1084,1427–1466` · `bin/disjuntor.py:355–364,1431–1442`

Conferi os quatro blobs da cópia contra os hashes novos do `diff.txt`, a sintaxe do selo com `bash -n` e os contraexemplos de parser, B4 e retomada com funções isoladas em memória. A identidade dos commits, o selo integral, os REDs dos mutantes e o estado de produção ficam **[nao verificado]**. O selo cria e remove arquivos, por isso a conferência foi somente leitura. `bin/tests/test_vigia_tmux.sh:37–40,179–185` · `bin/vigia_tmux.py:219–256,340–342,374–396` · `bin/disjuntor.py:1426–1442`

### Achados da volta 2

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| P1 | **Sim**, no caso apontado | `bin/vigia_tmux.py:250–251,269–297` · `bin/tests/test_vigia_tmux.sh:241–258,305–307,343–352` | Sai um separador do prompt e sobra qualquer branco adicional. `je` exige só o literal, sem Enter, e o motivo da falha. `a` e `c4` continuam cobrando painel fixo, marca antes da segunda captura e remoção da marca no adiamento normal. | — |
| P2 | **Parcial** | `bin/vigia_tmux.py:219–256` · `bin/tests/test_vigia_tmux.sh:150–164,390–392` | `c20` morde `─ detalhe` e `c20b` morde bordas de larguras distintas. Troque `─ detalhe` por uma linha de traços idêntica à borda. O parser escolhe essa linha como borda superior, descarta `❯ texto` e devolve caixa vazia. Conferido em memória. | Recusar o recorte ambíguo com bordas idênticas e pôr essa tela no selo. |
| P3 | **Parcial** | `bin/vigia_tmux.py:175–205` · `app/core/esteira_vigia.py:846–862,895–912` · `bin/tests/test_vigia_tmux.sh:290–303,393–397` | `r3` morde a queda após publicar, pois a linha preexistente basta para impedir duplicação. A transação usa a trava certa. A busca por substring em todo o RELATO também aceita uma citação fora de ALARMES como publicação. Uma linha arquivada sai da deduplicação. | Comparar o identificador em registros de ALARMES e definir se a unicidade inclui o arquivo histórico. |
| P4 | **Parcial** | `bin/hook_stop_fila1.py:972–1045,1157–1208,1441–1466` · `bin/tests/test_vigia_tmux.sh:455–493` | O núcleo do CLEAR é comum. `p1–p13` comparam a função com o Stop real e com o valor esperado, passando o mesmo caminho de transcript aos dois. Isso deixa de fora justamente a escolha do transcript pelo vigia e a obtenção das travas pelo Stop. | Compartilhar a obtenção da decisão aplicável e exercitar o vigia inteiro com as divergências abaixo. |
| P5 | **Sim**, para a releitura pedida | `bin/vigia_tmux.py:277–292,406–415` · `bin/tests/test_vigia_tmux.sh:331–334,398–423` | `c21` cria `PAUSADO` no `flock` e falha se o topo for lido. `c15/c16` cobram as outras duas leituras. No CLEAR, a revalidação do Stop ocorre depois da última leitura da pausa e pode alongar o intervalo até a tecla. | Para fechar esse intervalo, reler `PAUSADO` depois da guarda, junto de `send-keys`. |
| P6 | **Parcial** | `bin/tests/test_vigia_tmux.sh:23–33,226–230,250–258,289–297,335–340,500–508` | O selo exige o SHA esperado e as linhas completas. `a` e `c17` têm asserções que mordem marca tardia e remoção do `flock`. O hash compara só linhas `VIGIA-TMUX` e trata falhas de leitura do `grep` como ausência de linhas. RED executado dos mutantes **[nao verificado]**. | Medir o arquivo inteiro em fixtures isoladas, falhar em erro de leitura e registrar os dois REDs. |
| P7 | **Sim**, preservado | `bin/vigia_tmux.py:90–131` · `bin/tests/test_vigia_tmux.sh:212–225,341–342` | `c18` exige `ValueError` no log, rc 0, saída vazia e zero teclas. Mover o handler para depois do parse de `TETO_CLEAR` quebra essas asserções. | — |
| C1 | **Sim** | `bin/hook_stop_fila1.py:986–987,1029–1030,1041–1045,1209–1255` · `bin/tests/test_vigia_tmux.sh:477–478` | `p4/p5` cobram disparo encerrado e ausente. O vigia adia e o Stop fica fora de `CLEAR-RAPIDO`, podendo armar um novo disparo pelos ramos seguintes. | Acrescentar ao selo o arquivo de estado ausente, que tem retorno próprio no vigia. |
| C2 | **Parcial** | `bin/hook_stop_fila1.py:993–998,1014–1026,1093–1094,1193–1208` · `bin/tests/test_vigia_tmux.sh:460–476` | `p2/p3` mordem dentro e fora da janela. A igualdade no limite libera. Cada processo amostra seu próprio `agora`, e o vigia o amostra antes do checador do RELATO. O selo não cobra a borda dos 600 s. | Cobrar igualdade e expiração na fronteira com relógio controlado. Fixar a validade até a tecla. |
| C3 | **Parcial** | `bin/vigia_tmux.py:319–337,380–387` · `bin/hook_stop_fila1.py:952–962,1427–1433` · `bin/tests/test_vigia_tmux.sh:353–361,479` | `c19`, `id`, `id2` e `p6` mordem agente pendente no arquivo escolhido e ausência de identidade. Um transcript existente do último marco ainda pode pertencer à sessão errada. | Vincular o transcript à sessão ativa do `%N` e testar marco de A com painel e Stop de B. |
| C4 | **Sim**, para as duas entradas apontadas | `bin/hook_stop_fila1.py:972–977,1017–1018,1035–1038,1058–1070,1102–1140` · `bin/tests/test_vigia_tmux.sh:480–481` | `p7/p8` mordem ciclo trocado e `ORCAMENTO_CONTEXTO=0` no mesmo ambiente. O estado `desligado` persistido tem uma divergência conservadora descrita abaixo. | Cobrir também o religamento e o marco ilegível no selo. |
| C5 | **Parcial** | `bin/hook_stop_fila1.py:1017–1028,1283–1313,1441–1466` · `bin/tests/test_vigia_tmux.sh:482–487` | `p9–p11` mordem falta de prova, primeiro `siga` do B4 e `lembrete` que deixa o CLEAR seguir. O leitor externo reproduz a sequência, com ordem e tratamento de falhas próprios. | Compartilhar a decisão anterior ao CLEAR sob a mesma política de leitura e falhas. Testar B4 degradado e troca de estado. |
| C6 | **Parcial** | `bin/vigia_tmux.py:271–292,360–401` · `bin/tests/test_vigia_tmux.sh:67–70,95–96,262–272,362–372` | `c6a/b` mordem a remoção das revalidações. `c6c` sonda a sombra na terceira captura, e `b` exige que esteja livre na retomada. Um mutante que solte depois da captura e antes do Enter ainda pode passar. Um que segure durante a publicação e a espera, soltando só antes da retomada, também. | Sondar a trava no literal, no Enter, na publicação e no primeiro poll do marco. |

### Onde o vigia e o Stop divergem

Para o mesmo disparo, a mesma hora e as mesmas leituras de handoff e guarda, `_julga_clear` dá o mesmo resultado aos dois. A implicação **“o vigia digitou `/clear`, então o Stop liberaria” é falsa**. `CLEAR-RAPIDO` é uma autorização específica. O Stop também tem liberação por COTA, pelo par quando o orçamento falha, e por `AVISO`. `bin/hook_stop_fila1.py:980–998,1041–1045,1159–1208,1267–1280,1449–1466,1497–1506`

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| Divergência · transcript antigo | **Sim, vigia envia onde o Stop bloqueia** | `.claude/settings.json:34–61` · `bin/vigia_tmux.py:319–337,380–387` · `bin/disjuntor.py:462–478,1364–1373` · `bin/hook_stop_fila1.py:1178–1192,1427–1466` | Marco de A, transcript de A legível e limpo, painel na sessão B aberta depois por `startup`/`resume`. Com disparo ativo e handoff válido, o vigia pode enviar. Com agente pendente no transcript B, o Stop grava `CLEAR-ADIADO` e devolve `siga` na primeira ocorrência. Com `escada` só em B e B4 desarmado, o primeiro `siga` do B4 também bloqueia. | Provar a identidade ativa do painel em cada autorização. Adiar se a identidade não puder ser provada. |
| Divergência · trava do orçamento | **Sim, vigia pode enviar sem autorização explícita do Stop** | `bin/hook_stop_fila1.py:1029–1044,1071–1084,1266–1280,1460–1466,1497–1506` | Com JSON válido, disparo ativo e `.lock` ocupada por todas as tentativas, o vigia lê e autoriza. O Stop registra `OCUPADO` e segue pelo outro caminho. Se a trava for inválida, há fallback pelo par, que também pode recusar a autorização explícita, por exemplo com handoff de 11 min ainda mais novo que o disparo. | Dar ao vigia a mesma condição de acesso ao orçamento e cobrar trava ocupada e inválida. |
| Divergência · tempo e mudanças entre leituras | **Sim, pode bloquear de fato no Stop** | `bin/hook_stop_fila1.py:996–998,1014–1044,1093–1094,1202–1208` · `bin/vigia_tmux.py:277–292,380–389` | Se a última autorização do vigia amostrar `agora` em `desde + 599,9 s` e o Stop amostrar após `desde + 600 s`, o vigia pode pressionar Enter e o Stop devolver `siga` por expiração. Mudanças de prova, topo, despacho, B4 ou transcript após sua última leitura também podem invalidar o veredito. A trava da sombra protege contra o `LOCK_EX` da sombra. | Vincular a autorização à linha, à sessão e à validade observadas na fronteira de envio, com coordenação dos estados que podem invalidá-la. |
| Divergência · topo e ambiente | **Sim, conforme a entrada** | `bin/vigia_tmux.py:370–372,380–387,415–423` · `bin/hook_stop_fila1.py:1017–1028,1039–1044,1058–1070,1441–1460` | O vigia verifica `NADA_RODA` só na linha inicial. Se ela for substituída por outro `PAREI: ! CLEAR`, a revalidação pode autorizar o novo topo sem conferir o literal ou o SHA originais. Com ambientes diferentes, `ORCAMENTO_CONTEXTO=0` só no Stop impede seu CLEAR enquanto o vigia pode enviar. Com o corte só no vigia, ocorre o inverso. Ambiente efetivo **[nao verificado]**. | Revalidar a mesma linha e usar uma fonte comum para o corte. |
| Divergência · `desligado` persistido | **Sim, mais restritivo no vigia** | `bin/hook_stop_fila1.py:1035–1036,1102–1110,1193–1201` | Sem o corte no ambiente e com um disparo válido do mesmo ciclo, o Stop religa e pode gravar `LIGADO` e liberar CLEAR. O vigia adia. | Selar a assimetria. |
| Divergência · B4 libera por COTA | **Sim, mais restritivo no vigia** | `bin/disjuntor.py:147,1435–1442` · `bin/hook_stop_fila1.py:1023–1028,1449–1453` | Com `PAREI: ! COTA` e handoff suficiente o Stop libera por COTA. `autoriza_clear` recusa. As formas COTA e CLEAR são distintas, e o vigia só despacha CLEAR pelo seu reconhecedor. `bin/hook_stop_fila1.py:879–893` · `bin/vigia_tmux.py:419–423` | Selar o ramo `libera` do B4 e o motivo da recusa. |
| Divergência · estado ou identidade ilegível/ausente | **Sim, mais restritivo no vigia**, ressalvada a sessão errada acima | `bin/vigia_tmux.py:134–141,319–337` · `bin/hook_stop_fila1.py:1015–1016,1023–1038,1095–1101,1209–1255,1267–1280,1283–1313` | Orçamento ausente, JSON inválido ou não objeto faz o vigia adiar ou levantar e o Stop partir de `{}`, sem CLEAR imediato. Se o orçamento falhar fora dessa leitura, o Stop ainda pode liberar pelo par. Estado B4 ilegível faz o vigia levantar, enquanto o Stop contém a falha e pode liberar CLEAR. Sem transcript identificado o vigia adia, enquanto o Stop admite a guarda sem transcript. Um marco ilegível preserva o ciclo no Stop, mas o leitor bruto do vigia pode abortar antes de autorizar. | Selar cada saída conservadora e usar a mesma leitura de marco para a mesma decisão. |

### O que mudou no Stop

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| Stop · seleção do ciclo | **Não**, para os valores retornados pelo leitor | `diff.txt`, hunk antigo `bin/hook_stop_fila1.py:1051–1060` · `bin/hook_stop_fila1.py:972–977,1111–1140` | Marco válido fornece `ref`. Ausência fornece `None`. Marco existente e ilegível conserva `marco_ref` e mantém a mesma sinalização. O novo código chama `lexists` também com marco válido. | — |
| Stop · ramo CLEAR | **Não**, para o estado JSON e as guardas usuais | `diff.txt`, hunks antigos `bin/hook_stop_fila1.py:1099–1146` · `bin/hook_stop_fila1.py:980–998,1157–1212` | Continua na ordem disparo ativo → handoff → guarda → janela. Handoff velho marca `sem_handoff_visto`. Guarda atualiza `handoff_min`, limpa `clear_parei_desde` e registra o motivo como antes. Primeira liberação grava o instante, liberação na janela salva, expiração grava `desfecho='seguiu'`. A leitura de `desde` passa para dentro do juiz e é repetida no chamador. | — |
| Stop · B4 com estado bem formado | **Não**, nos resultados esperados | `diff.txt`, hunk antigo `bin/disjuntor.py:1412–1435` · `bin/disjuntor.py:1410–1442` | Sem escada apaga `b4_para`. Com DIFF mantém. B4 novo grava `desde` e anuncia. O topo COTA com handoff suficiente libera, e os outros caminhos devolvem `siga` novo ou `lembrete` existente. O anúncio agora ocorre depois de calcular o veredito, ainda antes da gravação do JSON. | — |
| Stop · B4 com estado aceito pelo leitor, mas incompleto | **Sim**, muda veredito, gravação e trilha | `diff.txt`, hunk antigo `bin/disjuntor.py:1412–1435` · `bin/disjuntor.py:355–377,1414–1442` · `bin/hook_stop_fila1.py:1308–1313,1452–1460` | Contraexemplo conferido em memória. Escada ativa, sombra livre, topo CLEAR, `b4_para={"outro":1}`. Antes, o topo fora de COTA curto-circuitava `b4p['desde']`, devolvia `lembrete` e `_com_estado` regravava o JSON. Agora o acesso antecipado dá `KeyError`, impede essa regravação e registra `FALHA` no Stop. Um `b4_para` string verdadeiro dá `TypeError` pelo mesmo motivo. | Ler `desde` só dentro do ramo COTA. Cobrar o comportamento antigo com estado incompleto. |

### Estados, donos e sentidos

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| Estado · RELATO | **Sim, múltiplos escritores** | `bin/hook_stop_fila1.py:408–416,462–483,1412–1424` · `bin/vigia_tmux.py:175–205,415–423` · `app/core/esteira_vigia.py:834–862,865–927` · `bin/disjuntor.py:290–322` · `bin/gerar_cortes.py:117–136` · `bin/deploy_agendado.sh:31–44` | Stop e vigia leem o comando do topo. O vigia lê e escreve a trilha de ALARMES pela transação da esteira. O disjuntor publica pela porta. `gerar_cortes` e `deploy_agendado` leem e regravam por fora e podem perder um alarme concorrente. | Usar a mesma transação para as regravações. |
| Estado · `sombra.lock` | **Sim, dois usos** | `bin/sombra_trava.sh:15–38` · `bin/vigia_tmux.py:360–389` · `bin/disjuntor.py:403–422` · `bin/handoff_sessao.sh:248–262` | A sombra cria/abre e toma `LOCK_EX`. O vigia também pode criar e mantém `LOCK_SH`. Stop e vigia sondam em `LOCK_SH`, compatível com o vigia. O handoff sonda em exclusivo e pode imprimir `TOMADA` enquanto só o vigia segura a trava. | Explicitar se cada leitura mede DIFF ativo ou indisponibilidade para uma nova tomada exclusiva. |
| Estado · `ultimo_clear.json` | **Sim, um escritor localizado e sentidos distintos** | `.claude/settings.json:34–61` · `bin/marco_de_clear.py:23–63` · `bin/orcamento_contexto.py:152–164` · `bin/hook_stop_fila1.py:1111–1153` · `bin/vigia_tmux.py:306–337,374–396` | `marco_de_clear.py` lê o anterior e grava sob `.lock` para `clear` e `compact`. `hora` é o instante do marco, `iso` é a data/hora formatada e `session_id` é do **último marco**. `clear_horas` guarda até 20 instantes sem IDs. O Stop usa `ref` para ciclo. O vigia usa o ID para escolher transcript e combina esse ID com um instante histórico para liberar a retomada. | Gravar o vínculo de cada clear com sessão e tentativa. |
| Estado · transcript e cota | **Sim, identidades distintas** | `bin/disjuntor.py:431–435,462–478,1313–1373` · `bin/hook_stop_fila1.py:1427–1433` · `bin/vigia_tmux.py:319–337` · `bin/statusline_cota.py:33–54` | O harness escreve o transcript segundo o código, e sua implementação fica **[nao verificado]**. O Stop recebe o caminho por payload. O vigia deriva o caminho do ID do marco. A status line escreve `cota.json` com o ID do payload dela, enquanto o B4 lê percentual e prazo de reset, sem usar esse ID para provar a sessão do painel. | Usar uma identidade atual comprovada do painel para a guarda. |
| Estado · painel e `feitos.json` | **Sim, o vínculo pode faltar na retomada** | `bin/vigia_tmux.py:144–168,212–216,263–297,340–342,374–393` · `bin/tests/test_vigia_tmux.sh:373–380` | O tmux fornece `%N`. O vigia usa esse valor nas capturas e teclas e é o escritor localizado de `feitos.json`. `pn` morde a troca de painel depois de um tick completo. Se cair após gravar `clear-enviado` e antes da segunda marca, o registro já tem `painel` e `hora_enter`, mas a retomada lê `painel_clear` ausente e `hora_clear` vira `0`. Pode aceitar marco velho e enviar a outro painel. Conferido em memória. Registros pendentes antigos também podem carecer do vínculo. | Persistir painel, instante e sessão juntos na primeira gravação de `clear-enviado`. Recusar retomada de registro incompleto que não possa ser recuperado. |
| Estado · orçamento, B4 e handoff | **Sim, donos identificados** | `bin/hook_stop_fila1.py:919–949,1048–1101,1153–1254` · `bin/orcamento_contexto.py:352–361` · `bin/disjuntor.py:333–398,1414–1423,1961–1975,2111–2116` · `bin/handoff_sessao.sh:18,318–327` | O Stop escreve `orcamento_estado.json` sob sua trava. Stop, vigia via `autoriza_clear`, orçamento e disjuntor leem. O disjuntor escreve seu JSON comum sob outra trava. `b4_para` é do Stop e `b4` é da negativa de agente no `PreToolUse`. O script de handoff escreve o documento, cujo **mtime** o Stop e a autorização leem. | Manter os significados separados e usar a mesma condição de leitura dos estados na autorização. |

### Travas e classes de reprovação

| id | procede? | arquivo:linha | consequência concreta | cura mínima |
|---|---|---|---|---|
| Ordem · publicação e espera | **Sim, solta antes das duas** | `bin/vigia_tmux.py:380–403,406–423` | A `.trava` do tick cobre a operação. Para CLEAR novo, `_toma_sombra` precede `_envia` e o `finally` fecha o descritor antes de `_publica` e do primeiro poll. Todo Enter de `/clear` desse caminho ocorre sob `LOCK_SH`. ESPERANDO e retomada usam `_envia` sem essa trava. | Cobrir no selo as fronteiras exatas de liberação. |
| Ordem · pode ficar presa | **Sim** | `bin/vigia_tmux.py:208–209,360–389` · `bin/hook_stop_fila1.py:462–483,1019–1022,1412–1420` | `flock` é não bloqueante e o `finally` cobre retornos e exceções depois da aquisição. Mas, se o RELATO for substituído por FIFO entre o checador e a leitura do topo, o `open` comum pode bloquear sob `LOCK_SH` antes de chegar ao `finally`. Se o `flock` falhar, o fd fica aberto até a saída do cron, sem a trava adquirida. `bin/vigia_tmux.py:97–107,364–389` | Ler o topo por abertura regular não bloqueante e fechar o fd também quando a aquisição falhar. |
| Classe · segundo juiz | **Parcialmente curada** | `bin/hook_stop_fila1.py:980–1045,1283–1313,1427–1466` · `bin/vigia_tmux.py:319–337` | Há um juiz para o disparo e a janela e um veredito comum de B4. Há duas decisões compostas, com aquisição de estado, identidade e tratamento de falhas próprios. As divergências acima são observáveis nessa composição. | Compartilhar a decisão composta e sua origem de dados. |
| Classe · escritor fora da porta | **Sim, preexistentes e um efeito indireto novo** | `bin/gerar_cortes.py:117–136` · `bin/deploy_agendado.sh:31–44` · `bin/keepalive.sh:4–9` · `bin/hook_stop_fila1.py:1019,1412–1424` · `bin/relato_afirma_com_prova.py:62–77,93–111` | Os dois escritores do RELATO continuam fora da trava. `keepalive.sh` é um segundo emissor de teclas, com alvo `code` e guardas próprias. O vigia passou a chamar o checador cujo CLI cria `afirma_sem_prova_base.txt` se a base faltar, embora `autoriza_clear` declare leitura sem gravação. Execução desses caminhos em produção **[nao verificado]**. | Coordenar os escritores e emissores. Usar no vigia uma chamada de prova sem inicialização da base. |
| Classe · relógio e correlação | **Parcialmente curada** | `bin/vigia_tmux.py:289–297,310–316,374–396` · `bin/marco_de_clear.py:40–57` · `bin/tests/test_vigia_tmux.sh:98–110,381–389` | `hora_enter` é amostrada ao chamar a tecla. Outro `/clear` posterior com sessão distinta satisfaz o predicado. Um `compact` pode fornecer o ID atual enquanto `clear_horas` fornece o instante de outro evento. `ss` m

[truncado em 20000 de 21006 caracteres]

## 23/11 16:29 · sessao 348baf761b84 · UI-EDITAR-RAIZ · censo · origem=Code

## 1. Edição da raiz, permissão e aviso

- **A rota do lápis já aceita uma raiz editável.** O GET busca o tipo pelo `tid` e só entra no fluxo “sob pai” quando está criando. O POST busca o mesmo registro e só lê `pai_id` na criação. O serviço atualiza o objeto recebido e o modelo mantém `nivel=0` quando `template_pai` é nulo. A raiz continua com o mesmo PK e sem pai. `app/escala/urls.py:25-27` · `app/escala/views_wizard.py:65-83,98-109` · `app/escala/services/cadastro_tipo.py:140-180,306-308` · `app/escala/models.py:771-775`
- **GET e POST exigem login e `editar_escalas`.** O decorador consulta `colaboradores.rbac_editar_escalas` e admite superusuário. `editavel=False` dá HTTP 400 no GET e recusa no serviço de gravação. A raiz passa pelas validações comuns do modelo e sai antes das validações de filho. O teste HTTP existente edita uma raiz e verifica que o ciclo é preservado. O teste seguinte cobre a recusa de uma raiz de sistema. `app/escala/views_wizard.py:63-70,95-109` · `app/colaboradores/mixins.py:322-346` · `app/escala/services/cadastro_tipo.py:144-150` · `app/escala/models.py:347-383` · `app/escala/tests/test_wizard_views.py:51-78`
- **O mesmo aviso vale para a raiz se houver dias barrados.** A porta pula as datas cobertas pela lavra e devolve a contagem. O juiz da lavra considera TXT entregue ou holerite publicado para o colaborador. `wizard_salvar` soma `_barrados` e acrescenta o aviso à mensagem de sucesso. Em HTMX, a mensagem vai no `HX-Trigger` de um 204 e a lista recarrega imediatamente ao receber o sucesso. A legibilidade do toast antes da recarga ficou [nao verificado]. `app/ponto/portas/celula.py:218-221,284-288,395-400` · `app/folha/export.py:597-628` · `app/escala/views_wizard.py:113-126` · `app/core/respostas.py:63-71` · `app/templates/escala/tipos_lista.html:242-246`

## 2. Campos gravados e nome exibido

- **O “Nome da escala” do wizard é `apelido`, e aparece no rótulo da raiz.** O campo visível envia `apelido`, gravado com até 12 caracteres. A raiz chama `rotulo_escala(t)`, que renderiza `nome_canonico_partes()` com o apelido, ciclo, dias e horários do próprio tipo. `nome_canonico()` é um método. `template_nome` e `descricao` entram no rótulo apenas se `nome_canonico_partes()` lançar exceção. No sucesso do drawer, a recarga consulta novamente os tipos. `app/templates/escala/wizard_tipo_escala.html:66-72` · `app/escala/services/cadastro_tipo.py:184-190` · `app/templates/escala/tipos_lista.html:48-49,242-246` · `app/core/templatetags/hasner_filters.py:64-89,119-136` · `app/escala/models.py:420-425,479-523` · `app/escala/views.py:83,114-115`
- **`template_nome` e `descricao` têm regras próprias.** O POST exige `template_nome` e o grava. No formulário de edição, ele vem oculto e só recebe o apelido se estiver vazio. `descricao` só recebe o nome como preenchimento quando está vazia. O wizard não envia `codigo`. O `save()` gera código se estiver vazio ou começar com `AUTO`. `app/templates/escala/wizard_tipo_escala.html:22,461-482` · `app/escala/views_wizard.py:18-60` · `app/escala/services/cadastro_tipo.py:148-150,184-190` · `app/escala/models.py:761-770`
- **Demais campos.** O serviço grava horários de início e fim, os dois pares de intervalo, duração do intervalo quando numérica, `noturno`, `permite_hora_extra`, extras declaradas antes/depois, calendário padrão, folgas semanais, dias de trabalho e `horarios_por_dia`. Para personalizada, fixa `ciclo_dias=7`. O modelo deriva `intervalo_modo` e `nivel`. A derivação de `minutos_jornada` no serviço roda na criação. `app/escala/services/cadastro_tipo.py:179-182,192-299` · `app/escala/models.py:771-799`

## 3. Herança, geração e DNA

- **Censo de `template_pai.` em produção.** As ocorrências literais estão em `app/escala/models.py:373` (`tipo_ciclo`, validação), `:376` (`descricao` e `tipo_ciclo`, mensagem de erro), `:380` (`nivel`, validação), `:775` (`nivel`, gravação) e `:814` (recursão de `valor_efetivo`). Esse método sobe ao pai quando o valor próprio é `None`, `''` ou `0`. A varredura encontrou apenas a definição e a chamada recursiva. Por alias, o gerador de código hierárquico lê `pai.codigo` ou chama o gerador do pai. `app/escala/models.py:745-759,806-814`
- **No nascimento sob pai**, o serviço grava o ciclo do pai e, no ramo comum, a base do pai. Para personalizada, usa `tipo_base_ref`, preenchido pelo formulário com a base do pai. Horários e pausas são gravados no próprio tipo a partir do payload. A operação de cópia também grava campos do original no novo objeto e conserva a FK ao mesmo pai. `app/escala/services/cadastro_tipo.py:152-178,205-267,397-418` · `app/templates/escala/wizard_tipo_escala.html:19`
- **Editar a raiz não muda por herança o rótulo nem as células futuras das filhas.** A lista lê o rótulo e os horários de `f`. O gerador escolhe o vínculo pela vigência, calcula se é dia de trabalho e monta o DNA a partir de `ec.tipo_escala`, com marcos e pausas desse tipo. `escala/alimentacao` resolve o vínculo pela geradora da célula e pela vigência. `app/templates/escala/tipos_lista.html:72-88` · `app/escala/models.py:281,326-345,479-523,1282,1376-1382` · `app/escala/alimentacao.py:214-216,269-286` · `app/ponto/portas/celula.py:935-953` · `app/ponto/management/commands/gerar_celulas.py:85-113`
- **O DNA gravado é lido da célula.** Sem marcos no DNA, o leitor de marcos recorre ao tipo do próprio vínculo. A edição pode reescrever DNA por regeneração explícita de um vínculo direto, se a célula for `gerada`, estiver fora da lavra e houver diferença. Há uma ressalva no trecho histórico: se um vínculo direto da raiz cobrir um dia cuja célula `gerada` pertence a outro vínculo do mesmo colaborador, o código pode trocar a geradora. O trecho futuro filtra pelo dono do dia. Existência dessa sobreposição em produção [nao verificado]. `app/escala/models.py:1128-1144,1703-1712` · `app/ponto/portas/celula.py:225-229,284-315` · `app/ponto/services/cadastro_futuro.py:43-59`

## 4. Vínculos diretos e regeneração

- **A raiz pode receber vínculo direto.** A FK aceita `TipoEscala` e a porta de vínculo busca o tipo pelo PK e o grava diretamente. Há um caso histórico documentado de vínculo corrigido para `PAI-6X1`, representado no teste por um tipo raiz. Quantidade e situação dos vínculos atuais [nao verificado]. `app/escala/models.py:871-877` · `app/colaboradores/services/vinculo.py:52-54,274-299` · `app/escala/tests/test_cobranca_orfa_regeneracao.py:13-19,122-126,153-155`
- **Toda edição chama a regeneração, inclusive a edição só de nome.** O trecho até hoje seleciona vínculos ativos com `tipo_escala=tipo` exato e começa no maior valor entre o início do vínculo e o início da competência anterior. O trecho futuro considera também vínculos encerrados desse tipo e seleciona células `gerada` já existentes depois de hoje. O nome fica fora do DNA, então a troca de nome por si só não cria diferença no DNA. `app/escala/services/cadastro_tipo.py:306-328,332-378` · `app/ponto/services/cadastro_futuro.py:19-25,43-59` · `app/ponto/management/commands/gerar_celulas.py:85-132` · `app/ponto/portas/celula.py:289-311`
- **A guarda protege as células.** O mesmo fluxo chama `invalidar_fechamento` para a janela histórica, que pode zerar `derivado_em` e atualizar `atualizado_em` de `FechamentoMensal`, sem filtro de lavra nesse `UPDATE`. `app/ponto/portas/celula.py:284-288` · `app/escala/services/cadastro_tipo.py:353-364` · `app/ponto/services/fechamento.py:1648-1656`

## 5. Selos de tela e regra de front

- **A busca literal em `app/*/tests` encontrou três arquivos.** O O74 renderiza a lista em `base.html`, recorta o cabeçalho da raiz e verifica a pílula do desenho. O smoke Chromium clica no lápis da **variante** e verifica o POST de preview e a ausência de gravação. O selo genérico de `window.hx*` varre todos os templates contra captura no carregamento. `filho-linha` teve zero ocorrências nessa busca. `app/escala/tests/test_o74_pilula_le_desenho.py:3,24-28,52-76` · `app/escala/tests/test_smoke_clique_wizard_preview.py:175-183` · `app/core/tests/test_hx_nao_se_captura_no_load.py:18,67-81`
- **Pela rota, o O122 também cobre essa tela.** Ele verifica a casca admin, o atalho “Nova escala”, seus atributos HTMX e o título. O recorte usa o prefixo `Hierarquia de tipos`. O texto atual é `clique no filho para editar`. `app/escala/tests/test_o122_etapa1_barra_em_tipos.py:48-97` · `app/templates/escala/tipos_lista.html:26`
- **A regra de smoke de clique nas duas cascas** enumera `static/js/`, service worker, bases e seus includes. A lista é uma folha da view que estende `base.html` e fica fora do grafo de includes literais das duas bases. A regra geral de selo de front pede renderização das duas cascas ou prova pelo grafo. O selo citado prova `_dialogo.html` pelo grafo, e o smoke Chromium renderiza ambas para verificar overlay e console. `CLAUDE.md:203-204` · `app/templates/escala/tipos_lista.html:1` · `app/escala/views.py:114-115` · `app/core/tests/test_selo_modal_nao_nasce_vazio.py:98-119` · `app/core/tests/test_smoke_chromium.py:172-190`

## 6. Renomear separado

A tabela de URLs de `escala` não tem rota separada de renomear `TipoEscala`. Na lista, a raiz tem o `+` para criar sob pai e a filha tem o lápis do wizard. Há um `reverse('escala:editar_tipo')` no pós-cópia sem rota correspondente na tabela. `app/escala/urls.py:12-39` · `app/templates/escala/tipos_lista.html:42-60,83-94` · `app/escala/views.py:141-151`

Hash Git, execução dos testes e smoke no navegador [nao verificado]. [nao verificado]

## 23/11 16:28 · sessao 824adfd70770 · NOTURNO-NAO-APURADO · censo · origem=Code

# NOTURNO-NAO-APURADO

Censo estático da cópia em modo somente leitura. O anexo identifica as autoridades. Hash Git da cópia, valores de produção, competências afetadas e resultado da suíte [nao verificado]. [nao verificado]

## 1. Escritores

`PeriodoCalculo` tem quatro campos noturnos, todos com default `0.0`: `minutos_noturnos_relogio`, `minutos_noturnos_legais`, `minutos_extra_50_noturna` e `minutos_extra_100_noturna`. `horas_noturnas` é uma propriedade de leitura que divide os minutos legais por 60. `app/ponto/motor_calculo_v2.py:208`, `app/ponto/motor_calculo_v2.py:214`, `app/ponto/motor_calculo_v2.py:224`, `app/ponto/motor_calculo_v2.py:259`

| Campos | Escritas encontradas |
|---|---|
| `minutos_noturnos_relogio` e `minutos_noturnos_legais` | `MotorBase._escrever_noturnas` grava o retorno de `calcular_noturnas`. Os corpos de `calcular_periodo` do 12x36, partido e comercial chamam esse escritor. `app/ponto/motor_calculo_v2.py:994`, `app/ponto/motor_calculo_v2.py:1003`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2896`, `app/ponto/motor_calculo_v2.py:3260` |
| `minutos_extra_50_noturna` e `minutos_extra_100_noturna` | `_aplicar_he_noturna` grava cada parcela quando o retorno correspondente é diferente de zero. É chamado dentro de `calcular_periodo` no 12x36 e comercial. No pós-processamento mensal, partido e comercial zeram as duas parcelas e chamam o mesmo escritor depois de apurar HE. No partido, os sítios são dobra, excedente diário e excedente semanal. No comercial, dobra e excedente diário. `app/ponto/motor_calculo_v2.py:1012`, `app/ponto/motor_calculo_v2.py:1019`, `app/ponto/motor_calculo_v2.py:2855`, `app/ponto/motor_calculo_v2.py:2937`, `app/ponto/motor_calculo_v2.py:3104`, `app/ponto/motor_calculo_v2.py:3160`, `app/ponto/motor_calculo_v2.py:3191`, `app/ponto/motor_calculo_v2.py:3294`, `app/ponto/motor_calculo_v2.py:3336`, `app/ponto/motor_calculo_v2.py:3361`, `app/ponto/motor_calculo_v2.py:3423` |
| Os quatro | `_dia_sem_previsto_fora_da_janela` zera por `setattr` quando incide o bloqueio total da L-097. `calcular_mes` chama esse método no laço principal, antes do bloco de crédito. `app/ponto/motor_calculo_v2.py:1422`, `app/ponto/motor_calculo_v2.py:1440`, `app/ponto/motor_calculo_v2.py:1471`, `app/ponto/motor_calculo_v2.py:2461`, `app/ponto/motor_calculo_v2.py:2511` |
| Relógio e legais, na releitura V3 | `reler` zera esses dois campos em cópia de par fechado duplicado e os reescreve para par elegível quando recebe `cct`. O módulo contém esse escritor. Os chamadores de `reler` localizados no censo estão na bateria de testes. `app/ponto/calculador/periodo.py:182`, `app/ponto/calculador/periodo.py:644`, `app/ponto/calculador/periodo.py:734`, `app/ponto/calculador/periodo.py:748`, `app/ponto/tests/test_o280_bateria_da_lei.py:4300` |

No crédito, `_pc` é calculado para cada par elegível. As duas escritas numéricas em `_abs[0]` são `minutos_trabalhados` e `minutos_fora_janela`. `app/ponto/motor_calculo_v2.py:2549`, `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/motor_calculo_v2.py:2559`

## 2. Leitores

| Leitor | O que faz |
|---|---|
| `ResultadoMes` e `FechamentoMensal` | `horas_noturnas` soma legais em `periodos + periodos_ft`. `horas_extra_50_noturna` e `horas_extra_100_noturna` somam os respectivos campos em `periodos`. O fechamento arredonda os três totais a duas casas e acrescenta à HE 100 noturna a parcela dos períodos de folga promovidos a feriado. `app/ponto/motor_calculo_v2.py:321`, `app/ponto/motor_calculo_v2.py:338`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/fechamento.py:921`, `app/ponto/services/fechamento.py:924`, `app/ponto/services/fechamento.py:962` |
| `DiaPago`, versão `motor` | `lavrar` soma legais e os dois subconjuntos de HE por dia, em horas. Folga com escala certa, sem escala certa e folga promovida a feriado carregam legais. A promoção a feriado também lê `minutos_extra_100_noturna`. `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:259`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1227` |
| Motor partido, régua `legais` | Usa `minutos_noturnos_legais - minutos_noturnos_relogio` de cada período do dia como acréscimo à base do excedente. É um leitor dos dois campos com efeito possível sobre a HE total. `app/ponto/motor_calculo_v2.py:3083`, `app/ponto/motor_calculo_v2.py:3136`, `app/ponto/motor_calculo_v2.py:3144` |
| Espelho, cartão/PDF e app | `somar_periodos` soma `p.horas_noturnas` e os minutos de relógio convertidos em horas. O espelho calcula `acrescimo_noturno` pela diferença. Na competência, `folha_manda` pode substituir o total legal pelo fechamento ou pela lavratura. Na linha diária, tela e PDF leem `DiaPago.horas_noturnas` via `rubricas_do_dia`. O PDF também consulta o campo legal do período para decidir se desenha a coluna. O app publica `total_noturnas` do resumo. `app/ponto/services/espelho.py:82`, `app/ponto/services/espelho.py:99`, `app/ponto/services/espelho.py:453`, `app/ponto/services/espelho.py:831`, `app/ponto/services/espelho.py:1306`, `app/relatorios/cartao_pela_celula.py:100`, `app/relatorios/cartao_pela_celula.py:176`, `app/relatorios/cartao_pela_celula.py:321`, `app/ponto/services/dia_pago.py:924`, `app/relatorios/pdf_espelho.py:502`, `app/relatorios/pdf_espelho.py:844`, `app/relatorios/pdf_espelho.py:911`, `app/templates/ponto/espelho.html:278`, `app/api/views.py:1431`, `app/api/views.py:1662` |
| TXT Domínio e holerite | O TXT lê `FechamentoMensal.horas_noturnas` no evento `adicional_noturno` (rubrica 25), resolve o código pelo mapa de rubricas e emite a linha. Emite HE pelos totais das faixas, sem linha própria dos subconjuntos noturnos. O módulo de holerite recebe PDF e entrega o arquivo armazenado. Valor de holerite emitido pelo Domínio [nao verificado]. `app/folha/export.py:424`, `app/folha/export.py:439`, `app/folha/export.py:730`, `app/holerite/views.py:1161`, `app/holerite/views.py:1193`, `app/holerite/views.py:1445` |
| `retratar_exportada` | Lê `FechamentoMensal` e a grade da célula. Nas linhas existentes atualiza os três campos de grade e os carimbos. Em dia sem linha cria o retrato com rubricas monetárias em zero. A linha de ajuste copia DSR e banco do fechamento. Esse caminho usa a grade e o gravado como insumo, sem consumir `PeriodoCalculo`. `app/ponto/services/dia_pago.py:451`, `app/ponto/services/dia_pago.py:461`, `app/ponto/services/dia_pago.py:469`, `app/ponto/services/dia_pago.py:488`, `app/ponto/services/dia_pago.py:504`, `app/ponto/services/dia_pago.py:538`, `app/folha/export.py:329` |
| Instrumentos fora de `tests/` | `diff_calculador` soma legais por dia no lado motor. `desvio_o68b` lê HE 100 noturna por período. `diff_extrato_parcial` lê o total legal de `ResultadoMes` e `aplicar_09_he_intra` lê os totais dos dois subconjuntos. O censo estrutural procura referências no código por AST. `app/ponto/management/commands/diff_calculador.py:260`, `app/ponto/management/commands/desvio_o68b.py:114`, `app/relatorios/management/commands/diff_extrato_parcial.py:68`, `app/ponto/management/commands/aplicar_09_he_intra.py:86`, `app/core/medida_leitor_do_dia.py:50`, `app/core/medida_leitor_do_dia.py:70` |

**Re-derivação do aberto.** O oráculo recalcula adicional e subconjuntos a partir dos segmentos fechados publicados e reutiliza as funções do motor. O `PeriodoCalculo` aberto entra como `(entrada, None, dia)` e fica fora dos segmentos fechados e das pontas de HE. O `_pc` do crédito é local ao laço. Assim, o oráculo não reconstrói o noturno desse crédito. A lavratura começa pela soma dos períodos, mas, quando o oráculo fornece a rubrica do dia, seu valor substitui a soma na versão `oraculo`. Essa substituição é o ponto de atenção da L-002. `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:157`, `app/ponto/calculador/alimentacao.py:525`, `app/ponto/calculador/regras.py:82`, `app/ponto/calculador/regras.py:716`, `app/ponto/calculador/regras.py:824`, `app/ponto/calculador/regras.py:844`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/fechamento.py:1288`, `app/ponto/services/dia_pago.py:322`, `app/docs/LEIS.md:60`

## 3. Período com `turno_aberto=True`

**Hoje os quatro campos ficam em `0.0`, inclusive com crédito de trabalhado.** Os três construtores retornam antes do cálculo noturno quando `saida is None`. O crédito conserva a geometria aberta e escreve apenas os dois totais de minutos já citados. `app/ponto/motor_calculo_v2.py:214`, `app/ponto/motor_calculo_v2.py:224`, `app/ponto/motor_calculo_v2.py:2751`, `app/ponto/motor_calculo_v2.py:2878`, `app/ponto/motor_calculo_v2.py:3216`, `app/ponto/motor_calculo_v2.py:2559`

- **Pontualidade.** `_aberto or _nulo` zera atraso e saída antecipada de todos os períodos agrupados no dia. `Motor12x36ComEscala` também retorna antes de julgar pontualidade do aberto. Essas guardas preservam os campos noturnos. `app/ponto/motor_calculo_v2.py:1131`, `app/ponto/motor_calculo_v2.py:1156`, `app/ponto/motor_calculo_v2.py:3546`
- **Releitura e apresentação.** `reler` pode zerar a pontualidade, conta o aberto e pula suas demais escritas, preservando o noturno que recebeu. Espelho, PDF e relatório usam o indicador para data, contagem, inconsistência ou rótulo. O crivo de aprovação também registra `turno_aberto` a partir da contagem mensal. `app/ponto/calculador/periodo.py:636`, `app/ponto/calculador/periodo.py:644`, `app/ponto/motor_calculo_v2.py:373`, `app/ponto/services/espelho.py:434`, `app/ponto/services/espelho.py:671`, `app/relatorios/pdf_espelho.py:513`, `app/relatorios/pdf_espelho.py:973`, `app/relatorios/services.py:144`, `app/templates/ponto/espelho.html:194`, `app/folha/export.py:830`
- **O que pode apagar um valor transferido.** Os resets mensais do partido e comercial zerariam os **dois campos de HE noturna** de `_abs[0]`. O adicional legal e o de relógio passam por esses resets. Na versão `oraculo`, a substituição da rubrica pode descartar a contribuição do aberto na **linha do dia**, sem alterar o objeto. `app/ponto/motor_calculo_v2.py:2928`, `app/ponto/motor_calculo_v2.py:2937`, `app/ponto/motor_calculo_v2.py:3329`, `app/ponto/motor_calculo_v2.py:3336`, `app/ponto/services/dia_pago.py:322`

## 4. Retorno de `calcular_periodo(e, s, itv)` e HE noturna

- **Adicional.** Para um par fechado, os três corpos recortam as pontas e os intervalos e escrevem relógio e legais. `calcular_noturnas` calcula a janela e a prorrogação configurada, desconta os intervalos noturnos e aplica o fator da hora reduzida conforme a régua. Plantão delega ao 12x36. 12x36 com escala delega e acrescenta pontualidade. Partido noturno e intermitente herdam seus respectivos corpos. `app/ponto/motor_calculo_v2.py:983`, `app/ponto/motor_calculo_v2.py:990`, `app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2886`, `app/ponto/motor_calculo_v2.py:2896`, `app/ponto/motor_calculo_v2.py:3224`, `app/ponto/motor_calculo_v2.py:3260`, `app/ponto/motor_calculo_v2.py:3534`, `app/ponto/motor_calculo_v2.py:3546`, `app/ponto/motor_calculo_v2.py:3560`, `app/ponto/motor_calculo_v2.py:3569`
- **HE noturna.** 12x36 e comercial já podem preencher as duas parcelas no retorno de `_pc`. Partido devolve as duas nos defaults e apura HE no mensal. `he_noturna` usa as pontas brutas guardadas no período, as HE 50/100 já calculadas, os intervalos do motor e a prorrogação. Recorta a cauda extra e reparte o noturno proporcionalmente. O split usa essa geometria e as faixas de HE como entrada. `app/ponto/motor_calculo_v2.py:430`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:2855`, `app/ponto/motor_calculo_v2.py:2896`, `app/ponto/motor_calculo_v2.py:3294`
- **Entrada do aberto na derivação mensal.** Seu trabalhado creditado já entra na soma do dia do partido e do comercial. O aberto pode receber HE total como último período do dia, por dobra, ou como último da semana no partido. O split sobre o próprio aberto retorna antes do cálculo porque `saida` é `None`. No partido com régua `legais`, transferir relógio e legais também alimentaria o termo de ganho da hora reduzida. Se esse ganho alterar a HE do dia e o portador for um período fechado, o split desse portador usa a nova HE. `app/ponto/motor_calculo_v2.py:1012`, `app/ponto/motor_calculo_v2.py:2943`, `app/ponto/motor_calculo_v2.py:3087`, `app/ponto/motor_calculo_v2.py:3097`, `app/ponto/motor_calculo_v2.py:3136`, `app/ponto/motor_calculo_v2.py:3153`, `app/ponto/motor_calculo_v2.py:3181`, `app/ponto/motor_calculo_v2.py:3328`, `app/ponto/motor_calculo_v2.py:3348`, `app/ponto/motor_calculo_v2.py:3354`, `app/ponto/motor_calculo_v2.py:3416`

## 5. Testes existentes [nao verificado]

| Caso | O que a asserção cobre |
|---|---|
| BO-B caso 1 e controles 5/6 | Caso 1 cobra 357 minutos, um aberto, pontualidade e falta em zero, igualdade com o juiz e alerta `pares fechados`. Caso 5 cobra zero trabalhado com entrada única. Caso 6 cobra 440 no dia fechado. Nenhuma asserção de noturno nesses casos. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:121`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:131`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:143`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:148` |
| BO-B casos 2, 4b e 8 | Caso 2 cobra 438 minutos, zero fora da janela e CxR. Caso 4b cobra 146 trabalhados, 130 fora e 236 no juiz. Caso 8 cobra 351 no motor, 447 no juiz e contadores `(0, 1)` para crédito e pausa dentro de par pago. Nenhuma asserção de noturno. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:157`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:204`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:221` |
| BO-B caso 3, noturno | O pinado espera 660 trabalhados e zero fora da janela. O gêmeo cobra 360 no motor, um aberto, juiz `(300, 60, False)` e `_dia_aberto_creditado == 0`. São asserções de trabalhado, geometria e ausência de crédito, sem valor esperado de adicional ou HE noturna. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:166`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:179`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:183` |
| Bateria C6 e controles de zero | C6 cobra dois abertos e 300 minutos de um par fechado diurno. Os testes de zero noturno localizados cobram `noturnas_do_dia` com par sem ponta, invertido ou vazio, e `_aplicar_he_noturna` com `saida=None`. Nenhum deles fixa zero noturno em **aberto que recebeu crédito de par fechado**. `app/ponto/tests/test_o280_bateria_da_lei.py:1272`, `app/ponto/tests/test_o280_bateria_da_lei.py:3198`, `app/ponto/tests/test_s5b_regra_noturnas.py:95`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:181` |

**O vermelho estrutural está identificado.** A checagem em memória com o próprio `varrer` reconheceu `MotorBase.calcular_mes` como segundo escritor ao inserir uma atribuição direta de minutos legais no crédito. O selo exige que o conjunto contenha somente `MotorBase._escrever_noturnas`. Ele também reconhece `setattr` com nome literal. A suíte Django [nao verificado]. `app/ponto/tests/test_noturno_sitio_unico.py:63`, `app/ponto/tests/test_noturno_sitio_unico.py:76`, `app/ponto/tests/test_noturno_sitio_unico.py:100`, `app/ponto/tests/test_noturno_sitio_unico.py:107`

## 6. Leis e cortes

- **R3 / L-099.** O placar diz que o dia aberto “MANTEM o numero rotulado (a soma dos pares fechados)”, igual em tela, PDF, cartão, app e TXT. O aval BO-B registra R3 + L-084 e veda fabricar batida ou pagar aberto como cheio. `CORTES.md` registra R3 no placar e remete B-B24 ao dia em aberto. `DOSSIES.md` usa o R3 como molde da palavra com número rotulado para dia sem turno pareável. `app/core/placar_estrutural.py:146`, `app/docs/LEIS.md:122`, `app/docs/CORTES.md:352`, `app/docs/CORTES.md:402`, `app/docs/DOSSIES.md:140`, `app/docs/PROMPTS.md:1357`
- **L-115 / L-097 / L-084.** Depois de pausa fechada, “essa batida e a SAIDA do turno pela posicao, qualquer que seja o tipo gravado”. A última E solta sem pausa fechada segue a BUG-144. O que passa do marco é ponta de HE pela L-097, e a L-084 mantém o julgamento pelas duas pontas do dia. No crédito, a cauda só entra se a pausa começa onde termina o par já fechado. Entrada já paga e par invertido ou nulo são descartados. `app/docs/LEIS.md:115`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:153`, `app/docs/LEIS.md:434`, `app/ponto/motor_calculo_v2.py:2525`, `app/ponto/motor_calculo_v2.py:2535`, `app/ponto/motor_calculo_v2.py:2550`
- **L-073 / L-093.** O teto temporal exige instante ou fato encerrado. O teto aritmético limita `atraso + saída antecipada` do dia ao previsto menos o trabalhado. A guarda do aberto aplica zero às duas rubricas de pontualidade. `app/docs/LEIS.md:100`, `app/docs/LEIS.md:111`, `app/ponto/motor_calculo_v2.py:1106`, `app/ponto/motor_calculo_v2.py:1156`
- **L-002 / L-095 / L-110.** A testemunha lê a mesma autoridade. O dinheiro do dia é lavrado uma vez. A referência do caso esperado é a regra aplicada ao cadastro e às batidas. O juiz registrado do dinheiro do par é `MotorBase.calcular_periodo`. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:136`, `app/core/juizes.py:1208`, `app/docs/CORTES.md:376`
- **L-092 / L-113 e mão única.** A L-092 registra a proteção histórica do gravado. A L-113 revoga a recusa genérica de recálculo e preserva o TXT anterior, a trilha e o DIFF prévio ao ato de frota. A guarda temporária na cópia é específica de 09/2026 nas empresas que exportaram. Para esta obra, o limite é a instrução do aval relatada no pedido. O corte de mão única inclui ponto, folha e export, e a zona inviolável exige aval explícito para o motor. `app/docs/LEIS.md:125`, `app/docs/LEIS.md:155`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:560`, `app/docs/CORTES.md:611`, `CLAUDE.md:143`

## 7. Decisões do construtor e perguntas de lei

- **Decisão de construção.** Acumular relógio e legais dos mesmos `_pc` que passam pelos filtros atuais, na mesma iteração que acumula trabalhado e fora da janela. Fazer a escrita pelo sítio canônico, preservando o selo e o rótulo do período. `minutos_realizados` entra no fechamento e em `DiaPago` pelo mapa da grade, uma fonte distinta do crédito. `app/ponto/motor_calculo_v2.py:994`, `app/ponto/motor_calculo_v2.py:2549`, `app/ponto/tests/test_noturno_sitio_unico.py:107`, `app/ponto/services/fechamento.py:1060`, `app/ponto/services/dia_pago.py:285`
- **Pergunta de lei sobre excedente.** O aval deve mover somente o adicional ou também o excedente do partido com régua `legais`, quando o ganho da hora reduzida do crédito eleva a base? `app/ponto/motor_calculo_v2.py:3136`, `app/ponto/motor_calculo_v2.py:3144`
- **Pergunta de lei sobre os subconjuntos.** Se o aval alcança HE 50/100 noturna, qual parcela deve prevalecer depois da apuração por dia ou semana: a do `_pc` ou a correspondente à HE consolidada? As faixas do par e as do mensal são apuradas em momentos distintos, e o aberto segue sem ponta final para o split. `app/ponto/motor_calculo_v2.py:1012`, `app/ponto/motor_calculo_v2.py:2847`, `app/ponto/motor_calculo_v2.py:3153`, `app/ponto/motor_calculo_v2.py:3181`, `app/ponto/motor_calculo_v2.py:3331`, `app/ponto/motor_calculo_v2.py:3416`
- **Pergunta de escopo sobre o oráculo.** A versão `oraculo` deve incorporar o mesmo crédito nesta obra? Hoje a sua alimentação omite a geometria do `_pc`, e uma rubrica calculada por dia pode sobrepor a soma vinda do motor. `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/calculador/alimentacao.py:83`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/dia_pago.py:322`
- **Decisão de verificação.** O caso que falta na bateria é um aberto que efetivamente recebe crédito de um par com noturno positivo. O caso noturno existente

[truncado em 20000 de 20426 caracteres]

## 23/11 16:28 · sessao d05e02130b88 · - · - · origem=Ronald (tmux)

**3. Herança e células**

- `template_pai` é uma FK anulável para o próprio `TipoEscala`, com relação reversa `variantes`. As leituras literais de `template_pai.` no código de produção estão em `app/escala/models.py:373` (`tipo_ciclo`, validação), `:376` (`descricao` e `tipo_ciclo`, mensagem de erro), `:380` (`nivel`, validação), `:775` (`nivel`, cálculo ao salvar) e `:814` (recursão de `valor_efetivo`). O fallback de `valor_efetivo` existe para valor próprio `None`, `''` ou `0`. Na busca por esse símbolo, as únicas ocorrências de produção foram a definição e a recursão em `app/escala/models.py:806-814`.
- A geração de código lê `pai.codigo` por alias em `app/escala/models.py:745-746`, quando o filho tem código vazio ou `AUTO` (`:761-770`). Na criação sob pai, o wizard grava o ciclo e a base no filho. A base de uma personalizada chega em `tipo_base_ref`, preenchido pelo wizard com `pai.tipo_base`. Horários e pausas vêm do payload ou dos campos do próprio tipo. `app/escala/services/cadastro_tipo.py:152-178,205-267`, `app/templates/escala/wizard_tipo_escala.html:19`. A cópia duplica campos do original e guarda a mesma FK de pai. `app/escala/services/cadastro_tipo.py:397-418`.
- O rótulo e os horários da filha vêm de `f`, e `nome_canonico_partes()` consulta campos próprios. A edição da raiz não muda o que a filha existente mostra. O cabeçalho do fluxo de **criar sob pai** consulta o rótulo e `template_nome` da própria raiz. `app/templates/escala/tipos_lista.html:72-88`, `app/core/templatetags/hasner_filters.py:64-89`, `app/escala/models.py:479-523`, `app/templates/escala/wizard_tipo_escala.html:53-56`.
- O gerador avalia a vigência e usa o `tipo_escala` do vínculo. `montar_dna` lê marcos, pausas, ciclo, jornada e extras desse tipo. A alimentação escolhe o vínculo pela geradora ou pela vigência. A edição da raiz não altera novas células cujo vínculo aponta para a filha. `app/ponto/portas/celula.py:919,942-953`, `app/ponto/management/commands/gerar_celulas.py:85-113,128-132`, `app/escala/models.py:326-345,1282,1376-1382`, `app/escala/alimentacao.py:269-286`.
- O leitor usa `CelulaDia.trabalha` e os marcos do DNA gravado. Sem marcos no DNA, consulta o tipo do próprio vínculo. A regeneração explícita do wizard seleciona vínculos diretos do tipo editado, sem percorrer `variantes`. Pode reescrever células geradas elegíveis, guardando `dna_anterior`. Para futuras existentes, preserva o dia atribuído pela juíza a outro vínculo. `app/escala/models.py:1098-1107,1128-1144`, `app/escala/services/cadastro_tipo.py:323-328,339-355,370-378`, `app/ponto/portas/celula.py:284-315`, `app/ponto/services/cadastro_futuro.py:43-59`.
- Se o mesmo colaborador tiver vínculo ativo direto na raiz sobreposto ao da filha, o caminho histórico pode substituir a geradora do dia porque valida a cobertura e compara a geradora para decidir se reescreve, sem o filtro de dono do suplemento futuro. Isso é efeito de vínculo sobreposto. A existência dessa combinação nos dados atuais é [nao verificado]. `app/escala/services/cadastro_tipo.py:339-355`, `app/ponto/portas/celula.py:227-229,284-310`, `app/ponto/services/cadastro_futuro.py:50-59`.

## 23/11 16:28 · sessao 284db5438802 · - · - · origem=Ronald (tmux)

**4. Vínculos e regeneração.** A FK permite apontar diretamente para uma raiz. O painel oferece os tipos com `is_template=True` sem filtrar pelo pai. O serviço grava no vínculo o próprio tipo selecionado pelo PK. [app/escala/models.py:871](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/models.py:871) · [app/colaboradores/views.py:270](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/views.py:270) · [app/colaboradores/services/vinculo.py:52](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/services/vinculo.py:52) · [app/colaboradores/services/vinculo.py:274](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/services/vinculo.py:274) · [app/colaboradores/services/vinculo.py:287](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/colaboradores/services/vinculo.py:287)

Há caso histórico registrado em comentário de produção com vínculo direto em `PAI-6X1`. A fixture representa esse destino como raiz e salva esse tipo diretamente no vínculo. Quantidade e estado atuais em produção [nao verificado]. [app/escala/tests/test_cobranca_orfa_regeneracao.py:13](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_cobranca_orfa_regeneracao.py:13) · [app/escala/tests/test_cobranca_orfa_regeneracao.py:122](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_cobranca_orfa_regeneracao.py:122) · [app/escala/tests/test_cobranca_orfa_regeneracao.py:153](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_cobranca_orfa_regeneracao.py:153)

Toda edição chama a regeneração para `tipo_escala=tipo` exato. A primeira janela abrange vínculos ativos, desde a competência anterior ou o início do vínculo, o que for mais recente, até hoje. O suplemento abrange também vínculos encerrados e seleciona células futuras já existentes com `origem='gerada'`, respeitando o vínculo do dia. A porta pula os lavrados e registra os dias barrados. Uma mudança isolada de nome chama a porta, e o nome fica fora do DNA. [app/escala/services/cadastro_tipo.py:306](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/services/cadastro_tipo.py:306) · [app/escala/services/cadastro_tipo.py:332](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/services/cadastro_tipo.py:332) · [app/escala/services/cadastro_tipo.py:370](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/services/cadastro_tipo.py:370) · [app/ponto/services/cadastro_futuro.py:22](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/services/cadastro_futuro.py:22) · [app/ponto/services/cadastro_futuro.py:43](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/services/cadastro_futuro.py:43) · [app/ponto/portas/celula.py:284](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/portas/celula.py:284) · [app/ponto/portas/celula.py:395](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/portas/celula.py:395) · [app/ponto/management/commands/gerar_celulas.py:85](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/management/commands/gerar_celulas.py:85)

**6. Renomear.** Não há rota separada no `app escala`. A lista tem `+` na raiz e lápis, cópia e exclusão nas filhas. O `reverse('escala:editar_tipo')` no retorno da cópia referencia um nome ausente da tabela de rotas. O Django Admin expõe o `ModelAdmin` genérico de `TipoEscala`. [app/escala/urls.py:12](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/urls.py:12) · [app/templates/escala/tipos_lista.html:58](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:58) · [app/templates/escala/tipos_lista.html:83](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:83) · [app/escala/views.py:149](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/views.py:149) · [app/escala/admin.py:7](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/admin.py:7)

## 23/11 16:28 · sessao 0a10596158c5 · - · - · origem=Ronald (tmux)

## 5. Selos e regra de front

- A busca literal por `tipos_lista`, `wizard_tipo_editar` e `filho-linha` em `app/*/tests` encontrou três arquivos. O O74 renderiza `/escala/tipos/` na casca `base.html`, recorta o cabeçalho da raiz e verifica a pílula do desenho. O smoke do wizard clica no lápis da variante e verifica o POST de **preview**. O terceiro teste varre todos os templates para detectar captura de `window.hx*` no carregamento. `filho-linha` teve zero ocorrências nesse recorte. [test_o74_pilula_le_desenho.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o74_pilula_le_desenho.py:24) · [test_o74_pilula_le_desenho.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o74_pilula_le_desenho.py:52) · [test_smoke_clique_wizard_preview.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_smoke_clique_wizard_preview.py:175) · [test_hx_nao_se_captura_no_load.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/core/tests/test_hx_nao_se_captura_no_load.py:67)
- Pela URL, também há o O122, que verifica a casca, o atalho «Nova escala», os atributos HTMX e o título. O recorte do título depende da expressão `Hierarquia de tipos`. O texto atual continua com `clique no filho para editar`. O O122 mede o estilo pelo Chromium e cobra console limpo. [test_o122_etapa1_barra_em_tipos.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o122_etapa1_barra_em_tipos.py:48) · [test_o122_etapa1_barra_em_tipos.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o122_etapa1_barra_em_tipos.py:84) · [test_o122_etapa1_barra_em_tipos.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_o122_etapa1_barra_em_tipos.py:140) · [tipos_lista.html](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:26)
- `FRONT SEM SMOKE NÃO SOBE` exige smoke de clique do Ronald nas duas cascas para push de `static/js/`, service worker, bases ou seus includes. `tipos_lista.html` é uma folha renderizada pela view e estende literalmente `base.html`. A travessia dos includes literais pelo método do selo confirmou que nenhuma das bases inclui essa folha. A regra geral de selo de front permite comprovar as duas cascas pelo grafo de includes. O selo citado usa o grafo para `_dialogo.html`, e o smoke Chromium abre ambas as cascas para verificar overlay e console. [CLAUDE.md](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/CLAUDE.md:203) · [tipos_lista.html](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/templates/escala/tipos_lista.html:1) · [views.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/views.py:114) · [test_selo_modal_nao_nasce_vazio.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/core/tests/test_selo_modal_nao_nasce_vazio.py:98) · [test_smoke_chromium.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/core/tests/test_smoke_chromium.py:172)

O teste HTTP de edição existente usa uma raiz: persiste `template_nome`, preserva `tipo_ciclo` e espera 302. O teste seguinte usa raiz `editavel=False` e confirma que o POST preserva o nome. [test_wizard_views.py](/home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/escala/tests/test_wizard_views.py:51)

## 23/11 16:20 · sessao 283dd0d8704d · NOTURNO-NAO-APURADO · sonda · origem=Code

```python
import logging
import sys
from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
from unittest.mock import patch

sys.dont_write_bytecode = True

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from ponto.janelas import janela_atual
from ponto.models import DiaPago
from ponto.services import dia_pago
from ponto.services.fechamento import recalcular_fechamento_mes
from ponto.turnos import batidas_apuraveis


INI = date(2026, 10, 1)
ATE = date(2026, 11, 22)
COLABS = (37, 113, 174, 218, 868, 382)
UM_DIA = timedelta(days=1)
H22, H05 = time(22), time(5)
ALERTA_CREDITO = "contados dos pares fechados"
ARGUMENTOS_PERIODOS = ("periodos", "ft_certa", "ft_feriado", "ft_sem_escala")
FLAGS_PERIODO = ("turno_aberto", "folga_trabalhada", "hora_extra_100")
NV = "[nao verificado]"


def alvos_por_competencia(colab):
    # C1 do anexo g3.py. Competencia em app/ponto/janelas.py:10,60.
    inicio = timezone.make_aware(datetime.combine(INI, H22))
    fim = timezone.make_aware(datetime.combine(ATE + UM_DIA, H05))
    dias_com_batida = set()
    batidas = batidas_apuraveis(colab, inicio, fim).values_list("timestamp", flat=True)
    for timestamp in batidas.iterator():
        local = timezone.localtime(timestamp)
        if local.time() >= H22:
            dias_com_batida.add(local.date())
        elif local.time() < H05:
            dias_com_batida.add(local.date() - UM_DIA)

    linhas = DiaPago.objects.filter(
        colaborador_id=colab.pk, data__range=(INI, ATE),
        tipo=DiaPago.TIPO_DIA, versao="motor",
        minutos_realizados__gt=0, horas_noturnas=0,
    ).values_list("data", "mes", "ano", "minutos_realizados", "horas_noturnas")

    alvos = defaultdict(dict)
    for dia, mes, ano, realizado, noturno_h in linhas:
        if dia in dias_com_batida:
            _, fim_comp = janela_atual(dia, colab.empresa)
            if (ano, mes) == (fim_comp.year, fim_comp.month):
                alvos[ano, mes][dia] = (realizado, noturno_h * 60)
    return alvos


def capturar_lavratura(cid, mes, ano):
    capturas = []

    def capturar(colab, mes_arg, ano_arg, *, versao, **dados):
        if versao == "motor":
            capturas.append(((colab.pk, mes_arg, ano_arg), dados))
        return 0

    # O alias e importado dentro da funcao. app/ponto/services/fechamento.py:1191,1200,1277.
    with patch.object(dia_pago, "lavrar", new=capturar):
        recalcular_fechamento_mes(
            mes, ano, colaborador_ids=[cid],
            somente_leitura=True, lavrar_dias_pagos=True,
        )

    if not capturas:
        return None
    if len(capturas) != 1 or capturas[0][0] != (cid, mes, ano):
        raise RuntimeError(f"Captura inesperada para col{cid:03d} {mes:02d}/{ano}")
    return capturas[0][1]


def hora_local(instante):
    return timezone.localtime(instante).strftime("%H:%M:%S") if instante is not None else "-"


def ramo_do_dia(periodos, realizado, noturno):
    if not periodos:
        return "sem_periodo"
    if noturno != 0:
        return "outro:periodo_com_noturno"

    positivos = [(p, credito) for _, p, credito in periodos if p.minutos_trabalhados > 0]
    if not positivos:
        return "outro:periodos_sem_trabalhado"

    creditados = [p for p, credito in positivos if p.turno_aberto and credito]
    if creditados:
        # Grade inteira e arredondamento do proprio alerta.
        # app/escala/services/leitor_celula.py:422,423
        # app/ponto/motor_calculo_v2.py:2559,2563
        if realizado is None:
            return "outro:credito_com_realizado_ausente"
        if len(positivos) == 1 and realizado > 0 and round(creditados[0].minutos_trabalhados) == realizado:
            return "credito_turno_aberto"
        return "outro:credito_nao_cobre_sozinho_o_realizado"

    if all(not p.turno_aberto and p.saida is not None for p, _ in positivos):
        return "periodo_fechado_sem_noturno"
    return "outro:trabalhado_em_aberto_sem_alerta_de_credito"


def emitir(colab_rotulo, alvos, lavra, contagem):
    por_dia = defaultdict(list)
    if lavra is not None:
        mapa = lavra["dia_da_jornada"]
        for argumento in ARGUMENTOS_PERIODOS:
            for p in lavra[argumento]:
                dia = dia_pago._dia_de(p, mapa)  # app/ponto/services/dia_pago.py:174,235,245,260
                if dia in alvos:
                    credito = any(ALERTA_CREDITO in alerta for alerta in p.alertas)
                    por_dia[dia].append((argumento, p, credito))

    for dia, (gravado, noturno_gravado) in sorted(alvos.items()):
        prefixo = f"{colab_rotulo} {dia}"
        if lavra is None:
            trabalhado = noturno = realizado_saida = NV
            ramo = "outro:lavratura_motor_nao_capturada"
        else:
            periodos = por_dia[dia]
            for argumento, p, credito in periodos:
                flags = [campo for campo in FLAGS_PERIODO if getattr(p, campo)]
                print(
                    f"PERIODO {prefixo} argumento={argumento} "
                    f"entrada={hora_local(p.entrada)} saida={hora_local(p.saida)} "
                    f"turno_aberto={p.turno_aberto} minutos_trabalhados={p.minutos_trabalhados} "
                    f"minutos_noturnos_legais={p.minutos_noturnos_legais} "
                    f"minutos_fora_janela={p.minutos_fora_janela} "
                    f"alerta_contados_dos_pares_fechados={credito} lista_de_ramos={flags}"
                )
            trabalhado = sum(p.minutos_trabalhados or 0 for _, p, _ in periodos)
            noturno = sum(p.minutos_noturnos_legais or 0 for _, p, _ in periodos)
            realizado = (lavra["realizado_por_dia"] or {}).get(dia)
            realizado_saida = NV if realizado is None else realizado
            ramo = ramo_do_dia(periodos, realizado, noturno)

        print(
            f"DIA {prefixo} DiaPago_minutos_realizados={gravado} "
            f"DiaPago_horas_noturnas_x60={noturno_gravado} "
            f"soma_trabalhados={trabalhado} soma_noturnos={noturno} "
            f"realizado_por_dia={realizado_saida} RAMO={ramo}"
        )
        contagem[ramo] += 1


def main():
    contagem = Counter(dict.fromkeys((
        "credito_turno_aberto", "periodo_fechado_sem_noturno", "sem_periodo",
    ), 0))
    nivel_anterior = logging.root.manager.disable
    logging.disable(sys.maxsize)  # app/ponto/services/fechamento.py:1305,1306
    try:
        with schema_context("juliani"), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ")
                cursor.execute("SET TRANSACTION READ ONLY")
                cursor.execute(
                    "SELECT current_database(), current_schema(), "
                    "current_setting('transaction_read_only')"
                )
                if cursor.fetchone() != ("sombra", "juliani", "on"):
                    raise RuntimeError("A sonda exige sombra/juliani em READ ONLY")

            colabs = Colaborador.objects.filter(pk__in=COLABS).select_related("empresa").order_by("pk")
            for colab in colabs:
                for (ano, mes), alvos in sorted(alvos_por_competencia(colab).items()):
                    lavra = capturar_lavratura(colab.pk, mes, ano)
                    emitir("col%03d" % colab.pk, alvos, lavra, contagem)
    finally:
        logging.disable(nivel_anterior)

    for ramo, total in sorted(contagem.items()):
        print(f"TOTAL RAMO={ramo} n={total}")
    print("FIM")


main()
```

`recalcular_por_evento` chama `recalcular_fechamento_mes`, que chama `apurar_por_fatia`. A montagem passa por `get_motor_cct` e pela fábrica `motor_calculo_v2.get_motor`. Cada fatia roda `motor.calcular_mes`, e o fechamento concatena os períodos e mescla `dia_da_jornada` antes de chamar `lavrar(periodos=resultado.periodos, ...)`. A sonda captura essa chamada e lê `realizado_por_dia` do mesmo conjunto de argumentos, vindo de `por_dia_da_grade`. `app/ponto/services/fechamento.py:1810`, `:735–756`, `:279–316`, `:1060–1061`, `:1200–1227`, `app/core/regua_cct.py:428–479`, `app/ponto/motor_calculo_v2.py:3626`.

`lista_de_ramos` imprime os nomes dos campos booleanos verdadeiros do `PeriodoCalculo`. Para rotular `credito_turno_aberto`, a sonda exige que o período aberto com o alerta seja o único com trabalhado positivo e que seus minutos arredondados, na precisão do próprio alerta, coincidam com o realizado da grade. A execução em Sombra e o ramo histórico de cada dia estão [nao verificado]. `app/ponto/motor_calculo_v2.py:236–253`, `:2553–2563`, `app/escala/services/leitor_celula.py:422–423`.

A transação `READ ONLY` protege a sonda. O lançador `--rodar` desta cópia executa `lavra_de_prod` antes de iniciar o script, com `update_or_create` em Sombra, e grava a saída com `tee`. `bin/sombra.sh:136–167`, `:483–493`.

## 23/11 15:55 · sessao f539a8e990a7 · PAR-PELO-TURNO · leitura · origem=Code

**Os dois campos são gravados por `ponto.services.dia_pago.lavrar`, mas chegam de listas diferentes.** O fechamento passa `realizado_por_dia` da grade e, separadamente, os períodos apurados pelo motor. `lavrar` monta a linha com `versao='motor'` e a persiste com `bulk_create`. (`app/ponto/models.py:281,327,351`, `app/ponto/services/fechamento.py:1200–1227`, `app/ponto/services/dia_pago.py:335–360`)

## De onde vem cada número

| Campo | Escritor e origem |
|---|---|
| `minutos_realizados` | `lavrar` copia o mapa em `app/ponto/services/dia_pago.py:288–292`. O fechamento obtém o mapa com `grade_do_fechamento` e `por_dia_da_grade`, que considera dias de `trabalho` ou `ausencia` e aplica `min(realizado, previsto)` por dia. A grade lê o número já gravado em `CelulaDia.ata`. (`app/ponto/services/fechamento.py:1013–1014,1060–1064,1212–1213`, `app/folha/export.py:329–338`, `app/escala/services/leitor_celula.py:379,423`, `app/ponto/services/dia_pago.py:386–402`) |
| `horas_noturnas` | `lavrar` soma `p.minutos_noturnos_legais / 60` por dia da jornada, nos períodos normais e nos três grupos de folga/feriado que recebe. O número nasce em `MotorBase.calcular_noturnas`, que mede a interseção do **par efetivo** com a janela noturna, desconta os intervalos noturnos e aplica o fator da régua. `_escrever_noturnas` grava o resultado no `PeriodoCalculo`. (`app/ponto/services/dia_pago.py:126–132,173–179,223–265`, `app/ponto/services/fechamento.py:1203–1204,1217`, `app/ponto/motor_calculo_v2.py:82–118,969–1005,2759–2765,2792,2896,3260`) |

**Onde o pareamento diverge**

- **Grade que alimenta `minutos_realizados`.** Na configuração padrão da cópia, o cartório monta a grade por turno. O montador chama `turnos_do_colab` com as lâmpadas da montagem atual. Essa função carrega as batidas apuráveis do colaborador, sem retratadas, com dois dias de margem antes e um depois. Pareia por segmento de vínculo com `parear_turnos`. `realizado_dos_turnos` seleciona por `data_turno` e soma até `_fim_fechado`, inclusive quando o turno segue aberto. O cartório leva esse número à ata. A configuração efetiva em produção é [nao verificado]. (`app/config/settings/base.py:165`, `app/escala/utils.py:1889–1909,2052–2069,2095–2101`, `app/ponto/turnos.py:1834–1847,1867–1871,1944,1977–2000,609–622,719–771,774–793`, `app/ponto/services/cartorio.py:759,1199–1204,1898–1901,1920–1922`)
- **Motor que alimenta `horas_noturnas`.** O fechamento carrega `batidas_apuraveis`, recorta por fatia de vínculo e chama `calcular_mes`. O motor remove duplicatas e chama `montar_periodos`. `turnos_via_autoridade` chama o mesmo `parear_turnos` com os parâmetros `AUT_*` do motor. O adaptador emite `(entrada, saída ou None)` e descarta a saída órfã como período. Depois, `_periodos_pelo_marco` pode substituir os períodos do dia pelas bordas da ata, se elas passarem pelas guardas. A lista `_turnos_do_juiz` que o motor monta à parte serve ao mapa de dias e ao crédito do turno aberto. (`app/ponto/turnos.py:2896–2904`, `app/ponto/services/fechamento.py:244–262,286–291,623–633`, `app/ponto/motor_calculo_v2.py:453–531,558–576,611–645,669–688,867–895,2346–2354,2396,2511–2527`, `app/ponto/adaptador_motor.py:28–45`)

Há também o escritor `retratar_exportada`, cujo padrão é `versao='motor'`. Ele atualiza `minutos_realizados` pela mesma grade. Nas linhas existentes, a atualização deixa `horas_noturnas` com o valor anterior. Nas linhas que cria, inicializa a rubrica em zero. (`app/ponto/services/dia_pago.py:405,461–495,533–545`)

## E 19:00 → S 01:07 → E 01:2x

O trecho até `01:07` tem **367 minutos de relógio**, dos quais **187** caem entre `22:00` e `01:07`, tomando os horários fornecidos no segundo zero. O destino desses 187 minutos depende de como o motor representa a `S 01:07`. (`app/ponto/turnos.py:605–622,734–741`, `app/ponto/motor_calculo_v2.py:98–118`)

| Ramo do código | `minutos_realizados` de D | `horas_noturnas` de D |
|---|---|---|
| **`01:07` fecha um período do motor.** A `E 01:2x` fica sem saída. | O trecho fechado de 367 min entra no realizado se a geometria da grade o atribui a D. A grade desconta eventual pausa aplicável, e o escritor limita o resultado ao previsto. (`app/ponto/turnos.py:719–771,1491–1498`, `app/ponto/services/dia_pago.py:386–402`) | Com o par efetivo íntegro e sem pausa noturna descontável, grava **`187 / 52,5 ≈ 3,5619 h`**, ou **`187 / 60 ≈ 3,1167 h`** com a hora reduzida afastada. A entrada aberta contribui zero. (`app/ponto/motor_calculo_v2.py:46,214–215,983–1005,2750–2765,2792`, `app/ponto/services/dia_pago.py:173–179`) |
| **`01:07` e `01:2x` são absorvidas como pausa e o período do motor fica aberto.** | `_fim_fechado` recupera `01:07` dentro do turno aberto. O realizado parte dos mesmos 367 min, sujeito ao desconto de pausa aplicável e ao limite do previsto. (`app/ponto/turnos.py:609–622,734–771,1356–1375`, `app/ponto/services/dia_pago.py:386–402`) | **`0,0 h` desse período.** `calcular_periodo` retorna com os minutos noturnos zerados quando `saída=None`. O crédito posterior recalcula o trecho fechado, mas só transfere `minutos_trabalhados` e `minutos_fora_janela` para o período aberto. `lavrar` lê `minutos_noturnos_legais` desse período. **É aqui que o primeiro trecho pode entrar em `minutos_realizados` e ficar fora de `horas_noturnas`.** (`app/ponto/motor_calculo_v2.py:214–215,2750–2755,2877–2882,3215–3220,2541–2560`, `app/ponto/services/dia_pago.py:179`) |

**Com uma `S` posterior barrada pelo teto**, `parear_turnos` mantém a entrada corrente aberta e separa a `S` como órfã. O adaptador não cria período para ela. Se o primeiro trecho permanece fechado separadamente, vale o primeiro ramo da tabela. Se ele só existe dentro do turno aberto, vale o segundo. O teste do teto usa o tempo desde a **entrada corrente** e as bordas disponíveis para ela. Com as duas pontas a mais de três horas dos marcos, o fecho passa a usar o envelope de 26 horas. (`app/ponto/turnos.py:66–82,1083–1095,1430–1457`, `app/ponto/motor_calculo_v2.py:1931,2541–2560`, `app/ponto/adaptador_motor.py:28–44`)

**Qual ramo ocorreu e quais valores ficaram gravados para D:** [nao verificado]. O anexo aponta as funções de geometria e cálculo. A ata de D, seus marcos, o previsto, a régua e o instante da eventual `S` posterior são [nao verificado]. (`app/docs/JUIZES.md:20–29`)

## Leitores de `horas_noturnas` fora do motor

| Saída | Leitura |
|---|---|
| **Espelho diário, calendário e PDF diário** | `rubricas_do_dia` lê `DiaPago.horas_noturnas`. O espelho entrega `dia.rubricas.noturnas` ao HTML. O calendário chama o mesmo helper. O PDF lê a rubrica para a coluna `NOTURNO`. (`app/ponto/services/dia_pago.py:901–925`, `app/ponto/services/espelho.py:831`, `app/templates/ponto/espelho.html:278–279`, `app/colaboradores/services/calendario.py:688–689`, `app/relatorios/pdf_espelho.py:844,857–858,910–913`) |
| **Total do espelho, PDF e app** | `somar_periodos` lê `p.horas_noturnas`. Para o resumo da competência, `folha_manda` aplica o total do fechamento lido por `eventos_do_fechamento`, ou a soma do `DiaPago` quando a lavratura é a fonte disponível. HTML, PDF e API consomem `total_noturnas`. (`app/ponto/services/espelho.py:99,1320`, `app/relatorios/cartao_pela_celula.py:69–76,90–100,142–176,321–346`, `app/templates/ponto/espelho.html:59`, `app/relatorios/pdf_espelho.py:501–502,628,782`, `app/api/views.py:1361–1366,1431,1662`) |
| **Extrato parcial e CSV** | Soma `DiaPago.horas_noturnas` no intervalo via `soma_do_periodo`. A view entrega o valor à tela e ao CSV. (`app/ponto/services/dia_pago.py:34–35,784–789`, `app/relatorios/views.py:481–487,524,551–562`, `app/templates/relatorios/extrato_parcial.html:74,92`) |
| **Horas do mês, fechamento e painel** | O relatório mensal lê `FechamentoMensal.horas_noturnas` no CSV, no total e no HTML. A linha e o painel de fechamento leem o mesmo atributo. A apuração individual de folha também o expõe pela API de mensageria. (`app/relatorios/views.py:94,108`, `app/templates/relatorios/horas_mes.html:68`, `app/templates/ponto/_fech_linha.html:31`, `app/templates/ponto/partials/painel_fechamento.html:33`, `app/folha/services/ranking_he.py:225`, `app/api/views_mensageria.py:1661–1669`) |
| **TXT Domínio, prévia e PDF de validação** | `eventos_do_fechamento` lê `fech.horas_noturnas` como `adicional_noturno`. `montar_linhas_export` resolve a rubrica cadastrada e emite a linha. A prévia e o PDF de validação usam esse mesmo montador. O gate de homologação também lê o evento. (`app/folha/export.py:447,657,730–739,1002–1012`, `app/folha/views.py:270`, `app/folha/validacao_pdf.py:66`) |

O campo também é lido na comparação do cartão com o TXT e na comparação da soma lavrada com o total do espelho. `pago_do_dia` o consulta entre as rubricas que indicam se há valor de trabalho sustentado pela linha, e a fotografia E4B captura o campo junto com os demais campos do modelo. (`app/relatorios/cartao_pela_celula.py:249–268`, `app/folha/porta_export.py:245–274`, `app/ponto/services/dia_pago.py:861–864,890–898`, `app/chamados/services/foto_e4b.py:247–252,289–290`)

## 23/11 15:53 · sessao 61a10b6c2207 · PAR-PELO-TURNO · leitura · origem=Code

```python
from collections import Counter, defaultdict
from copy import copy
from datetime import date, datetime, time, timedelta
from itertools import groupby
from types import SimpleNamespace

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from escala.servico_jornada import eh_turno_partido_do_tipo
from escala.utils import marcos_dna_periodo
from folha.export import competencia_entregue
from ponto import motor_calculo_v2 as motor
from ponto.janelas import janela_atual
from ponto.models import DiaPago
from ponto.turnos import batidas_apuraveis, intervalos_do_turno

INI = date(2026, 10, 1)
ATE = date(2026, 11, 22)
UM_DIA = timedelta(days=1)
H22 = time(22)
H05 = time(5)

ROTULOS = {
    "C1": (
        "DiaPago dia/motor da competencia de D com minutos_realizados > 0 "
        "E horas_noturnas == 0 E ao menos uma batida apuravel em hora local "
        "em [22:00,24:00) de D ou [00:00,05:00) de D+1"
    ),
    "C2": (
        "DiaPago dia/motor da competencia de D com minutos_realizados > 0 "
        "E horas_noturnas == 0 E ao menos um turno fechado da autoridade "
        "com data_turno = D E minutos_noturnos liquidos dos intervalos > 0"
    ),
}


def dias_entre(ini, fim):
    return (ini + i * UM_DIA for i in range((fim - ini).days + 1))


def perfil_do_vinculo(ec, celulas):
    tipo = ec.tipo_escala if ec is not None else None
    classe = motor.Motor12x36
    if tipo is not None:
        if tipo.tipo_ciclo == "intermitente":
            classe = motor.MotorIntermitente
        else:
            classe = motor.MOTOR_POR_BASE[tipo.tipo_base]
            if (
                issubclass(classe, motor.MotorTurnoPartido)
                and tipo.hora_inicio is not None
                and tipo.hora_fim is not None
                and not eh_turno_partido_do_tipo(tipo)
            ):
                # Comercial e 12x36ComEscala herdam os mesmos AUT_*.
                classe = motor.MotorBase

    return SimpleNamespace(
        escala=tipo,
        _ec=ec,
        celulas_alimentadas=celulas,
        AUT_MARCOS_INTERVALO=classe.AUT_MARCOS_INTERVALO,
        AUT_INTRA_MAX_S=classe.AUT_INTRA_MAX_S,
        AUT_CONT_MAX_S=classe.AUT_CONT_MAX_S,
        DUPLICATA=classe.DUPLICATA,
    )


def noturno_por_dia(dias, vinculos, celulas, batidas_por_data, candidatos):
    resultado = {}
    for ec, grupo in groupby(dias, key=vinculos.__getitem__):
        fatia = tuple(grupo)
        alvos = candidatos.keys() & set(fatia)
        if not alvos:
            continue

        perfil = perfil_do_vinculo(ec, celulas)
        batidas = [
            copy(b)
            for d in dias_entre(fatia[0] - UM_DIA, fatia[-1] + UM_DIA)
            for b in batidas_por_data.get(d, ())
        ]
        batidas_limpas = motor.MotorBase.remover_duplicatas(perfil, batidas)

        for turno in motor.turnos_via_autoridade(perfil, batidas_limpas):
            d = turno.data_turno
            if d not in alvos or turno.aberto or turno.entrada is None or turno.saida is None:
                continue

            entrada, saida = turno.entrada.timestamp, turno.saida.timestamp
            bruto = motor.minutos_noturnos(entrada, saida, prorrogacao=True)
            pausa = motor.minutos_noturnos_dos_intervalos(
                entrada, saida, intervalos_do_turno(turno), prorrogacao=True
            )
            liquido = bruto - pausa
            if liquido > 0:
                resultado[d] = max(liquido, resultado.get(d, 0.0))
    return resultado


def marcos_da_celula(cid, d, ec, celulas):
    celula = celulas.get((cid, d))
    if celula is None or not (celula.dna or {}).get("marcos"):
        return ()
    if ec is not None:
        return ec.marcos_do_dia(d, celulas=celulas)
    return marcos_dna_periodo(cid, d, d, celulas=celulas).get(d, ())


def contar():
    dias = tuple(dias_entre(INI, ATE))
    inicio_batidas = timezone.make_aware(datetime.combine(INI - UM_DIA, time.min))
    fim_batidas = timezone.make_aware(datetime.combine(ATE + UM_DIA, time.max))
    competencias = {}
    exportadas = {}
    contadores = {codigo: Counter() for codigo in ROTULOS}
    quebras = {"marcos=sim", "marcos=nao"}
    amostras = []
    total_colabs = 0
    total_dias_pagos = 0

    colabs = (
        Colaborador.objects.select_related("empresa")
        .only("id", "empresa", "empresa__dia_inicio_competencia")
        .order_by("pk")
    )
    for colab in colabs.iterator(chunk_size=500):
        total_colabs += 1
        cid, empresa_id = colab.pk, colab.empresa_id
        quebra_empresa = f"empresa_id={empresa_id}"
        quebras.add(quebra_empresa)

        if empresa_id not in competencias:
            competencias[empresa_id] = {
                d: janela_atual(d, colab.empresa)[1] for d in dias
            }
        fim_comp = competencias[empresa_id]

        linhas = DiaPago.objects.filter(
            colaborador_id=cid, data__range=(INI, ATE),
            tipo=DiaPago.TIPO_DIA, versao="motor",
        ).values_list("data", "mes", "ano", "minutos_realizados", "horas_noturnas")
        candidatos = {}
        for d, mes, ano, realizado, noturno in linhas:
            if (mes, ano) == (fim_comp[d].month, fim_comp[d].year):
                total_dias_pagos += 1
                if realizado > 0 and noturno == 0:
                    candidatos[d] = realizado
        if not candidatos:
            continue

        batidas_por_data = defaultdict(list)
        dias_relogio = set()
        for batida in batidas_apuraveis(colab, inicio_batidas, fim_batidas):
            local = timezone.localtime(batida.timestamp)
            d, hora = local.date(), local.time()
            batidas_por_data[d].append(batida)
            if hora >= H22:
                dias_relogio.add(d)
            elif hora < H05:
                dias_relogio.add(d - UM_DIA)
        if not batidas_por_data:
            continue

        celulas = celulas_do_periodo([cid], INI, ATE)
        escalas = escalas_do_periodo(
            cid, INI, ATE, mais={c.escala_geradora_id for c in celulas.values()}
        )
        vinculos = {d: vinculo_do_dia(cid, d, escalas, celulas=celulas) for d in dias}
        pares = noturno_por_dia(dias, vinculos, celulas, batidas_por_data, candidatos)

        for d in sorted(candidatos.keys() & (dias_relogio | pares.keys())):
            ec = vinculos[d]
            tipo = ec.tipo_escala if ec is not None else None
            base = tipo.tipo_base if tipo is not None else "sem_vinculo"
            ciclo = tipo.tipo_ciclo if tipo is not None else "sem_vinculo"
            marcos = marcos_da_celula(cid, d, ec, celulas)
            toca = any(h is not None and (h >= H22 or h < H05) for h in marcos)

            mes, ano = fim_comp[d].month, fim_comp[d].year
            chave_exportacao = (empresa_id, mes, ano)
            if chave_exportacao not in exportadas:
                exportadas[chave_exportacao] = competencia_entregue(*chave_exportacao)

            chaves = (
                quebra_empresa,
                f"tipo_base={base} tipo_ciclo={ciclo}",
                f"marcos={'sim' if toca else 'nao'}",
                f"competencia={mes:02d}/{ano} exportada={'sim' if exportadas[chave_exportacao] else 'nao'}",
            )
            quebras.update(chaves)
            if d in dias_relogio:
                contadores["C1"].update(("TOTAL", *chaves))
            if d in pares:
                contadores["C2"].update(("TOTAL", *chaves))
                if len(amostras) < 40:
                    texto_marcos = ",".join(
                        h.isoformat(timespec="minutes") if h is not None else "-"
                        for h in marcos
                    ) or "-"
                    amostras.append(
                        f"AMOSTRA C2 col{cid:03d} {d} {base} {ciclo} "
                        f"{texto_marcos} {pares[d]} {candidatos[d]}"
                    )

    return total_colabs, total_dias_pagos, contadores, sorted(quebras), amostras


def main():
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ")
            cursor.execute("SET TRANSACTION READ ONLY")
            cursor.execute(
                "SELECT current_database(), current_schema(), "
                "current_setting('transaction_read_only')"
            )
            if cursor.fetchone() != ("sombra", "juliani", "on"):
                raise RuntimeError("A medicao exige sombra/juliani em READ ONLY")

        colabs, dias_pagos, contadores, quebras, amostras = contar()

    print(f"UNIVERSO {INI}..{ATE} colabs={colabs} dias_colab_com_DiaPago_motor={dias_pagos}")
    for codigo, rotulo in ROTULOS.items():
        contagem = contadores[codigo]
        print(f"{codigo} ({rotulo}) total={contagem['TOTAL']}")
        for chave in quebras:
            print(f"{codigo} {chave} total={contagem[chave]}")
    for amostra in amostras:
        print(amostra)
    print("FIM")


main()
```

- `Colaborador.objects`, `empresa_id` e corte por empresa. `app/colaboradores/models.py:46`, `app/colaboradores/models.py:283`, `app/colaboradores/models.py:321`. `janela_atual` retorna a janela que contém D. O mês e o ano do fim identificam a competência. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.
- `DiaPago.objects` usa o filtro de tipo e versão da porta de exportação. A unicidade é por colaborador, competência, data e versão. `app/folha/porta_export.py:284`, `app/ponto/models.py:311`, `app/ponto/models.py:327`, `app/ponto/models.py:351`, `app/ponto/models.py:385`. `competencia_entregue` lê a exportação vigente por empresa, mês e ano. `app/folha/export.py:556`, `app/folha/export.py:580`.
- `batidas_apuraveis` faz a seleção e ordena por timestamp. `timezone.make_aware` e `timezone.localtime` seguem o fuso configurado. `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:1980`, `app/ponto/turnos.py:45`, `app/config/settings/base.py:93`.
- `celulas_do_periodo`, `escalas_do_periodo` e `vinculo_do_dia` alimentam a quebra de `tipo_base` e `tipo_ciclo`. A quebra de marcos testa cada hora declarada na 4-tupla da célula em `[22:00,24:00)` ou `[00:00,05:00)`. A amostra imprime essa tupla na ordem `hi,hf,hii,hfi`. `app/escala/alimentacao.py:54`, `app/escala/alimentacao.py:163`, `app/escala/alimentacao.py:228`, `app/escala/models.py:117`, `app/escala/models.py:170`, `app/escala/models.py:1109`, `app/escala/utils.py:84`.
- `turnos_via_autoridade` recebe `batidas_limpas` de `MotorBase.remover_duplicatas` e os atributos do perfil. O script usa fatias contíguas pelo vínculo de D e cópias das batidas com um dia civil de contexto em cada ponta. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:897`, `app/ponto/motor_calculo_v2.py:2351`, `app/ponto/motor_calculo_v2.py:3593`, `app/ponto/motor_calculo_v2.py:3640`, `app/escala/servico_jornada.py:159`, `app/ponto/turnos.py:1946`, `app/ponto/turnos.py:1984`, `app/ponto/services/fechamento.py:239`.
- `intervalos_do_turno`, `minutos_noturnos` e `minutos_noturnos_dos_intervalos` medem o líquido do par com a prorrogação padrão das duas funções. Quando mais de um par qualifica em D, a amostra mostra o maior líquido e a contagem soma um dia. A equivalência com `medir()` está [nao verificado]. `app/ponto/turnos.py:312`, `app/ponto/turnos.py:502`, `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:121`.
- `schema_context`, `transaction.atomic` e `connection.cursor` seguem o padrão local de transação somente leitura. O Python tem o formato esperado pelo `--rodar`. Nessa cópia, o launcher consulta a produção, importa a lavra no banco sombra e grava arquivos fora da transação do script. Execução contra o banco e contagens da frota [nao verificado]. `bin/gate_cobranca.sh:20`, `app/chamados/tests/cef_c3_sonda.py:21`, `app/config/settings/sombra.py:47`, `bin/sombra.sh:136`, `bin/sombra.sh:151`, `bin/sombra.sh:157`, `bin/sombra.sh:483`.

## 23/11 15:51 · sessao 0558da3094ac · - · - · origem=Ronald (tmux)

Para C2, `turnos_via_autoridade` aceita um `SimpleNamespace` com `escala`, `_ec`, `celulas_alimentadas` e as três constantes `AUT_*`. O perfil usa os valores de `MOTOR_POR_BASE`, a precedência do intermitente e o ajuste de geometria do partido contínuo. `MotorBase.remover_duplicatas(MotorBase, copias)` executa a limpeza sem instanciar o motor. [app/ponto/motor_calculo_v2.py:453](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:453>), [app/ponto/motor_calculo_v2.py:897](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:897>), [app/ponto/motor_calculo_v2.py:2351](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:2351>), [app/ponto/motor_calculo_v2.py:3593](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:3593>), [app/ponto/motor_calculo_v2.py:3626](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:3626>).

Separe por segmentos consecutivos de vínculo de D, use contexto civil de um dia em cada ponta e retenha apenas o `data_turno` do segmento. Exija entrada e saída. Subtraia os `intervalos_do_turno` com `minutos_noturnos_dos_intervalos`, usando o mesmo parâmetro `prorrogacao` no bruto e no desconto. O comportamento do `medir()` referido permanece [nao verificado]. [app/ponto/turnos.py:1946](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/turnos.py:1946>), [app/ponto/services/fechamento.py:239](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/services/fechamento.py:239>), [app/ponto/turnos.py:312](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/turnos.py:312>), [app/ponto/turnos.py:502](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/turnos.py:502>), [app/ponto/motor_calculo_v2.py:82](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:82>), [app/ponto/motor_calculo_v2.py:121](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/app/ponto/motor_calculo_v2.py:121>).

O wrapper invoca `lavra_de_prod` antes do Python solicitado e grava arquivos e snapshots nesse caminho. O script pode proteger suas próprias consultas com a transação solicitada. [bin/sombra.sh:144](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:144>), [bin/sombra.sh:151](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:151>), [bin/sombra.sh:157](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:157>), [bin/sombra.sh:483](</home/ronald/codex_ws/f82d53543f49cbcdc92893694536c3e3a28a65bf/bin/sombra.sh:483>).

## 23/11 15:51 · sessao 1e56280ea7e7 · - · - · origem=Ronald (tmux)

- O script pode usar `schema_context("juliani")` e `transaction.atomic(durable=True)`, emitir `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY` antes da primeira consulta e conferir banco, schema e modo. `bin/sombra_mascara_pessoal.py:7-16`, `app/chamados/tests/cef_c3_sonda.py:22`, `app/config/settings/sombra.py:47-51`.
- O `--rodar` desta cópia importa uma lavra de produção antes de executar o script, grava arquivos e faz `update_or_create` na sombra. A restrição de somente leitura exige outra entrada para executar o mesmo Python. O launcher ainda usa `exec(open(...).read())`, então a chamada a `main()` deve ser incondicional. `bin/sombra.sh:136-166`, `bin/sombra.sh:483-493`.
- Carregue células e escalas em lote, passando os IDs das geradoras em `mais=`, e resolva `vinculo_do_dia` por colaborador e data. Para “célula tem marcos”, `marcos_dna_periodo` devolve os quatro marcos congelados. `EC.marcos_do_dia` devolve os mesmos quatro quando existe DNA e cai no template vivo na ausência. A quebra pode testar a presença de um marco pontual na faixa como interpretação explícita. `app/escala/alimentacao.py:54-78`, `app/escala/alimentacao.py:105-160`, `app/escala/alimentacao.py:228-286`, `app/escala/models.py:1109-1144`, `app/escala/utils.py:84-108`.
- A autoridade recebe `batidas_limpas` de `MotorBase.remover_duplicatas`, geometria do perfil de `get_motor`, `_ec` e `celulas_alimentadas`. Fatias contíguas com halo de um dia permitem aplicar o vínculo de D e preservar a borda à meia-noite. Copie batidas por pareamento porque a autoridade coloca marcas transitórias nos objetos. O intervalo liquidável vem de `intervalos_do_turno`. `app/ponto/motor_calculo_v2.py:453-506`, `app/ponto/motor_calculo_v2.py:897-912`, `app/ponto/motor_calculo_v2.py:2351-2354`, `app/ponto/motor_calculo_v2.py:3626-3705`, `app/ponto/services/fechamento.py:239-262`, `app/ponto/turnos.py:1517-1535`, `app/ponto/turnos.py:502-546`.
- O anexo fornecido não traz `medir()`, de modo que opção exata de prorrogação e equivalência com aquele método são [nao verificado]. Para 22h–05h estritas, passe `prorrogacao=False` às duas funções puras. `app/ponto/motor_calculo_v2.py:82-121`.

## 23/11 15:50 · sessao c066df8bb348 · - · - · origem=Ronald (tmux)

- A competência de D é o mês e ano do fim retornado por `janela_atual(D, colab.empresa)`. O recorte pode alcançar dezembro, conforme o corte de cada empresa. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`.
- Consulte `DiaPago` por colaborador, intervalo de datas, `tipo=DiaPago.TIPO_DIA` e `versao='motor'`. Retenha somente a linha com mês e ano iguais à competência de D antes de calcular o universo. `app/folha/porta_export.py:284`, `app/ponto/models.py:311`, `app/ponto/models.py:327`, `app/ponto/models.py:351`, `app/ponto/models.py:386`.
- `competencia_entregue(empresa_id, mes, ano)` devolve o booleano de exportação vigente e aceita cache do resultado por tripla. `app/folha/export.py:556`, `app/folha/export.py:580`, `app/folha/export.py:594`.
- A autoridade acionada pela sonda lê uma borda de um dia em cada ponta e as fatias usam `data_local` para filtrar suas batidas. `app/ponto/services/espelho.py:1055`, `app/ponto/services/fechamento.py:239`.
- O pareador direto recebe o vínculo por `_ec` e o mapa por `celulas_alimentadas`. Preserve instâncias independentes de batidas quando rodar perfis sobrepostos. `app/ponto/management/commands/aplicar_09_corte_b.py:131`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/turnos.py:160`, `app/ponto/turnos.py:178`.
- O noturno líquido de relógio usa `max(0.0, minutos_noturnos(e, s, prorrogacao=p) - minutos_noturnos_dos_intervalos(e, s, intervalos, prorrogacao=p))`. A correspondência com `medir()` da sonda está [nao verificado]. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:121`, `app/ponto/motor_calculo_v2.py:983`.
- Para identificar marcos da célula, `marcos_dna_periodo` retorna o DNA congelado. `EscalaColaborador.marcos_do_dia` possui fallback para template, inclusive quando há célula sem marcos. `app/escala/utils.py:84`, `app/escala/models.py:1109`, `app/escala/models.py:1135`.
- `bin/sombra.sh --rodar` localiza o script por basename em `logs/sombra/`, usa o shell Django no schema e envolve a saída em `tee`. As credenciais da sombra são configuradas no wrapper. `bin/sombra.sh:79`, `bin/sombra.sh:184`, `bin/sombra.sh:483`, `app/config/settings/sombra.py:49`.

## 23/11 15:49 · sessao 057c9d4c1879 · PAR-PELO-TURNO · sonda · origem=Code

A sonda imprimiu **5 `a`, 0 `b` e 24 `c`**. Pelas evidências do anexo e pelos critérios pedidos, a triagem fica em **27 `a`, 0 `b` e 2 `c`**. Das **22 divergências**, **13** têm par separado pelo teto no próprio dia e **9** têm entrada apurável aberta no próprio dia, sem saída final no `turno`. As outras **7** classificações são sustentadas. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` `app/ponto/models.py:327` `app/ponto/models.py:351`

**Competência exportada:** **0 de 29 dias-colab**, abrangendo **0 dos 14 casos**, nas linhas `competencia` fornecidas. **NAO RESOLVIDO:** nenhum. Cada caso tem `RESOLVIDO n=1` com um único PK, reproduzido abaixo como `colNNN`. `app/folha/export.py:556` `app/ponto/models.py:315`

### Conferência por dia-colab

`E→S` indica os papéis no pareamento, identificados pelo PK. `Vão × teto` está em minutos, com até duas casas decimais. As órfãs vêm de `turnos_contagem` e podem incluir dias vizinhos. `DiaPago` traz `minutos_realizados / horas_noturnas` da versão `motor` em D, salvo quando D−1 está indicado. `app/ponto/turnos.py:43` `app/ponto/turnos.py:183` `app/ponto/turnos.py:1447` `app/ponto/motor_calculo_v2.py:503` `app/ponto/models.py:319` `app/ponto/models.py:327` `app/ponto/models.py:351`

| Caso | Colab | D | Classe impressa | Motivo impresso | Evidência do pareamento | DiaPago motor | Exportada | Conferência |
|---|---|---|---|---|---|---|---|---|
| caso01 | col129 | 2026-10-11 | c | `L-099_sem_batida_noturna_alvo` | Recusado `124339→124645`, **780,06 × 732**. D−1 fechou `123883→124025`, vão impresso [nao verificado], teto 732. Órfãs `123274, 124645, 125077`. | `0 / 0` (1414808). D−1 `492 / 8,8`. | Não · 10/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-10-25 | c | `L-099_sem_batida_noturna_alvo` | Recusado `137026→137305`, **780,29 × 732**. D−1 fechou `136544→136691`, 539,55 × 732. Órfãs `135921, 137305`. | `0 / 0` (2400115). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-11-01 | c | `L-099_sem_batida_noturna_alvo` | Recusado `143430→143722`, **780,01 × 732**. D−1 fechou `142959→143112`, 539,77 × 732. Órfãs `142323, 143722`. | `0 / 0` (2400121). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-11-02 | c | `L-099_sem_batida_noturna_alvo` | Recusados `144111→144456` em D, **779,01 × 732**, e `143430→143722` em D−1, 780,01 × 732. Órfãs `143112, 143722, 144456`. | `0 / 0` (2400122). D−1 `0 / 0`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D e S `143722` órfã atribuída a D. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` |
| caso01 | col129 | 2026-11-08 | c | `L-099_sem_batida_noturna_alvo` | Recusado `149397→149717`, **780,29 × 732**. D−1 fechou `148929→149080`, 540,23 × 732. Órfãs `148307, 149717`. | `0 / 0` (2400127). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso01 | col129 | 2026-11-15 | c | `L-099_sem_batida_noturna_alvo` | Recusado `155814→156153`, **779,99 × 732**. D−1 fechou `155325→155481`, 539,98 × 732. Órfãs `154676, 156153`. | `0 / 0` (2400134). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-10-04 | c | `L-099_sem_batida_noturna_alvo` | Recusado `118190→118479`, **782,25 × 732**. D−1 fechou `117743→117877`, 543,34 × 732. Órfãs `117099, 118479`. | `0 / 0` (1419406). D−1 `492 / 8,8`. | Não · 10/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-10-18 | c | `L-099_sem_batida_noturna_alvo` | Recusado `130455→130747`, **783,99 × 732**. D−1 fechou `130003→130139`, 540,31 × 732. Órfãs `129385, 130747`. | `0 / 0` (1419420). D−1 `492 / 8,8`. | Não · 10/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-10-25 | c | `L-099_sem_batida_noturna_alvo` | Recusado `137024→137304`, **781,02 × 732**. D−1 fechou `136542→136683`, 538,67 × 732. Órfãs `135924, 137304`. | `0 / 0` (2450136). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-11-01 | c | `L-099_sem_batida_noturna_alvo` | Recusado `143429→143724`, **781,71 × 732**. D−1 fechou `142958→143111`, 543,33 × 732. Órfãs `142326, 143724`. | `0 / 0` (2450143). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso02 | col155 | 2026-11-15 | c | `L-099_sem_batida_noturna_alvo` | Recusado `155813→156154`, **782,82 × 732**. D−1 fechou `155324→155479`, 539,11 × 732. Órfãs `154673, 156154`. | `0 / 0` (2450157). D−1 `492 / 8,8`. | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D. `app/ponto/turnos.py:183` |
| caso03 | col121 | 2026-11-03 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=145213, S=None`. Internas `S145367/E145385`. Comparação fora da fatia `143660→143828`, 312,64 × 1560, aceita. Órfã `143649`, fora da fatia. | `354 / 0` (2453972) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D. Os alvos noturnos aparecem como internos e a saída final está ausente. `app/ponto/turnos.py:312` |
| caso04 | col502 | 2026-10-17 | c | `L-099_sem_batida_noturna_alvo` | Fechado `129546→129953`, **565,97 × 740**, com `129953` gravada como `E` e usada como saída. Internas `S129743/S129805`. D+1 tem comparação aceita `130188→130268`, 178,41 × 1560, e E `130283` aberta. Órfãs `[]`. | `440 / 0` (1529039) | Não · 10/2026 | **Sustentada.** Em D, `abertas_e_orfas=(False, False)` e `minutos_noturnos_relogio=0`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso05 | col868 | 2026-10-02 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=117058, S=None`. Internas `S117059/E117060`. Sem `teto_comparado` no bloco. Órfãs `[]`. | `367 / 0` (1413722) | Não · 10/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso06 | col174 | 2026-11-11 | c | `L-099_criterios_a_b_ausentes_na_janela` | Recusado `152378→152727`, **722 × 720** na linha `par_separado_pelo_teto`. Decimal de `teto_comparado` [nao verificado]. Internas `S152566/E152577`. Órfã `152727`. | `420 / 0` (2470743) | Não · 11/2026 | **Diverge → a.** A saída final está na lista de órfãs e o par separado está explícito. `app/ponto/turnos.py:183` |
| caso07 | col612 | 2026-11-11 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=152381, S=None`. Internas `S152546/E152562`. Comparação fora da fatia `150586→150650`, 279,61 × 1560, aceita. Órfã `150567`, fora da fatia. | `378 / 0` (2450464) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso08 | col999 | 2026-11-09 | c | `L-099_sem_batida_noturna_alvo` | `E=150272, S=None` no vínculo 1569, `na_fatia=True`. D+1 fechou `150789→151169`, 540,18 × 780, com tipos gravados `S/E`. Órfã `149108`, fora da fatia. | `0 / 0` (2434305) | Não · 11/2026 | **Diverge → a.** `turno` do próprio D fica aberto no vínculo de D. `app/ponto/turnos.py:312` `app/escala/alimentacao.py:228` |
| caso08 | col999 | 2026-11-10 | c | `L-099_sem_batida_noturna_alvo` | Fechado `150789→151169`, **540,18 × 780**, com tipos gravados `S/E` e internas `E150858/S150916`. E `150272` aberta em D−1. D+1 fechou `151739→152239`, 551,62 × 780. Órfãs `[]`. | `470 / 0` (2434306) | Não · 11/2026 | **Sustentada.** Em D, `abertas_e_orfas=(False, False)` e `minutos_noturnos_relogio=0`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso09 | col992 | 2026-11-03 | c | `L-099_criterios_a_b_ausentes_na_janela` | Recusado `145230→145574`, **721,94 × 720**. Internas `S145380/E145394`. Comparação fora da fatia `143666→143759`, 294,74 × 1560, aceita. Órfãs `143652, 145574`. | `367 / 0` (2535747) | Não · 11/2026 | **Diverge → a.** `par_separado_pelo_teto` em D, com S `145574` órfã. `app/ponto/turnos.py:183` |
| caso10 | col490 | 2026-10-28 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=140120, S=None`. Internas `S140345/E140351`. Comparação fora da fatia `138327→138452`, 165,99 × 1560, aceita. Órfã `138319`, fora da fatia. | `495 / 0` (2452929) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso10 | col490 | 2026-11-01 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=143507, S=None`. Internas `S143678/E143686`. Comparação fora da fatia `142287→142356`, 175,12 × 1560, aceita. Órfã `142279`, fora da fatia. | `484 / 0` (2452931) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-04 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=118383, S=None`. Comparações aceitas `117003→117074` 337,79 × 1560, `117003→117213` 422,88 × 1560, `118456→118477` 50,00 × 1560 e `118578→119321` 950,67 × 1560. Órfãs `[]`. | `0 / 0` (1406259) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=118383` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-14 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=127324, S=None`. Comparações vizinhas aceitas `125753→126275` 475,61 × 840 e `127416→127422` 60,77 × 1560. Órfãs `[]`. | `0 / 0` (1406268) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=127324` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-16 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=129246, S=None`. Comparações vizinhas aceitas `127416→127422` 60,77 × 1560 e `129342→129348` 69,43 × 1560. Órfãs `[]`. | `0 / 0` (1406270) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=129246` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso11 | col788 | 2026-10-28 | c | `L-099_sem_batida_noturna_alvo` | `E=140245, S=None`. Comparações vizinhas aceitas `138680→139201` 455,64 × 840 e `140352→140360` 51,23 × 1560. Órfãs `[]`. | `0 / 0` (2542626) | Não · 11/2026 | **Diverge → a.** `turno` de D aberto com E `140245` às 21:33. O par com 22,87 min noturnos tem `data_turno=2026-10-29`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso11 | col788 | 2026-11-01 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | `E=143603, S=None`. D+1 tem comparações aceitas `143682→143689` 66,49 × 1560, `143682→144086` 766,02 × 1560 e `144090→144298` 268,03 × 1560. Órfãs `[]`. | `0 / 0` (2542629) | Não · 11/2026 | **Sustentada.** `batida_alvo pk=143603` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso12 | col516 | 2026-10-30 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=142071, S=None`. Internas `S142228/E162536`. Comparação fora da fatia `140300→140413`, 371,92 × 1560, aceita. Órfã `162534`, fora da fatia. | `303 / 0` (2528760) | Não · 11/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |
| caso13 | col820 | 2026-10-16 | a | `batida_noturna_em_E_aberta_ou_S_orfa` | Em D, E `129248` e E `128591` abertas. Comparações vizinhas aceitas `127945→128276` 414,99 × 720 e `129345→129355` 65,04 × 1560. Órfãs `[]`. | `0 / 0` (1404220) | Não · 10/2026 | **Sustentada.** `batida_alvo pk=129248` tem papel `E_aberta` em D. `app/ponto/turnos.py:312` |
| caso14 | col467 | 2026-10-11 | c | `L-099_criterios_a_b_ausentes_na_janela` | `E=124392, S=None`. Internas `S124547/E124563`. Comparação fora da fatia `123191→123287`, 311,48 × 1560, aceita. Órfã `123173`, fora da fatia. | `330 / 0` (1415304) | Não · 10/2026 | **Diverge → a.** `turno` aberto em D, com alvos internos e sem saída final. `app/ponto/turnos.py:312` |

A leitura de `a` inclui a entrada apurável sem saída final em D, a mesma situação registrada como `E_aberta` nos cinco `a` impressos. Nas nove divergências por abertura, a causa da falta de fechamento é [nao verificado]. Para `b`, nenhum bloco combina par **do próprio D** com `minutos_noturnos_relogio > 0`, `minutos_realizados > 0` e `horas_noturnas = 0`. Nos dois `c` sustentados, o par de D é diurno. Onde D está aberto, a própria linha `turno` informa `sem_par_E_S`. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` `app/ponto/models.py:327` `app/ponto/models.py:351`

### Dono L-099 dos `c` impressos

**BATIDA** é o dono sugerido quando o `turno` de D tem entrada aberta e o PK aparece em `batidas_apuraveis`. Essa é a evidência local que corresponde à guarda de batida solta do juiz. O veredito do juiz em PROD é [nao verificado]. O leitor dos pares separados também consulta esse juiz para determinar o dono. `app/ponto/management/commands/e6_oraculo.py:98` `app/ponto/management/commands/e6_oraculo.py:182` `app/ponto/management/commands/e6_oraculo.py:198` `app/ponto/services/teto_na_lista.py:86`

| `c` impresso | Dono sugerido | Linha da saída que sustenta |
|---|---|---|
| caso01 · col129 · seis datas | **BATIDA** | `turno` em D com `S=None` e E `124339` (10-11), `137026` (10-25), `143430` (11-01), `144111` (11-02), `149397` (11-08) e `155814` (11-15). Cada E consta em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso02 · col155 · cinco datas | **BATIDA** | `turno` em D com `S=None` e E `118190` (10-04), `130455` (10-18), `137024` (10-25), `143429` (11-01) e `155813` (11-15). Cada E consta em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso03 · col121 · 11-03 | **BATIDA** | `turno` em D `E=145213, S=None`, com `145213` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso04 · col502 · 10-17 | **não decide** | `turno` em D `129546→129953`, fechado e com `minutos_noturnos_relogio=0`. Dono [nao verificado]. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso05 · col868 · 10-02 | **BATIDA** | `turno` em D `E=117058, S=None`, com `117058` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso06 · col174 · 11-11 | **BATIDA** | `turno` em D `E=152378, S=None`, com `152378` em `batidas_apuraveis`. `par_separado_pelo_teto` identifica a S `152727`. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso07 · col612 · 11-11 | **BATIDA** | `turno` em D `E=152381, S=None`, com `152381` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso08 · col999 · 11-09 | **BATIDA** | `turno` do vínculo 1569 em D `E=150272, S=None, na_fatia=True`, com `150272` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso08 · col999 · 11-10 | **não decide** | `turno` em D `150789→151169`, fechado e com `minutos_noturnos_relogio=0`. A E `150272` aberta tem `data_turno=2026-11-09`. Dono [nao verificado]. `app/ponto/turnos.py:312` `app/ponto/motor_calculo_v2.py:983` |
| caso09 · col992 · 11-03 | **BATIDA** | `turno` em D `E=145230, S=None`, com `145230` em `batidas_apuraveis`. `par_separado_pelo_teto` identifica a S `145574`. `app/ponto/turnos.py:183` `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso10 · col490 · 10-28 e 11-01 | **BATIDA** | `turno` em D com `S=None` e E `140120` (10-28) ou `143507` (11-01). Ambas constam em `batidas_apuraveis` nos respectivos blocos. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso11 · col788 · 10-28 | **BATIDA** | `turno` em D `E=140245, S=None`, com `140245` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso12 · col516 · 10-30 | **BATIDA** | `turno` em D `E=142071, S=None`, com `142071` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |
| caso14 · col467 · 10-11 | **BATIDA** | `turno` em D `E=124392, S=None`, com `124392` em `batidas_apuraveis`. `app/ponto/turnos.py:312` `app/ponto/turnos.py:2877` |

Hash da cópia e implementação da seleção de alvos em `sonda.py` [nao verificado].

## 23/11 15:44 · sessao f124818f214d · PAR-PELO-TURNO · sonda · origem=Code

A saída fornecida contém **2 casos com `CAUSA c` e 14 `NAO RESOLVIDO`**. Nos dois casos com `CAUSA`, o motivo impresso é sustentado pelos turnos diurnos, mas **a classe diverge do critério literal do aval**. O `DiaPago` motor tem realizado positivo e noturno zero, o que preenche **b**. Os outros 15 dos 31 casos declarados estão [nao verificado]. Linhas `CAUSA`, `turno` e `DiaPago` da saída; `app/ponto/motor_calculo_v2.py:82`, `app/ponto/models.py:327`, `app/ponto/models.py:351`.

### Triagem por dia-colab

Os pares abaixo são os do dia `D`, identificados por pk e horário local. A coluna de evidência usa `minutos_realizados` em minutos e `horas_noturnas` em horas, ambos da versão `motor`. Linhas `turno`, `teto_comparado` e `DiaPago` da saída; `app/ponto/turnos.py:43`, `app/ponto/models.py:322`, `app/ponto/models.py:325`, `app/ponto/models.py:351`.

| Caso | colNNN (pk) | D | Classe impressa → auditada | Motivo | Evidência |
|---|---|---|---|---|---|
| caso01 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso01 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso02 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso02 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso03 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso03 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso04 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso04 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso05 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso05 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso06 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso06 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso07 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso07 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso08 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso08 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso09 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso09 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso10 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso10 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso11 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso11 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso12 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso12 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso13 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso13 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| caso14 | [nao verificado] | [nao verificado] | NAO RESOLVIDO | `NAO RESOLVIDO caso14 n=0` | E/S, vão × teto, órfãs, DiaPago e exportação [nao verificado] |
| col214 | col214 (214) | 2026-11-15 | **c → b · DIVERGÊNCIA** | `L-099_sem_batida_noturna_alvo` é sustentado por `b_dia_E_S=[]` e 0 min noturnos nos dois turnos. O gravado `440 min / 0 h` preenche b literal. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/models.py:327`, `app/ponto/models.py:351`. | E155565 07:00:20 → S155668 12:06:40 **306,33 × 1560 min**. E155704 13:06:42 → S155775 15:20:54 **134,20 × 740 min**. Ambos aceitos; órfãs `[]`. `DiaPago` motor 2476612 **440 min / 0 h**. 11/2026 **exportada não**. Linhas `teto_comparado`, `turno`, `turnos_contagem`, `DiaPago` e `competencia`; `app/ponto/turnos.py:1447`, `app/ponto/models.py:351`, `app/folha/export.py:556`. |
| col107 | col107 (107) | 2026-11-15 | **c → b · DIVERGÊNCIA** | `L-099_sem_batida_noturna_alvo` é sustentado por `b_dia_E_S=[]` e 0 min noturnos nos dois turnos. O gravado `382 min / 0 h` preenche b literal. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/models.py:327`, `app/ponto/models.py:351`. | E155585 07:05:19 → S155607 09:15:09 **129,83 × 1560 min**. E155627 10:13:25 → S155739 14:25:11 **251,76 × 750 min**, via `pos_pausa_sem_volta`. Ambos aceitos; órfãs `[]`. `DiaPago` motor 2441257 **382 min / 0 h**. 11/2026 **exportada não**. Linhas `teto_comparado`, `turno`, `turnos_contagem`, `DiaPago` e `competencia`; `app/ponto/turnos.py:1183`, `app/ponto/turnos.py:1447`, `app/ponto/models.py:351`, `app/folha/export.py:556`. |

Cada dia tem dois turnos fechados. As linhas `turno` trazem `_separada_pelo_teto=None` e `abertas_e_orfas=(False, False)`, e `evidencias_causa` traz `a_pares_recusados=[]` e `a_abertas_ou_orfas=[]`. Que os dois turnos de cada dia fossem uma única jornada separada fica [nao verificado]. A classe **b** aqui é a aplicação do critério literal. Noturno devido e omitido fica [nao verificado]. `app/ponto/turnos.py:183`, `app/ponto/turnos.py:312`, `app/ponto/motor_calculo_v2.py:82`.

### Totais

| Contagem no trecho fornecido | a | b | c | NAO RESOLVIDO |
|---|---:|---:|---:|---:|
| Impressa nas linhas `CAUSA` e `NAO RESOLVIDO` | 0 | 0 | 2 | 14 |
| Auditada pelo critério literal, com os dois `DiaPago` motor (`app/ponto/models.py:327`, `app/ponto/models.py:351`) | 0 | 2 | 0 | 14 |

- **Competência EXPORTADA:** 0 sim, 2 não. As duas linhas `competencia` mostram `status='aberto' exportada=False exportacao_pk=None`. A situação dos outros 29 casos e o total nos 31 ficam [nao verificado]. `app/folha/export.py:556`.
- **NAO RESOLVIDO:** `caso01` a `caso14`, todos com `n=0` nas respectivas linhas da saída. O status dos 15 casos ausentes fica [nao verificado].

### Dono L-099 para os dois `c` impressos

- **col214 — não decide.** `teto_comparado` para E155565/S155668 mostra `teto_do_dia=False` com marcos `12:40–21:00`. É indício de **CADASTRO**, pois o pareador usa o envelope quando ambas as pontas estão fora da régua da L-084. A inclusão de `D` em `dias_cadastro_x_realidade`, insumo do juiz L-099, fica [nao verificado]. `app/ponto/turnos.py:1435`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/management/commands/e6_oraculo.py:162`.
- **col107 — não decide.** `teto_comparado` para E155585/S155607 mostra `teto_do_dia=False` com marcos `12:50–21:20`. É o mesmo indício de **CADASTRO**. A inclusão de `D` em `dias_cadastro_x_realidade` fica [nao verificado]. `app/ponto/turnos.py:1435`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/management/commands/e6_oraculo.py:162`.

## 23/11 15:42 · sessao 727a1b162816 · PAR-PELO-TURNO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

_7964 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
