# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 12:40** · publicado em **11/11/2026 12:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 12:40 · sessao 3898b92fbd5f · FIO-E0-V4-E4B · sonda · origem=Code

Não reexecutei a sonda: seu fonte não foi localizado na cópia. Abaixo está a saída raspada fornecida, integralmente; execução, contagens e PKs sem verificação independente. [não verificado]

```text
sonda_leitura: sonda_exportada_viva.py [75c00e700ea78f78] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

ancoradas_nao_validadas_em_chamado_encerrado n 22246
perguntas ('aberta', 'morta') 9655
perguntas ('aberta', 'viva') 1913
perguntas ('comp', 'aberta', '2026-04') 20
perguntas ('comp', 'aberta', '2026-05') 35
perguntas ('comp', 'aberta', '2026-06') 3
perguntas ('comp', 'aberta', '2026-09') 446
perguntas ('comp', 'aberta', '2026-10') 1134
perguntas ('comp', 'aberta', '2026-11') 275
perguntas ('comp', 'exportada', '2026-05') 3
perguntas ('comp', 'exportada', '2026-06') 17
perguntas ('comp', 'exportada', '2026-08') 420
perguntas ('comp', 'exportada', '2026-09') 706
perguntas ('exportada', 'morta') 9532
perguntas ('exportada', 'viva') 1146
chamados_encerrados_com_viva exportada 422 [1225, 1367, 1544, 1583, 1620, 1693, 1694, 1695, 1712, 1836, 2385, 2388, 2586, 2587, 2588, 2765, 5037, 5623, 5715, 8986]
chamados_encerrados_com_viva aberta 745 [1203, 2350, 2525, 2765, 2834, 5037, 5894, 8591, 9185, 9535, 9551, 9687, 9748, 10072, 10970, 12920, 14296, 15630, 16014, 16025]
chamados_encerrados_com_viva indecidivel 0 []
```

EXPORTADA: 422 chamados encerrados com pergunta viva, conforme dado fornecido. [não verificado]  
ABERTA: 745 chamados encerrados com pergunta viva, conforme dado fornecido. [não verificado]  
INDECIDÍVEL: 0 chamados, conforme dado fornecido. [não verificado]  
EXPORTADA — meses de `data_evento`: 2026-05, 2026-06, 2026-08, 2026-09. [não verificado]  
ABERTA — meses de `data_evento`: 2026-04, 2026-05, 2026-06, 2026-09, 2026-10, 2026-11. [não verificado]

## 11/11 12:39 · sessao 73d538fff093 · - · - · origem=Ronald (tmux)

Q4 — contraexemplos condicionais que permanecem após o diff:

- **Triagem online, `getUserMedia` pendente:** o confirmar a batida nasce oculto (`app/templates/ponto/triagem/apto.html:93`); `abrirCamera()` oculta abrir câmera e aguarda `getUserMedia` sem timeout (`:148-150`); a liberação ocorre depois da captura (`:182-185`) ou no catch (`:156-163`). Uma promessa que nunca resolve/rejeita não alcança nenhum desses caminhos. Ocorrência em produção: [nao verificado].
- **Triagem online, `toBlob` sem callback:** `confirmarFoto()` esconde vídeo/confirmar selfie (`app/templates/ponto/triagem/apto.html:175-179`), mas libera o botão de batida somente no callback `toBlob` (`:182-186`). O diff altera `confirmarFotoOffline`, mantendo esse trecho online. Ocorrência em produção: [nao verificado].
- **Legado, câmera pendente:** `bloco-botoes` começa `display:none` (`app/templates/ponto/bater_ponto.html:246`); `abrirCameraPre` aguarda câmera sem timeout (`:354-356`). O novo catch não cobre promessa pendente. Contudo, a view atual renderiza v2 ou triagem, sem selecionar esse legado (`app/ponto/views.py:356-358`; seleção da triagem em `:225-235`).
- **v2, falha assíncrona da conversão:** `capturarFrame` põe `done=true` antes de tentar `toDataURL` no callback (`app/templates/ponto/bater_ponto_v2.html:265`), e chama `toDataURL` também fora de proteção no watchdog (`:267`). Se callback assíncrono recebe `null` e `toDataURL` lança, a promessa pode permanecer pendente, e `done=true` impede o watchdog de resolvê-la (`:263-267`). O novo `.catch()` só cobre rejeição. Ocorrência real desse cenário: [nao verificado].
- **`pwa.js` indisponível:** ambas as cascas incluem o arquivo com `defer`, o que comprova inclusão, sem comprovar carregamento (`app/templates/base.html:20`; `app/templates/base_app.html:20`). A persistência offline depende de `window.JulianiPWA.registrarBatidaOffline` na v2 (`app/templates/ponto/bater_ponto_v2.html:333`, `:346`), legado (`app/templates/ponto/bater_ponto.html:543`, `:597`) e triagem (`app/templates/ponto/triagem/apto.html:282`). O diff mantém essa dependência; seu isolamento do aviso da v2 não fornece outra implementação da fila. Falha de carregamento efetiva: [nao verificado].

Telas de fluxo não tocadas:

| Tela | Função e classe |
|---|---|
| `app/templates/ponto/triagem/turno_confirmar.html:8` | POST de nova entrada, `tipo=E`, `confirmar=sim`; sem câmera/foto nesse formulário (`:10-17`). Sem a barreira de selfie; envio depende da rede, sem fila local no template (`:8-24`). |
| `app/templates/ponto/triagem/turno_regularizar.html:19` | Regularização, `tipo=regularizar_saida` (`:21`), horários de saída (`:29-31`); sem câmera. Selecionada por `app/ponto/views.py:229-230`. |
| `app/templates/ponto/triagem/fora_horario.html:20` | Solicita anuência (`:22`), não captura selfie. Selecionada por `app/ponto/views.py:231-232`. |
| `app/templates/ponto/regularizar_retorno.html:23` | Regularização com horário e motivo (`:28`, `:35`), sem câmera nesse formulário (`:23-57`). |
| `app/templates/ponto/materializar_saida_retroativa.html:38` | Criação administrativa de saída retroativa, horário e motivo (`:43`, `:50`), sem câmera nesse formulário (`:38-58`). |

Nas buscas por `getUserMedia`, `registrarBatidaOffline`, `name="foto"` e `capture=` em `templates/ponto/**`, só as três telas tocadas têm captura de selfie; completude diante de nomes dinâmicos não identificáveis por esses termos: [nao verificado]. A view encaminha diretamente a confirmação para `turno_confirmar.html` (`app/ponto/views.py:233-234`), que não entra no mapa `TELAS_SELFIE` do anexo; esse mapa não é um censo de todos os formulários de batida [nao verificado].

Fora do escopo estrito de telas de batida, existe o gate de onboarding: colaborador incompleto é redirecionado antes da tela (`app/ponto/views.py:243-244`); onboarding tem selfie (`app/templates/core/onboarding.html:53-63`), câmera sem timeout (`:154`), `toBlob` sem watchdog (`:190-200`) e `salvarFoto()` retorna sem foto (`:212-213`). A inclusão disso no aval é [nao verificado].

Q5 — **na árvore há um chamador, não zero:** definição de `estadoPermissao` em `app/templates/ponto/bater_ponto_v2.html:281`, chamada em `:288`. Busca `rg -n --hidden 'estadoPermissao' .` encontrou somente essas duas ocorrências; nenhuma em static/testes [nao verificado]. O diff remove definição e esse único chamador; zero remanescentes após aplicação é previsão estática, aplicação/execução [nao verificado].

## 11/11 12:39 · sessao e28afac356d2 · - · - · origem=Ronald (tmux)

Auditoria de fila/foto, somente leitura:

- **Foto nula já é contrato da fila antes do diff.** `registrarBatidaOffline` recebe `fotoBlob` e grava `foto: fotoBlob || null`; a transação IndexedDB conclui antes do retorno. Portanto o diff muda os produtores das telas, sem inaugurar `null` no formato armazenado. `app/static/pwa.js:107`, `app/static/pwa.js:113`, `app/static/pwa.js:118`, `app/static/pwa.js:120`.
- **Leitor da fila tolera ausência.** `sincronizarBatidas` lê todos os registros, ordena pelo `timestamp`, e só acrescenta multipart `foto` quando `b.foto` é truthy. O POST sem foto segue ao mesmo endpoint `/api/ponto/bater/`. `app/static/pwa.js:126`, `app/static/pwa.js:132`, `app/static/pwa.js:137`, `app/static/pwa.js:152`, `app/static/pwa.js:153`.
- **Formato exigido quando a foto existe:** `form.append('foto', b.foto, 'selfie.jpg')` usa o valor como Blob/File, sem conversão de dataURL. Um dataURL truthy armazenado nessa propriedade não tem adaptador nesse leitor; `null` é pulado. Os produtores atuais escrevem Blob de `canvas.toBlob`, File do input, ou Blob convertido de dataURL, e não a string do preview. `app/static/pwa.js:152`; `app/templates/ponto/triagem/apto.html:174`, `app/templates/ponto/triagem/apto.html:182`, `app/templates/ponto/triagem/apto.html:183`; `app/templates/ponto/bater_ponto.html:395`, `app/templates/ponto/bater_ponto.html:430`, `app/templates/ponto/bater_ponto.html:443`; `app/templates/ponto/bater_ponto_v2.html:252`, `app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:265`.
- **POST direto também é condicional.** V2 só adiciona `_fotoBlob` ao FormData quando existe; a tela antiga faz o mesmo; apto só preenche input de arquivo quando `_fotoBlob` existe, e depois submete o formulário. `app/templates/ponto/bater_ponto_v2.html:320`, `app/templates/ponto/bater_ponto.html:531`, `app/templates/ponto/triagem/apto.html:229`, `app/templates/ponto/triagem/apto.html:235`.
- **Servidor recebe ausência como ausência de arquivo**, via `request.FILES.get('foto')`; o modelo admite foto opcional. API usa `if not foto` para consultar `selfie_ausente` e acrescentar motivo/contexto, enquanto anexação só ocorre com foto. Não há veto por ausência nesse ramo. `app/api/views_core.py:423`, `app/api/views_core.py:436`, `app/api/views_core.py:448`, `app/api/views_core.py:451`, `app/api/views_core.py:776`; gêmeo `app/api/views.py:436`, `app/api/views.py:449`, `app/api/views.py:461`, `app/api/views.py:464`, `app/api/views.py:781`; `app/ponto/models.py:78`.
- **Escritor persistente de foto é a porta existente:** `anexar_foto` escreve `Batida.foto` por `anotar`, opcionalmente cria/atualiza `FotoBatida`. Ausência não chama essa função no endpoint de bater. Upload separado exige arquivo e, depois de anexá-lo, resolve disputas. `app/ponto/registro_batida.py:296`, `app/ponto/registro_batida.py:305`, `app/ponto/registro_batida.py:307`, `app/ponto/registro_batida.py:311`; `app/api/views_core.py:835`, `app/api/views_core.py:844`, `app/api/views_core.py:854`.
- **Leitores posteriores conhecidos toleram ausência:** cron compõe `tem_foto = bool(FotoBatida existente ou b.foto)` para o juiz; dashboard e detalhe usam condicionais antes de `.url`. `app/ponto/management/commands/marcar_foto_ausente_retro.py:70`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; `app/core/views.py:1005`; `app/colaboradores/services/detalhe.py:87`; `app/templates/core/_d2_feed.html:6`.
- **`foto_ausente` é motivo de disputa, não campo booleano da batida.** As APIs acrescentam a string em `motivos_anomalia` e um objeto em `contexto_anomalia`; cron cria/atualiza disputa com esse motivo; upload separado procura disputas abertas contendo a string e a remove/fecha. `app/api/views_core.py:451`, `app/api/views_core.py:452`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/api/foto_recebida.py:61`, `app/api/foto_recebida.py:70`, `app/api/foto_recebida.py:72`, `app/api/foto_recebida.py:80`.

Sincronização e unidades:

- SW **não lê foto nem IndexedDB** no tratador de sync: se há clientes, transmite `SYNC_BATIDAS`; sem clientes, oferece notificação para abrir o app. A página recebe a mensagem e chama `sincronizarBatidas`. `app/static/sw.js:112`, `app/static/sw.js:116`, `app/static/sw.js:119`, `app/static/sw.js:122`; `app/static/pwa.js:15`.
- Outros gatilhos de sync são evento `online` e inicialização com navegador online; exige meta `user-perfil=colaborador`. `app/static/pwa.js:128`, `app/static/pwa.js:320`, `app/static/pwa.js:328`.
- `resp.ok` apaga o item; HTTP 400 também apaga como erro definitivo; falha de rede/5xx deixa para nova tentativa. Ausência de foto em si não é condição de descarte no cliente. `app/static/pwa.js:158`, `app/static/pwa.js:162`, `app/static/pwa.js:166`, `app/static/pwa.js:172`, `app/static/pwa.js:177`.
- Relógio/gap: escritor calcula ISO corrigido com `Date.now()+gap`, grava também ISO bruto, `timestamp_gap_ms`, coordenadas, `gps_accuracy`, `gps_captured_at`. Gap é calculado em milissegundos. Sync envia somente o ISO bruto dentre os timestamps; `timestamp` serve para ordenar, e gap/accuracy/captured_at não são acrescentados ao formulário. Essa lacuna preexiste ao diff. `app/static/pwa.js:85`, `app/static/pwa.js:91`, `app/static/pwa.js:109`, `app/static/pwa.js:115`, `app/static/pwa.js:117`, `app/static/pwa.js:137`, `app/static/pwa.js:143`, `app/static/pwa.js:147`.
- API passa o timestamp bruto ao juiz `hora_do_aparelho`, com `agora` injetado; o juiz aceita passado até 48h e, fora disso, usa `agora`, conservando a hora lida em outro retorno. Portanto o ISO corrigido salvo no IndexedDB não é a hora enviada por esse sincronizador. `app/api/views_core.py:496`, `app/api/views_core.py:505`, `app/api/views_core.py:507`; `app/api/hora_do_aparelho.py:124`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:146`, `app/api/hora_do_aparelho.py:153`.

Contraexemplos estáticos que sobrevivem ao diff:

- **Apto online, callback `toBlob` nunca chamado:** botão principal só é liberado dentro do callback de `confirmarFoto`, trecho não alterado pelo diff. Execução com esse callback pendente deixa o fluxo sem botão liberado; incidência concreta [nao verificado]. `app/templates/ponto/triagem/apto.html:182`, `app/templates/ponto/triagem/apto.html:185`.
- **V2, timeout de captura também pode não resolver:** em `capturarFrame`, timeout avalia `cv.toDataURL(...)` antes de chamar o conversor protegido por `try`. Se `toBlob` não chama e `toDataURL` lança no callback do timeout, a promessa não resolve/rejeita naquele callback; o `.catch` proposto em `garantirRequisitos` não resolve uma promessa ainda pendente. Incidência concreta [nao verificado]. `app/templates/ponto/bater_ponto_v2.html:257`, `app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:265`, `app/templates/ponto/bater_ponto_v2.html:267`.
- **PWA ausente, fila indisponível:** todos os caminhos de gravação offline chamam diretamente `window.JulianiPWA.registrarBatidaOffline`; timeout do aviso da v2 isola o aviso, mas não oferece um escritor alternativo da fila. `app/templates/ponto/bater_ponto_v2.html:333`, `app/templates/ponto/bater_ponto_v2.html:346`; `app/templates/ponto/bater_ponto.html:543`, `app/templates/ponto/bater_ponto.html:597`; `app/templates/ponto/triagem/apto.html:282`.

## 11/11 12:39 · sessao b1e127efcd57 · - · - · origem=Ronald (tmux)

Servidor e predicados, somente leitura:

- `selfie_ausente` já é juiz **da cobrança**, registrado como “a foto da batida está ausente?”, não juiz booleano da admissibilidade. Predicado: `origem in ('app',) and not tem_foto and decorrido_min >= prazo`; PWA=0min, APK/NULL=90min, override só para APK/NULL. Não lê banco nem relógio; recebe os insumos. (`app/ponto/registro_batida.py:64-91`; `app/core/juizes.py:1192-1195`)
- `canal_da_batida` deriva `apk` se `str(device_id or '').strip()` for não vazio, senão `pwa`; o canal é gravado na Batida. (`app/ponto/registro_batida.py:53-61`; `app/api/views.py:665-678`; `app/api/views_core.py:657-670`)
- O chokepoint `registrar_batida` não testa foto: seus retornos impeditivos são competência/colisão; dedup devolve existente. Cria a Batida sem exigir foto. (`app/ponto/registro_batida.py:94-161`)
- `Batida.foto` aceita `null=True, blank=True`; `FotoBatida.foto` é uma imagem num registro opcional separado ligado por OneToOne. (`app/ponto/models.py:78`; `app/ponto/models.py:1026-1031`)

API, ambas cascas:

| Responsabilidade | `views.py` | `views_core.py` |
|---|---|---|
| Lê multipart `request.FILES.get('foto')` | `:436` | `:423` |
| Sem foto consulta juiz com origem app, canal derivado, `tem_foto=False`, decorrido=0; acrescenta motivo/contexto `foto_ausente` | `:449-465` | `:436-452` |
| Grava batida pela porta | `:665-680` | `:657-672` |
| Encaminha motivos/contexto à porta de disputa, após gravação | `:767-779` | `:762-774` |
| Só se foto existe chama `anexar_foto(...registro='criar')` | `:781-787` | `:776-782` |

- A API sem foto não retorna erro por selfie; ela cobra PWA imediatamente e APK depois do prazo. O upload **separado de foto** exige foto e batida_id, mas isso não é a porta da batida. (`app/api/views.py:449-465`; `app/api/views.py:847-859`; `app/api/views_core.py:832-844`)
- A rota `/api/ponto/bater/` usa `views.api_bater_ponto`; a casca core importa `views_core`. (`app/api/urls.py:2`; `app/api/urls.py:67`; `app/api/urls_core.py:5`)

POST web triagem:

- `views.bater_ponto` recebe POST E/S e delega a `ramo_registrar_es`; foto ausente não é condição de veto. (`app/ponto/views.py:268-273`; `app/ponto/views.py:346-354`)
- O ramo web grava origem `app`, canal derivado/PWA pela mesma porta; só depois lê `foto_sessao or request.FILES.get("foto")` e anexa se truthy. **Não consulta `selfie_ausente` no ato.** (`app/ponto/services/triagem_batida.py:700-713`; `app/ponto/services/triagem_batida.py:748-751`)
- O comentário confirma que a porta web não passa pelo detector de anomalias; ausência será cobrável pelo retro, que lê origem app/canal PWA gravados. Não afirmar disputa imediata para esse caminho. (`app/ponto/services/triagem_batida.py:761-765`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:66-77`)

Escritores/leitores no servidor:

- `anexar_foto` é escritor comum: `registro=None` grava só `Batida.foto`; `criar` cria FotoBatida antes e grava Batida.foto; `atualizar` grava Batida.foto e update_or_create FotoBatida. (`app/ponto/registro_batida.py:296-312`)
- `marcar_foto_ausente_retro` captura relógio na borda `agora=timezone.now()`, busca candidatas e fotos em lote, fornece `tem_foto=bool(FotoBatida existe or b.foto)` e minutos ao juiz. Em `--apply`, encaminha `['foto_ausente']` e contexto pela porta de disputa. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:58-77`; `:89-104`)
- A porta de disputa recebe motivos/contexto, normaliza a lista e chama `_abrir_ou_agregar_disputa`; gera perguntas na criação e atualização. (`app/chamados/services/disputa_emissao.py:634-670`; `:682`)
- O catálogo lê `foto_ausente` como pergunta de confirmação de presença no posto, não upload compulsório. (`app/chamados/catalogo/perguntas.py:93-98`)
- Upload separado chama `resolver_disputas_de_foto`: procura disputas abertas contendo motivo, exige `contexto.ultima_batida.id == batida.pk`, apara só motivo quando restam outros ou fecha a disputa. (`app/api/foto_recebida.py:57-85`)
- `salvar_foto_batida` aceita foto apenas se a Batida ainda não tem foto; usa a porta. (`app/ponto/views.py:2893-2905`)

Sobre `semSelfie` proposto: o corpo do diff apenas retorna texto, não importa/consulta o juiz registrado nem transmite um resultado de admissibilidade às telas. Classificá-lo como leitor do juiz existente seria incorreto: é nova fonte textual do aviso, enquanto o controle de liberar/impedir permanece nos tratadores/guardas locais; a cobrança continua autoridade de `selfie_ausente`. (anexo `diff_v1.patch`, hunk `app/static/pwa.js @@ -104,6 +104,18`; `app/ponto/registro_batida.py:80-91`; `app/core/juizes.py:1192-1195`)

Não rodei testes nem consultei banco/log; resultado de execução e efeito em produção: [nao verificado]. [nao verificado]

## 11/11 12:32 · sessao 94104672075f · PRONTO-AUSENTE-RASCUNHO · leitura · origem=Code

### E4B
1. O censo da obra precede sua abertura. -- fonte: avais_obras.txt:7: "CENSO-ANTES-DA-VAGA em toda abertura"
2. O backfill usa a porta canônica e tem DRY como padrão. -- fonte: backlog_obras.txt:1887: "pela porta canonica, DRY default"
3. Os atos legados são conservados por pergunta, com uma transação por unidade, idempotência e reversão anterior. -- fonte: avais_obras.txt:92: "por unidade = pergunta, uma transacao por unidade, idempotente, reversao antes"
4. DIVERGE, com substituição expressa: pergunta viva não é encerrada pelo chamado em nenhum ramo. -- fonte: avais_obras.txt:63: "pergunta viva NAO se encerra pelo chamado, em ramo nenhum"
5. Quando o fechamento deixa pergunta viva sem chamado vivo que a liste, a porta reabre o chamado com nota. -- fonte: avais_obras.txt:63: "o credor volta pela porta (reabrir com nota)"
6. Antes do apply, uma amostra confirma que o C6 considera vivas as perguntas das 131 por fatos do dia, sem defeito do próprio C6. -- fonte: avais_obras.txt:63: "por fato do dia, e nao por defeito do proprio C6"
7. O passivo dos 45 dias passa por DRY usando a mesma porta. -- fonte: avais_obras.txt:63: "Passivo dos 45 dias: DRY pela mesma porta"
8. Classe e, limbo e ramo sem porta estão curados. -- fonte: avais_obras.txt:28: "classe e, limbo, ramo sem porta curados"
9. O ramo segue_sem_porta tem matriz de transições anterior à construção, contendo ramo, retorno, porta devida e prova. -- fonte: avais_obras.txt:20: "ramo -> retorno -> porta devida -> prova"
10. O backfill da competência 11 está aplicado com DIFF. -- fonte: avais_obras.txt:28: "backfill da 11 aplicado com DIFF" [nao verificado]
11. O apply que move dinheiro da competência 11 tem reversão anterior, exportada intacta e prova de frota. -- fonte: avais_obras.txt:9: "reversao antes, exportada intacta, prova de frota"
12. Competências exportadas ficam em lista para o `!` de Ronald. -- fonte: avais_obras.txt:92: "exportada em lista para o ! do Ronald"
13. A competência 10 só recebe apply, relavra ou rejulgamento com `!`, ou fica listada com DIFF por colaborador. -- fonte: avais_obras.txt:23: "Nenhum apply/relavra/rejulgar na 10 sem o meu `!`"
14. O DIFF anterior ao apply inclui DiaPago, fechamento, DSR e faltas por colaborador, sem centavo inesperado prosseguindo. -- fonte: avais_obras.txt:24: "centavo mudando fora do esperado = PARE com lei:"
15. Antes de apply que reabra chamado, emita pergunta ou dispare push, o RELATO publica o DIFF e as quantidades que nascerão. -- fonte: avais_obras.txt:25: "contando o que nasce (chamados, perguntas, push, itens na Central)" [nao verificado]
16. Relavra ou rejulgamento de frota ocorre fora do horário do admin. -- fonte: avais_obras.txt:25: "ate 07h ou depois das 19h"
17. A remedida posterior à E4a-2 está no RELATO. -- fonte: avais_obras.txt:28: "remedida pos E4a-2 no RELATO"
18. fio_morto_alcance e hoje+ medem zero na sombra em três rodadas de dias diferentes. -- fonte: avais_obras.txt:28: "fio_morto_alcance = 0 E hoje+ = 0 remedidos na sombra em 3 rodadas de dias diferentes"
19. alarma_em_0 está armado no carimbo. -- fonte: avais_obras.txt:28: "alarma_em_0 armado no carimbo"
20. nao ocorreu: pular opus, suite ou B3. -- fonte: avais_obras.txt:29: "pular opus, suite ou B3" [nao verificado]
21. nao ocorreu: baixar contador por universo, filtro, exclusão ou fallback. -- fonte: avais_obras.txt:29: "baixar contador por universo, filtro, exclusao ou fallback"
22. Colaboradores de teste e CADASTRO só saem por ato de Ronald na tela. -- fonte: avais_obras.txt:26: "nunca por codigo nem filtro" [nao verificado]

N=22  
LACUNA: o conteúdo integral de C3.2–C3.9 e da matriz de segue_sem_porta não foi fornecido [nao verificado]. DIVERGE resolvida por substituição: o aval anterior mandava encerrar a pergunta — avais_obras.txt:66: "a pergunta encerra pela porta com motivo premissa morta e trilha"; o posterior declara — avais_obras.txt:63: "substitui por inteiro E4B-CLASSE-E-PELA-LEI e E4B-CLASSE-E-ADMIN".

### E4C
1. Existe censo AST dos leitores dos campos legados, com arquivo:linha e a resposta produzida em tela, PDF ou contador. -- fonte: avais_obras.txt:81: "com o que cada um responde na tela/PDF/contador"
2. Cada leitor tem a autoridade registrada que passará a consultar, e ausência de juiz vira decisão listada. -- fonte: avais_obras.txt:82: "sem juiz = DECISAO listada, nunca juiz novo"
3. O censo usa a cópia da E3 indicada e atualiza somente o delta após seu pouso. -- fonte: avais_obras.txt:80: "refazer SO o delta quando a E3 pousar" [nao verificado]
4. A ordem de troca identifica dependências de arquivo e cruzamentos com E4b/E4d. -- fonte: avais_obras.txt:83: "ordem de troca por dependencia de arquivo e o que cruza com E4b/E4d"
5. O arquivo de censo da E4C está entregue no formato consumido pelo brief. -- fonte: avais_obras.txt:84: "logs/E4C/censo.r, no formato que o brief do construtor consome"
6. O RELATO informa a quantidade de perguntas de lei, inclusive quando zero. -- fonte: avais_obras.txt:87: "se der 0 pergunta de lei, dizer \"0\""
7. A publicação do censo é precedida da conferência por amostra declarada de cinco escritores e cinco leitores. -- fonte: avais_obras.txt:85: "5 escritores e 5 leitores de cada lote antes de publicar"
8. Os leitores DIVERGE da matriz passam a ler o ato somente depois da E4b medida. -- fonte: avais_obras.txt:92: "os DIVERGE da matriz passam a ler o ato, so depois da E4b medida"
9. O selo 2 demonstra ausência de leitura de campo abolido. -- fonte: avais_obras.txt:92: "selo 2 (nenhum leitor le campo abolido)"
10. A03, A05 no ramo de assinatura, A06, A17, A18 e A20 conservam a projeção como fonte até existir fato equivalente. -- fonte: avais_obras.txt:92: "continuam com a projecao como fonte lida ate existir fato equivalente"
11. Há registro de obra de fato próprio para cada exceção e da pergunta de lei da A20. -- fonte: avais_obras.txt:92: "REGISTRAR obra de fato proprio para cada um"
12. nao ocorreu: construção, cópia de escrita ou apply durante o censo adiantado. -- fonte: avais_obras.txt:86: "construtor, copia de escrita, apply"
13. Há opus final da obra. -- fonte: avais_obras.txt:7: "opus final sempre"

N=13  
LACUNA: a matriz completa de leitores DIVERGE e seus mapeamentos de autoridade não foi fornecida [nao verificado].

### RAIZ F1
1. O brief contém F0, F1 e a resposta do aval sobre o eleitor único. -- fonte: avais_obras.txt:98: "A c22 abre com F0+F1 + esta resposta no brief"
2. fio.gravar_destino_da_pergunta é a função única de escrita de chamado_destino, chamada por _nascer e reapontar_resolvedora. -- fonte: avais_obras.txt:98: "funcao unica fio.gravar_destino_da_pergunta"
3. O selo AST por função falha se houver escrita de chamado_destino fora da função única. -- fonte: avais_obras.txt:98: "AST: escrita de chamado_destino fora dela = FALHA"
4. O eleitor prioriza o chamado do fato resolvido pela pergunta, com mesmo motivo e marco. -- fonte: avais_obras.txt:98: "o chamado DO FATO que a pergunta resolve -- mesmo motivo/marco"
5. Empates entre candidatos do mesmo fato são resolvidos pelo menor PK, deterministicamente e com idempotência. -- fonte: avais_obras.txt:98: "menor PK, deterministico e idempotente"
6. A posição no lote do agrupador deixa de eleger o destino. -- fonte: avais_obras.txt:98: "\"Primeiro do lote do agrupador\" (disputa_emissao.py:1197) MORRE como regra"
7. declaracao_texto lê o destino gravado sem elegê-lo. -- fonte: avais_obras.txt:98: "vira LEITOR do destino gravado (testemunha le, nao elege)"
8. disputa_emissao.py:238 chama o eleitor único. -- fonte: avais_obras.txt:98: "disputa_emissao.py:238 vira chamador do eleitor unico"
9. A alteração de _nascer limita-se à chamada da função única, preservando predicado e campos com prova por mutante. -- fonte: avais_obras.txt:98: "predicado e campos intocados; mutante prova"
10. Dois pedidos do mesmo fato em ordens diferentes produzem o mesmo destino. -- fonte: avais_obras.txt:98: "dois pedidos no mesmo fato em ordens diferentes = mesmo destino"
11. Replay com origem None não reelege o destino. -- fonte: avais_obras.txt:98: "replay com origem None nao re-elege"
12. O tratamento da dona FECHADA segue HX-RESSUSCITA. -- fonte: avais_obras.txt:98: "dona FECHADA segue a lei HX-RESSUSCITA"
13. A matriz F1 inclui h2(B), quem fecha órfãos sem disputa e a decisão da dona FECHADA com o número de motivo_em_dona_fechada. -- fonte: backlog_obras.txt:1895: "com o numero do contador motivo_em_dona_fechada"
14. Há opus final da obra. -- fonte: avais_obras.txt:7: "opus final sempre"

N=14  
LACUNA: o conteúdo de HX-RESSUSCITA, a matriz completa e a resposta sobre quem fecha órfãos não foram fornecidos [nao verificado].

### RAIZ F2
1. O censo antecede a cura da BO-DISPUTA-SEM-PERGUNTA. -- fonte: backlog_obras.txt:1889: "cura so com aval depois do censo"
2. A cura contempla exceção e reuso aprovados. -- fonte: backlog_obras.txt:1889: "cura (a) excecao + (b) reuso APROVADA"
3. Existe matriz da porta de nascimento da DisputaSupervisao cobrindo os estados a–j e avaliando resolver antes de escrever. -- fonte: backlog_obras.txt:1890: "avalia resolver-antes-de-escrever"
4. Toda linha da matriz que viola o nascimento com pergunta recebe cura na obra ou pergunta de lei. -- fonte: backlog_obras.txt:1890: "linha que fere \"nasce com pergunta ou nao nasce\" = cura na obra ou pergunta de lei"
5. Há invariante de disputa aberta sem pergunta, acompanhado de selo que morde. -- fonte: backlog_obras.txt:1889: "invariante em auditar_invariantes_chamados ao lado do I7"
6. O contador COM ata tem esperado zero e dono ESTRUTURA. -- fonte: backlog_obras.txt:1889: "COM ata esperado 0 (ESTRUTURA)"
7. O contador SEM ata é publicado sob ata/cartório. -- fonte: backlog_obras.txt:1889: "SEM ata publicado (ata/cartorio)" [nao verificado]
8. nao ocorreu: alteração das sete disputas por shell. -- fonte: backlog_obras.txt:1889: "as 7 nao se mexem por shell"
9. O opus final recebe a matriz anexa. -- fonte: backlog_obras.txt:1890: "opus final so com a matriz anexa"

N=9  
LACUNA: não foram fornecidos os estados a–j, os detalhes das curas de exceção/reuso nem o conteúdo de HX-ATA-NAO-SABE [nao verificado].

### RAIZ F3
1. Existe censo AST de escritores e leitores de cada campo de estado do par e da DisputaSupervisao, dentro e fora do agregado. -- fonte: avais_obras.txt:74: "cada campo de estado do par e da DisputaSupervisao"
2. A matriz por porta contém estado, juiz, operação, persistido, retorno e prova. -- fonte: avais_obras.txt:75: "estado -> juiz -> operacao -> persistido -> retorno -> prova"
3. A matriz cobre responder, validar, materializar, guarda de paridade, reconciliar e fechar disputa. -- fonte: avais_obras.txt:75: "Portas: responder (colab), validar (admin), materializar"
4. As divergências semânticas estão numeradas e acompanhadas da quantidade de perguntas de lei e dos números de frota. -- fonte: avais_obras.txt:76: "DIVERGENCIAS SEMANTICAS como DECISOES numeradas"
5. O censo identifica quais dos 77 pares, 848 duplicadas e 18 pode_fechar True cada transição explica. -- fonte: avais_obras.txt:77: "quais dos 77 pares / 848 duplicadas / 18 pode_fechar True cada transicao explica"
6. O autor do fecho automático é o autor do ato que carimbou a última pergunta, com motivo próprio na trilha. -- fonte: avais_obras.txt:104: "o autor do ato que carimbou a ultima pergunta, com motivo proprio na trilha"
7. O brief de construção contém censo e matriz. -- fonte: avais_obras.txt:107: "abrem construtor com censo + matriz no brief"
8. Há opus final da obra. -- fonte: avais_obras.txt:7: "opus final sempre"

N=8  
LACUNA: as condições completas que autorizam ou impedem cada fecho automático não estão enumeradas nos anexos [nao verificado].

### RAIZ F4
1. Existe censo AST dos escritores e leitores dos campos de estado do par e da DisputaSupervisao. -- fonte: avais_obras.txt:74: "CENSO POR AST de quem ESCREVE e quem LE"
2. Existe matriz por porta com estado, juiz, operação, persistido, retorno e prova. -- fonte: avais_obras.txt:75: "estado -> juiz -> operacao -> persistido -> retorno -> prova"
3. As transições explicam seus respectivos casos entre os 77 pares, 848 duplicadas e 18 pode_fechar True. -- fonte: avais_obras.txt:77: "quais dos 77 pares / 848 duplicadas / 18 pode_fechar True cada transicao explica"
4. O par é lido dos marcos de saída e volta do mesmo intervalo na célula do dia. -- fonte: avais_obras.txt:102: "O par e LIDO dos marcos da celula do dia"
5. A DisputaSupervisao agrupa perguntas pelos marcos que cada uma cobra. -- fonte: avais_obras.txt:102: "a DisputaSupervisao agrupa as perguntas pelos marcos que cada uma cobra"
6. Sem marco de pausa na célula, não existe par e cada pergunta segue sozinha como CADASTRO. -- fonte: avais_obras.txt:103: "nao ha par; cada pergunta segue sozinha, como hoje"
7. Se o censo encontra autoridade existente para identificar o par da pausa, a F4 lê dessa autoridade e registra arquivo:linha. -- fonte: avais_obras.txt:105: "ela e a autoridade e a F4 le dela"
8. nao ocorreu: derivação do par em paralelo à célula. -- fonte: avais_obras.txt:105: "Proibido derivar o par em paralelo a celula"
9. nao ocorreu: criação de juiz par_da_pausa ou registro novo em core/juizes.py. -- fonte: avais_obras.txt:106: "juiz par_da_pausa ou qualquer registro novo em core/juizes.py"
10. nao ocorreu: gravação de “aguardando a outra metade”. -- fonte: avais_obras.txt:106: "gravar \"aguardando a outra metade\" (e derivado, so leitura)"
11. O brief contém censo e matriz, e a obra recebe opus final. -- fonte: avais_obras.txt:7: "opus final sempre"

N=11  
LACUNA: a matriz completa e o conteúdo da única lei identificada como D8 não foram fornecidos; sua menção está em backlog_obras.txt:1896: "so D8 lei" [nao verificado].

### RAIZ F6
1. O passivo é tratado em DRY. -- fonte: avais_obras.txt:10: "F6 PASSIVO: DRY"
2. Existe DIFF da sombra por classe. -- fonte: avais_obras.txt:10: "DIFF da sombra por classe"
3. A reversão está em logs/. -- fonte: avais_obras.txt:10: "reversao em logs/"
4. A competência exportada está em lista para o `!` de Ronald. -- fonte: avais_obras.txt:10: "exportada em LISTA para o meu !"
5. Das 848 duplicadas são medidos somente idade e efeito, sem apply. -- fonte: avais_obras.txt:10: "as 848 duplicadas so medir idade e efeito, sem apply"

N=5  
LACUNA: as classes, os efeitos esperados e o escopo completo da F6 além desse passivo não estão definidos nos anexos [nao verificado].

### RAIZ F7
1. Cada fase nasce acompanhada do selo F7 com esperado zero. -- fonte: avais_obras.txt:5: "F7 (selo esperado 0) nasce junto de CADA fase, nao no fim"

N=1  
LACUNA: não estão nomeados o que o selo mede, seu universo e os critérios específicos de cada fase [nao verificado].

### O168
1. O censo antecede a abertura da obra. -- fonte: avais_obras.txt:7: "O168 e LASTRO parte 3 ainda sem censo: Codex censo antes"
2. Cartório e supra_juiz consultam um único juiz para a guarda de conversa. -- fonte: backlog_obras.txt:1396: "passa a ser chamada pelos DOIS, com UM juiz"
3. No evento, o cartório não fecha cobrança sem furo de chamado que tenha conversa. -- fonte: backlog_obras.txt:1396: "o cartorio nao fecha cobranca sem furo de chamado que tem conversa"
4. O `--executar` do supra só é liberado depois da guarda valer no evento e de CSF_RETRATADO ser medido em zero. -- fonte: backlog_obras.txt:1396: "com `CSF_RETRATADO` medido em ZERO"
5. Há opus final da obra. -- fonte: avais_obras.txt:7: "opus final sempre"

N=5  
LACUNA: nenhuma.

### LASTRO
1. O degrau 2 consulta o juiz _match_marcos/marco_ocupado_pela_batida para determinar o papel do marco. -- fonte: backlog_obras.txt:1732: "o degrau 2 pergunta ao JUIZ"
2. A parte 2 contém a autópsia dos chamados #35623, #33126 e #36045. -- fonte: backlog_obras.txt:1761: "autopsia #35623/#33126/#36045"
3. O fechamento é curado por marco via reconciliar(), e fechar_cobranca_com_lastro passa a vigia `--alarme`. -- fonte: backlog_obras.txt:1761: "cura fecho por MARCO via reconciliar() -> fechar_cobranca_com_lastro vira vigia --alarme"
4. O pouso da parte 2 respeita as dependências JUIZ-1 e CRONS-INSTALL-SOB-MARCA. -- fonte: backlog_obras.txt:1761: "pouso tambem depende de CRONS-INSTALL-SOB-MARCA" [nao verificado]
5. A morte da pergunta por concordância ou recusa não deixa o chamado fechável sem evento. -- fonte: backlog_obras.txt:1763: "pergunta que morre por concordancia/recusa deixa o chamado fechavel SEM evento"
6. O censo da parte 3 antecede sua abertura. -- fonte: avais_obras.txt:7: "O168 e LASTRO parte 3 ainda sem censo: Codex censo antes"
7. O escritor e a frota do tipo folga fora do catálogo são medidos antes da cura. -- fonte: backlog_obras.txt:1808: "Escritor e frota a medir antes de cura"
8. Há opus final da obra. -- fonte: avais_obras.txt:7: "opus final sempre"

N=8  
LACUNA: não há delimitação consolidada do PRONTO de LASTRO nem descrição da cura de folga fora do catálogo [nao verificado].

### O169-FECHADA
1. DIVERGE no nome: o contador cluster_encerrado_sem_lastro mede clusters encerrados cujo dia ainda tem assinatura viva, com esperado zero. -- fonte: avais_obras.txt:132: "`cluster_encerrado_sem_lastro` = cluster ENCERRADO cujo dia ainda tem assinatura viva"
2. O contador usa a função real _leitura_da_celula, sem decidir pelo motivo gravado. -- fonte: avais_obras.txt:132: "contador pela FUNCAO REAL, nao por motivo gravado"
3. O universo do contador contém encerramentos automáticos por cron, varredura ou janela. -- fonte: avais_obras.txt:132: "universo = encerramentos AUTOMATICOS (cron/varredura/janela)"
4. Via humana, desligamento e tranca ficam fora com motivo declarado, dono ADMIN/DP e possibilidade de contestação. -- fonte: avais_obras.txt:132: "ficam FORA com motivo declarado no contador"
5. O mutante que fecha por idade torna o teste vermelho. -- fonte: backlog_obras.txt:1940: "mutante fechar-por-idade = VERMELHO" [nao verificado]
6. No legado trancado, a relavra altera somente a VIA para tranca e deixa trilha. -- fonte: backlog_obras.txt:1940: "trancada relavra so a VIA p/ tranca com trilha"
7. No legado não trancado, o chamado reabre pela porta. -- fonte: backlog_obras.txt:1940: "nao trancada reabre pela porta"
8. O rc 2 diário não é silenciado durante a espera pelo pouso da E4a-2. -- fonte: backlog_obras.txt:1940: "rc 2 diario ate la nao se silencia" [nao verificado]
9. Há opus final da obra. -- fonte: avais_obras.txt:7: "opus final sempre"

N=9  
LACUNA: DIVERGE nominal registrada: backlog_obras.txt:1940: "contador `fechados_por_idade` (esperado 0)"; o aval determina a retirada desse nome — avais_obras.txt:132: "Nome fechados_por_idade morre". Não foi fornecida uma lista consolidada que delimite quais achados separados de O169 integram este delta [nao verificado].

### UI-PAINEL-MOSTRA-O-APP
1. O topo da aba Mensagens mostra a faixa com rótulo e número atuais do app e a lista de perguntas pendentes com texto, dia e marco. -- fonte: avais_obras.txt:117: "a lista das perguntas pendentes (texto da pergunta, dia, marco), so leitura"
2. O contador do botão Mensagens soma as perguntas pendentes. -- fonte: avais_obras.txt:118: "contador do botao da aba Mensagens soma as perguntas pendentes"
3. O painel consulta consulta_do_app para pendentes e histórico. -- fonte: avais_obras.txt:115: "consulta_do_app (pendentes, historico)"
4. Uma única função fornece o rótulo aos dois leitores da API e ao painel. -- fonte: avais_obras.txt:124: "uma funcao de rotulo com 3 leitores (2 API + painel)"
5. A contagem de mensagens não lidas também pertence à função única. -- fonte: backlog_obras.txt:1131: "a contagem de nao lidos tambem vai a funcao unica"
6. Com duas perguntas pendentes, painel e API mostram Responder DP e número dois, e o painel lista ambas. -- fonte: avais_obras.txt:121: "painel mostra \"Responder DP, 2\" e as 2 perguntas"
7. Com somente uma mensagem não lida, painel e API mostram Chamados e número um. -- fonte: avais_obras.txt:121: "\"Chamados, 1\" nos dois"
8. Sem pendência, a faixa mostra nada pend

[truncado em 20000 de 23818 caracteres]

## 11/11 12:32 · sessao 186f5d5b3ede · FIO-E0-V4-E4B · construtor · origem=Code

**PARE no item 2, conforme sua condição.** A recusa S143 grava chamado e pergunta no fio, sem competência; esse destino não atende aos três dados exigidos ([disputa_emissao.py:1854](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1854)).

Há outra lacuna: C7 impede fechar a **disputa** com pergunta viva, mas não devolve ao estado aberto o chamado já encerrado ([models.py:1848](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1848), [signals.py:96](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/signals.py:96)).

O [recibo c5](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c5.txt:1) contém os quatro juízes, escritores/leitores, chamadores e comandos ORM. Registra 17 testes em memória e 40 selos passando; ORM não executado. Cura, RED e mutante novos não foram construídos por causa do PARE; sem commit ([recibo:131](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c5.txt:131)). [nao verificado]

Perguntas ao Ronald:

- Qual destino existente deve receber chamado, pergunta e competência da recusa?
- “Chamado continua ABERTO” significa `ChamadoColaborador` ou a disputa? Se for o chamado, qual porta existente deve impedir seu encerramento, já que a guarda atual exclui exportada ([models.py:364](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:364))?

## 11/11 12:26 · sessao 191db1405fa4 · APP-MENSAGENS-MSG-0 · censo · origem=Code

Censo estático da cópia fornecida, sem alterações. Resultados de testes, funcionamento em execução e confirmação independente dos números de produção: **[nao verificado]**. [nao verificado]

1. **Escritores de justificativa**

O modelo é `ponto.Justificativa`: colaborador, entrada, `data_turno`, tipo, saída informada, descrição, coordenadas, decisão, batida gerada e `chamado_origem`. Os dois vínculos são distintos: a justificativa guarda `chamado_origem`; o chamado guarda `justificativa_relacionada`. (`app/ponto/models.py:136`, `app/ponto/models.py:144`, `app/ponto/models.py:149`, `app/ponto/models.py:154`, `app/ponto/models.py:159`, `app/ponto/models.py:167`, `app/ponto/models.py:173`; `app/chamados/services/vinculos.py:81`)

| Escritor | O que grava | Critério de dia e cobrança | Abertura de chamado/envelope |
|---|---|---|---|
| API: `api_justificativas` | Procura a primeira pendente por colaborador/dia; atualiza `tipo`, `descricao` e origem se nula, ou cria com esses campos e `status='pendente'`. (`app/api/views.py:1994`, `app/api/views.py:1997`, `app/api/views.py:2004`) | Dia ISO recebido. Busca mesmo colaborador, módulos **literais** `turno_aberto`/`batida_ausente`, estados `VIVOS`; compara JSON `data_turno` ou, quando ausente, **dia civil da criação**. Não chama `dia_do_fato` nessa seleção. (`app/api/views.py:1952`, `app/api/views.py:1960`, `app/api/views.py:1967`) | Sem cobrança encontrada, chama `ChamadoColaborador.abrir` para `justificativa_colab`; depois aponta o vínculo inverso pela porta de vínculos. (`app/api/views.py:1977`, `app/api/views.py:2012`) |
| Web: `_abrir_justificativa_fluxo` | Procura a primeira pendente por colaborador/dia; atualiza tipo/descrição, saída se fornecida, entrada/origem se nulas; ou cria esses campos e pendente. (`app/ponto/views.py:1621`, `app/ponto/views.py:1624`, `app/ponto/views.py:1635`) | Dia ISO recebido. Entrada por `timestamp__date` — dia civil. Cobrança por `COBRANCAS_DO_TURNO`, `VIVOS`, mesmo colaborador, menor PK cujo **`dia_do_fato(c)`** coincide. (`app/ponto/views.py:1594`, `app/ponto/views.py:1597`, `app/ponto/views.py:1612`) | Com cobrança, grava origem e aponta o inverso. Sem cobrança, grava origem nula; esse corpo não abre envelope. (`app/ponto/views.py:1643`, `app/ponto/views.py:1645`) |
| Serviço: `regularizar_turno_orfao` | `get_or_create` por colaborador/entrada/dia; defaults `esqueci_saida`, saída, descrição com motivo e pendente. Se existente sem saída, atualiza saída/descrição/status. (`app/ponto/services/regularizacao.py:91`, `app/ponto/services/regularizacao.py:102`) | **Dia civil local da entrada**; valida o turno por `turnos_do_colab`. Não acrescenta ponteiros de justificativa/cobrança. (`app/ponto/services/regularizacao.py:59`, `app/ponto/services/regularizacao.py:73`, `app/ponto/services/regularizacao.py:91`) | Não abre envelope. Põe em análise chamados vivos de cinco módulos do colaborador, **sem recorte de dia**, e anexa ocorrência com seus motivos. (`app/ponto/services/regularizacao.py:126`, `app/ponto/services/regularizacao.py:136`) |
| Serviço da tela de ponto: `ramo_regularizar_saida` | `update_or_create` por colaborador/dia/entrada; defaults `esqueci`, saída informada, descrição automática contendo o horário informado e pendente. (`app/ponto/services/triagem_batida.py:227`, `app/ponto/services/triagem_batida.py:258`) | **Dia civil local da entrada**, `_ent['entrada'].date()`. Não acrescenta vínculo de justificativa. (`app/ponto/services/triagem_batida.py:180`, `app/ponto/services/triagem_batida.py:260`) | Não abre envelope; escala urgência de chamados vivos `entrada_bloqueada_turno_aberto` do colaborador, sem filtro de dia. (`app/ponto/services/triagem_batida.py:287`) |
| Command: `_abrir_justificativa_automatica` | `get_or_create` por colaborador/dia; defaults entrada, `esqueci_saida`, descrição do sistema, pendente e origem. Só aponta o inverso quando criou a justificativa. (`app/ponto/management/commands/processar_alertas_turno.py:392`, `app/ponto/management/commands/processar_alertas_turno.py:394`, `app/ponto/management/commands/processar_alertas_turno.py:409`) | Recebe **`tm.data_turno`**, o dia do turno materializado. (`app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:321`) | Abre antes a cobrança crítica; se a abertura for recusada, não cria justificativa. Não é nascimento de `justificativa_colab`. (`app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:324`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |

A regularização por serviço tem três cascas: web `regularizar_retorno`, API normal e API core. O outro ramo é chamado por `bater_ponto` quando `tipo=='regularizar_saida'`. (`app/ponto/views.py:3215`, `app/api/views.py:2866`, `app/api/views_core.py:1188`; `app/ponto/views.py:333`)

`views_core.py` existe e tem `api_regularizar_turno`; não foi localizado ali um gêmeo `api_justificativas`. O URLconf core publica regularização, mas não justificativas. (`app/api/views_core.py:1143`; `app/api/urls_core.py:7`, `app/api/urls_core.py:19`; ausência de outro escritor: **[nao verificado]**)

Há também a superfície genérica do Django admin: `JustificativaAdmin` herda de `admin.ModelAdmin`, sem o mixin `SoLeitura` usado em outros modelos desse arquivo; não contém lógica própria de escolha do dia, cobrança ou envelope. Uso efetivo dessa superfície: **[nao verificado]**. (`app/ponto/admin.py:12`, `app/ponto/admin.py:18`)

O formulário web lê diretamente `request.POST`; não foi localizado um form Python escritor de justificativa. `ponto/forms.py` contém `AusenciaForm`. (`app/ponto/views.py:1589`; `app/ponto/forms.py:41`; ausência global: **[nao verificado]**)

Os demais escritores encontrados alteram justificativas já existentes:

| Escritor | Mutação |
|---|---|
| `aprovar_justificativa_dp` | Batida gerada, status aprovada, usuário/instante; encerra origem. (`app/ponto/services/justificativa.py:204`, `app/ponto/services/justificativa.py:214`) |
| `rejeitar_justificativa_dp` | Status rejeitada, usuário/instante/motivo; abre chamado módulo **`justificativa`**, diferente do envelope `justificativa_colab`. (`app/ponto/services/justificativa.py:283`, `app/ponto/services/justificativa.py:291`) |
| `materializar_saida_dp` | Batida gerada, saída informada, aprovação, usuário/instante; fecha cascata e origem. (`app/ponto/services/justificativa.py:380`, `app/ponto/services/justificativa.py:393`, `app/ponto/services/justificativa.py:408`) |
| `aprovar_lote_justificativas` | Aprovação direta, usuário/instante, eventual batida gerada; encerra origem. (`app/ponto/views.py:1796`, `app/ponto/views.py:1831`, `app/ponto/views.py:1837`) |
| `rejeitar_lote_justificativas` | Rejeição direta, usuário/instante/motivo. (`app/ponto/views.py:1874`) |
| Entrada sincronizada | Auto-rejeita pendente vinculada e acrescenta nota à descrição após reconciliação. (`app/ponto/services/entrada_sincronizada.py:31`, `app/ponto/services/entrada_sincronizada.py:37`) |
| Triagem de batida | Auto-rejeita pendente vinculada e acrescenta nota à descrição após reconciliação. (`app/ponto/services/triagem_batida.py:597`, `app/ponto/services/triagem_batida.py:603`) |
| Reconciliador de chamados | Auto-rejeita pendente vinculada e acrescenta nota à descrição dentro da retratação. (`app/chamados/reconciliador.py:522`, `app/chamados/reconciliador.py:529`) |

Essas decisões operam sobre a justificativa/vínculo já escolhido; o efeito comum dos `save()` é o signal mandar julgar `instance.data_turno` no cartório. Isso é separado da abertura explícita do envelope. (`app/ponto/signals.py:161`)

2. **Juiz do dia e fonte das cobranças**

- **`dia_do_fato(chamado, fortes=False) -> date | None`**: primeiro célula ligada; depois chave declarada no catálogo ou `data_turno` legado; depois batida, disputa e timestamps fracos. `fortes=True` encerra antes desses últimos degraus. (`app/chamados/juizes.py:87`, `app/chamados/juizes.py:98`, `app/chamados/juizes.py:113`, `app/chamados/juizes.py:127`, `app/chamados/juizes.py:129`, `app/chamados/juizes.py:133`, `app/chamados/juizes.py:138`)
- **`data_do_chamado(modulo_origem, contexto_json)`** devolve o valor bruto da chave declarada, ou `None`; não devolve necessariamente um objeto `date`. (`app/chamados/catalogo/modulos.py:219`)
- **`dias_do_fato(chamados)`** devolve mapa `{chamado_id: date | None}`. O corpo do lote lê `data_turno` diretamente nesse degrau, enquanto o singular consulta também a chave do catálogo; equivalência integral entre ambos: **[nao verificado]**. (`app/chamados/juizes.py:171`, `app/chamados/juizes.py:187`; `app/chamados/juizes.py:113`)
- **`COBRANCAS_DO_TURNO`** é uma constante, não função: `('turno_aberto', 'batida_ausente')`. O consumidor de seleção localizado é a web; a API mantém sua lista literal. (`app/chamados/catalogo/modulos.py:83`; `app/ponto/views.py:1616`; `app/api/views.py:1971`)
- **`VIVOS`** contém `aberto`, `em_analise`, `registrado`; **`COBRANCA`** contém somente os dois primeiros. As seleções de justificativa usam `VIVOS`. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`; `app/api/views.py:1972`; `app/ponto/views.py:1616`)

**Já existe `cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)`.** Aceita módulo único ou coleção; retorna o chamado vivo mais novo, por `-criado_em,-pk`, ou `None`; confirma o dia com `dia_do_fato(..., fortes=True)`. Entretanto, primeiro restringe candidatos a JSON `data_turno=D` ou `celula.data=D`: outros degraus do juiz, isoladamente, não fazem um candidato atravessar esse pré-filtro. (`app/chamados/juizes.py:2151`, `app/chamados/juizes.py:2161`, `app/chamados/juizes.py:2163`, `app/chamados/juizes.py:2167`)

Seus consumidores encontrados são reatamento de perguntas, gate do fechamento, triagem, entrada sincronizada e cartório. (`app/chamados/models.py:390`, `app/chamados/models.py:748`; `app/ponto/services/triagem_batida.py:589`; `app/ponto/services/entrada_sincronizada.py:28`; `app/ponto/services/cartorio.py:1641`)

Além da seleção web, o censo de chamadas diretas/aliases de **`dia_do_fato`** encontrou:

- Chamados: `app/chamados/canal.py:45`; `app/chamados/juizes.py:2167`, `app/chamados/juizes.py:2343`; `app/chamados/models.py:185`, `app/chamados/models.py:731`, `app/chamados/models.py:1140`, `app/chamados/models.py:1234`, `app/chamados/models.py:1269`, `app/chamados/models.py:1345`; `app/chamados/reconciliador.py:139`, `app/chamados/reconciliador.py:200`, `app/chamados/reconciliador.py:286`, `app/chamados/reconciliador.py:1023`, `app/chamados/reconciliador.py:1290`; `app/chamados/signals.py:214`.
- Serviços de chamados: `app/chamados/services/acoes_chamado.py:370`; `app/chamados/services/adesao.py:106`; `app/chamados/services/celula_carimbada.py:57`, `app/chamados/services/celula_carimbada.py:92`, `app/chamados/services/celula_carimbada.py:151`; `app/chamados/services/competencia_trancada.py:261`; `app/chamados/services/declaracao_texto.py:199`, `app/chamados/services/declaracao_texto.py:330`, `app/chamados/services/declaracao_texto.py:461`, `app/chamados/services/declaracao_texto.py:571`, `app/chamados/services/declaracao_texto.py:606`; `app/chamados/services/disputa_emissao.py:231`, `app/chamados/services/disputa_emissao.py:781`; `app/chamados/services/feriado_reavaliar.py:30`; `app/chamados/services/fio.py:86`; `app/chamados/services/limbo_folgas.py:196`, `app/chamados/services/limbo_folgas.py:317`; `app/chamados/services/resposta_tardia.py:41`; `app/chamados/services/silencio_ausencia.py:62`; `app/chamados/services/vigia_loop.py:49`.
- Commands de chamados: `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:62`; `app/chamados/management/commands/auditar_invariantes_chamados.py:235`, `app/chamados/management/commands/auditar_invariantes_chamados.py:623`, `app/chamados/management/commands/auditar_invariantes_chamados.py:641`, `app/chamados/management/commands/auditar_invariantes_chamados.py:656`, `app/chamados/management/commands/auditar_invariantes_chamados.py:979`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1345`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:40`.
- Outros domínios: `app/colaboradores/services/calendario.py:439`; `app/escala/services/regeneracao.py:138`; `app/ponto/worklist.py:51`; `app/ponto/services/cluster_espurio.py:68`, `app/ponto/services/cluster_espurio.py:114`, `app/ponto/services/cluster_espurio.py:255`; `app/ponto/services/esmeril_espelho.py:183`; `app/ponto/services/faltas_de_hoje.py:65`; `app/ponto/services/furos_vetados.py:101`; `app/ponto/services/lastro.py:244`, `app/ponto/services/lastro.py:390`.
- Commands de ponto: `app/ponto/management/commands/detectar_intervalo_ausente.py:128`; `app/ponto/management/commands/emitir_furo_retroativo.py:84`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:64`; `app/ponto/management/commands/mapa_divergencia.py:242`; `app/ponto/management/commands/reconciliar_fantasmas.py:179`; `app/ponto/management/commands/reconciliar_grade.py:373`.

3. **Envelope `justificativa_colab`**

O nascimento explícito encontrado é o POST da API quando sua busca não acha cobrança. A abertura usa colaborador, categoria `justificativa`, módulo `justificativa_colab` e `VIVOS`; assunto, título, descrição, área DP, subcategoria, urgência e contexto com dia entram em `defaults`. **O dia não faz parte do lookup dessa abertura.** (`app/api/views.py:1977`, `app/api/views.py:1983`, `app/api/views.py:1990`)

A porta é `ChamadoColaborador.abrir`, com criação/obtenção e resolução de célula no nascimento. O catálogo declara o envelope com chave de data `None`, escopo `NENHUM`, dono `colab`; o juiz ainda pode ler seu `data_turno` legado. (`app/chamados/models.py:833`, `app/chamados/models.py:952`, `app/chamados/models.py:971`, `app/chamados/models.py:1224`; `app/chamados/catalogo/modulos.py:190`; `app/chamados/juizes.py:117`)

A API grava os dois sentidos do vínculo, mas preserva origem já preenchida; a porta inversa também preserva o primeiro apontamento e retorna `False` se já houver justificativa. Ela não escolhe dia nem cobrança. (`app/api/views.py:2000`, `app/api/views.py:2010`, `app/api/views.py:2014`; `app/chamados/services/vinculos.py:54`, `app/chamados/services/vinculos.py:81`)

Leitores encontrados:

- Painel identifica explicitamente o módulo como `origem_destaque`; a lista mostra a pílula “Justificativa”. (`app/chamados/services/painel.py:393`, `app/chamados/services/painel.py:489`; `app/templates/chamados/partials/_lista_chamados.html:62`)
- Painel de justificativa lê origem e followups, e seu template mostra o número da origem. (`app/ponto/views.py:1683`; `app/templates/ponto/partials/painel_justificativa.html:128`)
- O sistema registra ocorrências pela origem; aprovação encerra/retrata a origem; justificativa pendente bloqueia fechamento e aprovada pode regularizar a premissa. (`app/chamados/utils.py:2`; `app/ponto/services/justificativa.py:33`; `app/chamados/models.py:136`, `app/chamados/models.py:1266`)
- O reconciliador lê o vínculo para retenção e auto-rejeição na retratação. (`app/chamados/reconciliador.py:451`, `app/chamados/reconciliador.py:529`)
- O envelope fica fora dos dois módulos classificados como solicitação; a separação de abas usa esse juiz. (`app/chamados/rotulos.py:26`, `app/chamados/rotulos.py:36`; `app/chamados/views.py:612`)

4. **Ícones, destino e formulário**

| Elemento | Situação atual |
|---|---|
| Dashboard antigo | “Justificar Ponto” aponta para `chamados:abrir`. (`app/templates/core/dashboard_colaborador.html:97`, `app/templates/core/dashboard_colaborador.html:103`) |
| Dashboard v2 | “Justificar” aponta para `chamados:abrir`. A view do dashboard seleciona v2 como padrão. (`app/templates/core/dashboard_colaborador_v2.html:82`; `app/core/views.py:270`) |
| Destino atual | `/chamados/abrir/` → `abrir_chamado`. O fluxo próprio do colaborador recebe assunto/descrição/categoria, sem selecionar dia, e chama `abrir_chamado_local`. (`app/config/urls.py:28`; `app/chamados/urls.py:10`; `app/chamados/views.py:167`, `app/chamados/views.py:196`) |
| Persistência desse texto livre | Cria `ChamadoColaborador` com descrição e classificação; seu contexto contém identificação e pendência GLPI, sem dia do fato nesse corpo. (`app/chamados/services/abertura.py:19`, `app/chamados/services/abertura.py:47`, `app/chamados/services/abertura.py:58`) |
| Formulário de justificativa | `/ponto/justificativa/abrir/`, nome `ponto:abrir_justificativa`, view `abrir_justificativa`, template `ponto/justificativa_form.html`. (`app/config/urls.py:29`; `app/ponto/urls.py:37`; `app/ponto/views.py:1552`, `app/ponto/views.py:1654`) |

**Existe uma barreira adicional ao href:** GET sem qualquer justificativa pendente do colaborador redireciona para `chamados:abrir`; POST e GET com pendência chegam ao fluxo. (`app/ponto/views.py:1561`, `app/ponto/views.py:1564`)

Hoje a lista vem de `turnos_do_colab`, janela de dois dias e entrada nas últimas 48 horas, somente turnos abertos com entrada. O `<select name="data_turno">` usa a data do timestamp de entrada; sem turnos, o template nem apresenta o formulário. **Não é uma lista de dias obtida das cobranças vivas.** (`app/ponto/views.py:1581`; `app/templates/ponto/justificativa_form.html:16`, `app/templates/ponto/justificativa_form.html:27`)

O formulário recebe tipo, saída informada e descrição; seu POST segue para a mesma URL. (`app/templates/ponto/justificativa_form.html:22`, `app/templates/ponto/justificativa_form.html:37`, `app/templates/ponto/justificativa_form.html:53`, `app/templates/ponto/justificativa_form.html:60`)

5. **Arquivos envolvidos e interseções**

A interseção funcional direta do pedido está nos arquivos abaixo; isto identifica os sítios existentes, não um diff futuro fechado. (`app/api/views.py:1941`; `app/ponto/views.py:1552`, `app/ponto/views.py:1568`; `app/templates/ponto/justificativa_form.html:27`; `app/templates/core/dashboard_colaborador.html:97`; `app/templates/core/dashboard_colaborador_v2.html:82`)

- `app/api/views.py`.
- `app/ponto/views.py`, incluindo a barreira de GET.
- `app/templates/ponto/justificativa_form.html`.
- `app/templates/core/dashboard_colaborador.html`.
- `app/templates/core/dashboard_colaborador_v2.html`.
- Camada de serviço de justificativa: já existe `app/ponto/services/justificativa.py`, dedicada hoje às decisões/materialização; arquivo e nome da função de submissão futura: **[nao verificado]**. (`app/ponto/services/justificativa.py:132`, `app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:331`)
- Testes dos contratos afetados, identificados no item 6; arquivos novos específicos da MSG-0: **[nao verificado]**. [nao verificado]

`api/views_core.py` entra no censo pelo gêmeo da regularização, mas alteração nele por causa de um gêmeo de `api_justificativas` não foi estabelecida. (`app/api/views_core.py:1143`; `app/api/urls_core.py:19`; necessidade de alteração: **[nao verificado]**)

Quanto aos arquivos destacados no pedido: os ícones e o formulário estão nos templates acima; o formulário estende `base_tpl`. Não há interseção direta estabelecida com `apto*.html`, `bater_ponto_v2.html` ou os templates de chamados enumerados; o conjunto final de arquivos alterados permanece **[nao verificado]**. (`app/templates/ponto/justificativa_form.html:1`; `app/templates/core/dashboard_colaborador.html:97`; `app/templates/core/dashboard_colaborador_v2.html:82`)

Há, porém, interseção **de fluxo** com bater ponto: ele chama `ramo_regularizar_saida`, outro escritor encontrado, em `triagem_batida.py`. Isso não demonstra necessidade de alterar seu template. (`app/ponto/views.py:333`; `app/ponto/services/triagem_batida.py:227`)

O PWA localizado nesta cópia está em **`app/static/pwa.js`**; correspondência com o caminho citado `static/js/pwa.js`: **[nao verificado]**. (`app/static/pwa.js:1`)


[truncado em 20000 de 25289 caracteres]

## 11/11 12:24 · sessao 7109902a2bb3 · BO-ROTULO-ENCERRADO · verificador · origem=Code

**BLOQUEIA — os exemplos da volta 1 foram fechados, mas a classe continua aberta.** O selo inspeciona fonte HTML; não observa o texto final após includes, renderização de variáveis ou JS. A comparação de duplicidade continua limitada a cada arquivo isolado. ([teste:41](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:41), [teste:120](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:120))

**1. Fechamento dos bloqueios da volta 1**

| Escape anterior | Situação nesta cópia |
|---|---|
| Aspas simples | Fechado para opções literais reconhecidas: `_ATTR` aceita aspas duplas, simples e valor sem aspas. `app/chamados/tests/test_bo_rotulo_encerrado.py:21`. |
| Partial sem `<select>` | Fechado para `<option>` literal com código conhecido em `.html` sob `templates/chamados`, independentemente de ter seletor. Valor dinâmico ainda escapa. Mesmo teste:110 e :113. | [nao verificado]
| Outro nome de controle | A proibição anterior agora independe do nome, dentro de `templates/chamados`; a comparação global também reconhece opções de outros controles. Porém o censo e `DESTINOS` continuam restritos a `name=novo_status`. Mesmo teste:46, :57 e :110. | [nao verificado]
| Arquivo fora de `templates/chamados` | A comparação de textos passou a percorrer todo `app/templates`, mas separadamente por arquivo; a proibição geral de opções manuais continua restrita a `chamados`. Mesmo teste:110 e :120. | [nao verificado]
| Remover, acrescentar ou reordenar destinos | Fechado para as chamadas literais reconhecidas nos cinco seletores: comparação exata com `DESTINOS`. Mesmo teste:127. | [nao verificado]

Um contraexemplo conserva **exatamente os destinos atuais**, alterando apenas seus textos. Inserido imediatamente depois do seletor de [detalhe_local.html:399](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/templates/chamados/detalhe_local.html:399):

```html
<script>
document.querySelectorAll('select[name="novo_status"] option').forEach(function(o) {
  if (['resolvido', 'fechado'].includes(o.value))
    o.textContent = 'Encerrado';
});
</script>
```

O varredor continua lendo as quatro tags originais e seus textos em `MOVER_PARA`; não interpreta essa alteração por JS. O teste final apenas compila os templates. ([teste:48](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:48), [teste:130](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:130), [teste:134](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:134))

Também reproduzi o escape de partial **dentro de chamados**: manter as tags e acrescentar dois includes, passando `destino="resolvido"` e `destino="fechado"`, de um partial com:

```django
{% load chamados_tags %}
<option value="{{ destino }}">{{ destino|espera|first }}</option>
```

O selo ignora o `value` dinâmico porque ele não pertence literalmente a `ESTADOS`; o filtro existente devolve “Encerrado” para ambos os códigos. ([teste:46](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:46), [teste:113](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:113), [verbos.py:114](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/services/verbos.py:114))

**Verificação:** executei em memória, via AST, os seis métodos independentes de Django: **6/6 passaram na base, com o JS e com o partial dinâmico**. Não alterei os testes extraídos. Os dois métodos dependentes de Django, a suíte integral e a renderização em navegador ficam `[nao verificado]`; Django está indisponível neste ambiente `[nao verificado]`. Métodos exercitados: `app/chamados/tests/test_bo_rotulo_encerrado.py:75`, :80, :95, :99, :117 e :127.

**2. Censo e cobertura**

O selo percorre **todos os arquivos terminados em `.html` sob `app/templates/`**. Não percorre arquivos `.js`, Python, forms ou templates com outra extensão; também não renderiza includes. ([teste:17](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:17), [teste:33](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:33), [teste:134](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/chamados/tests/test_bo_rotulo_encerrado.py:134))

Estes são os cinco controles encontrados que **oferecem códigos de estado como destinos**, todos cobertos estaticamente por `DESTINOS` e pela proibição de opções manuais (`app/chamados/tests/test_bo_rotulo_encerrado.py:64`, :99 e :127):

| Controle | Destinos, na ordem, após o placeholder vazio |
|---|---|
| [detalhe.html:130](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/templates/chamados/detalhe.html:130) | `em_analise`, `resolvido`, `fechado`, `aberto` |
| [detalhe_local.html:393](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/templates/chamados/detalhe_local.html:393) | `em_analise`, `resolvido`, `fechado`, `aberto` |
| [fio_colaborador.html:265](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/templates/chamados/fio_colaborador.html:265) | `em_analise`, `resolvido`, `fechado` |
| [modal_fio.html:665](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/templates/chamados/partials/modal_fio.html:665) | `em_analise`, `resolvido`, `fechado` |
| [painel_chamado_turno.html:74](/home/ronald/codex_ws/c6d9cb734ebf09e73efc4d5930153cb769273169/app/templates/chamados/partials/painel_chamado_turno.html:74) | `em_analise`, `resolvido`, `fechado` |

Não encontrei oferta adicional de destinos montada por JS, view Python ou form Django; ausência absoluta de construção indireta: `[nao verificado]`. Os receptores encontrados recebem diretamente o código do POST: `app/chamados/services/acoes_chamado.py:287` e `app/chamados/services/fio.py:502`.

Há também ações que alcançam mudanças de estado. **Nenhuma é coberta pelo selo como controle de ação**, pois ele reconhece opções e os cinco seletores (`app/chamados/tests/test_bo_rotulo_encerrado.py:41`, :52 e :127):

| Ação/controle | Efeito e referências |
|---|---|
| Aprovar/rejeitar benefício, inclusive botões construídos por JS | `resolvido`/`fechado`: `app/templates/colaboradores/painel_solicitacao_beneficio.html:23`; `app/templates/colaboradores/fila_solicitacoes_beneficio.html:138`; `app/colaboradores/views.py:2068`. |
| Aceitar/recusar escala ou agenda | `resolvido`: `app/templates/chamados/detalhe_local.html:163` e :200; `app/chamados/services/acoes_chamado.py:149` e :108. |
| Aprovar exceção | `resolvido`: detalhe local:256; modal:598; `app/chamados/services/acoes_chamado.py:70`. |
| Autorizar geofence, com/sem advertência, ou recusar | `resolvido`, com preparação de `registrado` para análise: detalhe local:285; modal:506; `app/chamados/services/acoes_chamado.py:222`, :233 e :277. |
| Abrir registro de ausência | `analisar()`: detalhe local:233; `app/chamados/views.py:1513`. |
| Reabertura administrativa de disputa; resgatar chamado encerrado pela tranca | `reabrir(humano=True)`: modal:390; `app/chamados/services/acoes_disputa.py:269`; `app/templates/chamados/partials/_lista_chamados.html:85`; `app/chamados/views_arquivar.py:70`. |
| Aceitar folga sugerida | Reconcilia chamados do template: modal:550; `app/chamados/services/limbo_folgas.py:98`. |
| Validar resposta, validar assim mesmo, inline, lote e aplicar declaração escrita | Caminhos de validação/materialização/reconciliação: modal:140, :265 e :281; `_lista_chamados.html:82`; `app/templates/chamados/partials/_lote_preview.html:21`; `app/chamados/services/validacao.py:295`; `app/ponto/services/veredito_celula.py:202`. |
| Regularizar disputa e fechar | Materializa perguntas validadas: modal:355; `app/chamados/services/acoes_disputa.py:139`. Resultado de cada caso sobre `status_local`: `[nao verificado]`. |
| Iniciar disputa dos órfãos; pedir confirmação de âncora | `analisar()`: modal:628 e :644; `app/chamados/services/disputa_emissao.py:1159` e :1379. |
| Desligar colaborador | Encerra chamados: `app/templates/colaboradores/editar.html:271`; `app/colaboradores/services/desligamento.py:150`. |
| Salvar vínculo; realocar régua | Reconciliação posterior: `app/templates/colaboradores/partials/painel_vinculo.html:136`; modal:575; `app/colaboradores/services/vinculo.py:329`; `app/colaboradores/services/pos_vinculo.py:42`. |
| Regularizar retorno | Põe chamados em análise: `app/ponto/views.py:3153`; `app/ponto/services/regularizacao.py:126`. |

O histórico oferece um **filtro**, cujo receptor aplica `qs.filter`, sem transição (`app/templates/relatorios/historico_chamados.html:54`; `app/relatorios/views.py:920`). “Reabrir questionário” altera respostas/carimbos da pergunta; seus efeitos posteriores sobre `status_local` ficam `[nao verificado]` (`app/chamados/services/disputa_emissao.py:1593` e :1632). Censo integral de todo efeito indireto de alterações cadastrais e de fatos: `[nao verificado]`.

**3. `DESTINOS`, receptores e preservação da base**

**Conjunto, ordem e `value` foram preservados pelo diff fornecido** nos cinco seletores; conferem com a tabela acima e com `DESTINOS`. Checkout independente da base: `[nao verificado]`. Fontes atuais: `app/chamados/tests/test_bo_rotulo_encerrado.py:64` e os cinco seletores citados.

Isso não significa que cada destino oferecido seja aceito em qualquer situação:

- Detalhe local e painel de turno chegam à resposta livre; fio e modal compartilham `processar_post_fio`. Ambos verificam pertinência a `STATUS_CHOICES` e delegam a `transicionar`, sem consultar `DESTINOS` (`app/chamados/views.py:547`, :674 e :953; `app/templates/chamados/partials/painel_chamado_turno.html:68`; `app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/fio.py:719`).
- O motor decide por origem/perfil e recusa destino igual à origem. Para perfil comum: `aberto → em_analise/resolvido/fechado`; `em_analise → resolvido/fechado/aberto`; `resolvido → fechado/em_analise`; `registrado → em_analise/fechado`. Há exceção para `admin_ti` e terminais absolutos (`app/chamados/catalogo/motor.py:99` e :139).
- O modelo ainda aplica guardas de justificativa, disputa, perguntas e regularização do furo (`app/chamados/models.py:135`, :148 e :181).

**FILA — detalhe GLPI oferece status sem aplicá-lo:** o formulário envia ao followup; esse receptor lê conteúdo e envia a mensagem ao GLPI, ignorando `novo_status` (`app/templates/chamados/detalhe.html:122`; `app/chamados/urls.py:12`; `app/chamados/views.py:292` e :304).

**FILA — “Reabrir” pode ser recusado:** envia `aberto`; `resolvido → aberto` é recusado para perfil comum. Essa desconexão já existia no diff da base fornecido (`app/chamados/services/verbos.py:136`; `app/chamados/catalogo/motor.py:102` e :153).

**4. Quem escreve e quem lê**

| Valor | Escritor → leitores |
|---|---|
| Código/`value` | Argumentos literais dos cinco templates → tag escreve o mesmo código → POST → serviços → `transicionar` → `_aplicar` → `_gravar_estado`. `app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/services/acoes_chamado.py:287` e :315; `app/chamados/services/fio.py:502` e :721; `app/chamados/models.py:199`, :228 e :450. |
| Texto da opção | `MOVER_PARA` → `rotulo_de_movimento` → tag → texto HTML. `app/chamados/services/verbos.py:132` e :146; `app/chamados/templatetags/chamados_tags.py:76`. |
| `title` | `ESTADO_TITLE` → helper/tag; também alimenta rótulo da linha e da pessoa. `app/chamados/services/verbos.py:100`, :146, :155 e :166; `app/chamados/templatetags/chamados_tags.py:77`. |
| Filtro `espera` | Delega a `rotulo_para_pessoa`, que lê `ESPERA_COLAB`; permanece nas pílulas de estado atual. `app/chamados/templatetags/chamados_tags.py:53`; `app/chamados/services/verbos.py:162`; `app/templates/chamados/detalhe_local.html:68`. |

Nos receptores rastreados, decisão e gravação usam **códigos**, sem interpretar texto ou tooltip. O compartilhamento de “Encerrado” permanece na apresentação para a pessoa; as opções de movimento agora usam o mapa específico (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/fio.py:719`; `app/chamados/services/verbos.py:114` e :132).

**5. Classes anteriores**

- **BLOQUEIA — selo com mordida insuficiente:** permanece pelos contraexemplos do item 1. Porém **não passa vazio**: exige pelo menos cinco arquivos e igualdade exata com `DESTINOS` (`app/chamados/tests/test_bo_rotulo_encerrado.py:95` e :131).
- **Segundo juiz duplicando o registrado:** não demonstrado no delta. A autoridade continua `motor.decidir`; a tag consulta apresentação, e `DESTINOS` é usado nos testes (`app/core/juizes.py:274`; `app/chamados/models.py:123`; `app/chamados/templatetags/chamados_tags.py:74`; `app/chamados/tests/test_bo_rotulo_encerrado.py:127`).
- **Derivação paralela da mesma fonte:** não demonstrada no delta. Os cinco seletores consultam o helper único; movimento e espera têm consumidores distintos (`app/chamados/services/verbos.py:140` e :162; seletores do item 2).
- **Campo com dois significados:** não demonstrado como introdução desta obra. Código, texto e `title` continuam separados; permanecem os dois achados de fila do item 3 (`app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/services/acoes_chamado.py:313`).

## 11/11 12:23 · sessao f33dea6f2c6b · UI-PAINEL-MOSTRA-O-APP · verificador · origem=Code

**Parecer: bloqueia declarar PRONTO pelo selo exigido.** Os testes conferem as duas cascas por grafo de includes e texto JavaScript; o ORM renderiza somente o partial. Isso não cumpre literalmente “renderiza as duas cascas”. A execução ORM e seus mutantes permanece **[nao verificado]**. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:83`; `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:5,94`)

**1. Existe uma função única?**

**Sim, para os três leitores pedidos; não globalmente para toda superfície do colaborador.** A decisão completa do badge está em `projecao_do_app`: pergunta pendente vence mensagem não lida; a função escolhe `count`, `cor`, `rotulo` ou `None`. (`app/chamados/services/pergunta_no_app.py:89-104`)

| Sítio | Decide ou lê? |
|---|---|
| `app/chamados/services/pergunta_no_app.py:90-102` | **Decide** os dois números e o badge. |
| `app/api/views.py:315-319,358-360` | Lê e propaga a projeção. |
| `app/api/views_core.py:303-307,346-348` | Lê e propaga a projeção. |
| `app/colaboradores/views.py:940-941,1006` | Lê e entrega `no_app` ao template. |
| `app/templates/colaboradores/partials/painel_colaborador.html:190-198` | Lê cor/rótulo/número e as perguntas prontas. |
| `app/templates/colaboradores/partials/painel_colaborador.html:39-40` | Calcula **outro número**, autorizado pelo aval: chamados exibidos + perguntas pendentes. |
| `app/templates/core/_drawer_generico.html:118-145` | Decide visibilidade e seleção das abas; não decide o badge. |
| `app/core/views.py:252-253` | Conta perguntas diretamente pela `consulta_do_app`. |
| `app/templates/core/dashboard_colaborador_v2.html:78-80` | **Decide sozinho** texto, cores e badge de perguntas do dashboard web. |

**Fila — derivação paralela preexistente:** o dashboard web ativo continua com sua própria apresentação “Responder DP/Chamados”; portanto, “uma função para todo o app” seria uma afirmação excessiva. Essa superfície é usada por `core.views`, mas não foi alterada pelo diff fornecido. (`app/core/views.py:270-271`; `app/templates/core/dashboard_colaborador_v2.html:78-80`)

**2. Antes/depois dos dois kernels**

Pelo diff fornecido, **não houve mudança das três chaves, dos valores para um mesmo estado ou da precedência do badge**. Os dois kernels continuam expondo `chamados_nao_lidos`, `questionarios_pendentes` e `chamados_badge`. (`app/api/views.py:358-360`; `app/api/views_core.py:346-348`; `app/chamados/services/pergunta_no_app.py:90-104`)

| Estado | Antes → depois, conforme o diff |
|---|---|
| P perguntas pendentes, inclusive com mensagens não lidas | `Responder DP`, `count=P`, `#f59e0b` |
| Zero perguntas; C chamados vivos com nota DP não lida | `Chamados`, `count=C`, `#ef4444` |
| Nenhum dos dois | `chamados_badge=None`; contadores zero |

A regra atual que sustenta os três casos está em `app/chamados/services/pergunta_no_app.py:97-102`; a comparação histórica independente do anexo é **[nao verificado]**.

**Mudou a ordem das consultas, não a precedência:** antes o diff mostrava a contagem de não lidos primeiro; agora C6 é consultado antes da contagem. Igualdade das respostas durante alterações concorrentes é **[nao verificado]**. (`app/chamados/services/pergunta_no_app.py:89-96`)

O que o Android/iOS efetivamente desenha e a ausência de alterações em seus fontes são **[nao verificado]**; o contrato dos endpoints acima está preservado na extração mostrada.

**3. O painel monta universo próprio?**

**O novo aviso não.** A porta é:

`painel_colaborador → projecao_do_app → consulta_do_app → perguntas_do_app → q_vivas/pergunta_viva`.

Ela reaproveita C6 e separa pendentes/histórico sem novo predicado no painel. (`app/colaboradores/views.py:940-941`; `app/chamados/services/pergunta_no_app.py:47-58,69-75,89`)

A query de não lidos também passou para essa mesma projeção, compartilhada pelos dois endpoints e pelo painel. (`app/chamados/services/pergunta_no_app.py:91-96`)

**Existe uma query local antiga**, dos últimos cinco chamados da pessoa, de qualquer status. Ela alimenta a lista e a parcela antiga do contador da aba; não determina a faixa “No app dele agora”. (`app/colaboradores/views.py:937-939,1005-1006`; `app/templates/colaboradores/partials/painel_colaborador.html:39,190-191`)

**4. Quem escreve, quem lê e qual a unidade?**

| Campo/valor | Escritor e significado | Leitores |
|---|---|---|
| `pendentes` / `historico` | `consulta_do_app`; perguntas C6 vivas, separadas por resposta e fechamento da disputa. (`app/chamados/services/pergunta_no_app.py:69-75`) | Projeção e lista do painel. (`app/chamados/services/pergunta_no_app.py:89`; `app/templates/colaboradores/partials/painel_colaborador.html:193-198`) |
| `questionarios_pendentes` | Projeção; **quantidade de perguntas**, não disputas. (`app/chamados/services/pergunta_no_app.py:90`) | Dois `/me/` e soma do botão. (`app/api/views.py:318,359`; `app/api/views_core.py:306,347`; template do painel:39) |
| `chamados_nao_lidos` | Projeção; **quantidade de chamados vivos com pelo menos uma nota DP não lida**, não quantidade de notas. (`app/chamados/services/pergunta_no_app.py:91-96`) | Dois `/me/` e escolha do badge. (`app/api/views.py:317,358`; `app/api/views_core.py:305,346`; serviço:99-100) |
| `count`, `cor`, `rotulo` | Exclusivamente a projeção para os três leitores pedidos. (`app/chamados/services/pergunta_no_app.py:97-104`) | APIs propagam; faixa apresenta diretamente. (`app/api/views.py:319,360`; `app/api/views_core.py:307,348`; template do painel:190-191) |
| `no_app` | Variável/contexto transitório, produzido pela view; não é coluna persistida. (`app/colaboradores/views.py:941,1006`) | Template do painel:39,190-198. |
| `chamados` / `total_mensagens` | View seleciona até cinco chamados; template soma perguntas. (`app/colaboradores/views.py:937-939`; template do painel:39-40) | Botão e lista antiga. (`app/templates/colaboradores/partials/msgs_lista.html:1-11`) |

Os universos são por **pessoa**, sem filtro local de vínculo ou do mês do Espelho; as perguntas continuam submetidas à vitalidade C6. O dia apresentado é `p.data_evento`. (`app/chamados/services/pergunta_no_app.py:49-58,91-96`; `app/templates/colaboradores/partials/painel_colaborador.html:198`)

Nos insumos persistidos, notas nascem por `nota.anexar_ocorrencia`; o carimbo de leitura é escrito por `nota.marcar_lidas`. O painel novo apenas consulta. (`app/chamados/services/nota.py:75-109,126-133`; `app/colaboradores/views.py:940-941`)

Texto/dia da pergunta passam pelo nascimento; marco é derivado/validado na inserção. O diff não acrescenta escritores desses campos. (`app/chamados/models.py:2825-2828,3013-3017,3425-3447`)

**Não encontrei leitor alterado supondo outra unidade.** `count` tem unidade condicionada ao rótulo, como antes; o contador do botão tem outra medida, expressamente pedida pelo aval. Não é evidência de corrupção semântica nova. (`app/chamados/services/pergunta_no_app.py:98-100`; `app/templates/colaboradores/partials/painel_colaborador.html:39-40`)

**5. Os selos mordem?**

- **Memória: sim, nos mutantes fornecidos.** Nesta revisão, foram 7/7 verdes; `painel_query` e `template_rotulo` morreram com uma falha cada; as duas inversões de precedência morreram com três falhas cada, sem erros. Alvos/assertivas: `app/colaboradores/tests/ui_painel_fontes.py:19-35`; `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:25-172`.
- **ORM: tem casos pertinentes, mas execução e morte dos mutantes ORM estão [nao verificado].** O código cobre duas perguntas na mesma disputa, precedência, chamados versus notas, vazio, origem variável e preservação do carimbo de leitura. (`app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:114-192`)
- **Bloqueia — duas cascas sem renderização.** O teste estático segue includes e procura strings; o ORM chama a view do partial. Nenhum deles renderiza `base.html` e `base_app.html`. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:83-105`; `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:94-101`)
- **E3 não perdeu os casos pelo deslocamento.** Os mutantes agora alteram o escalar; a matriz existente exige esse escalar e duas perguntas na mesma disputa. O autor errado passou para a query real e continua discriminado. (`app/chamados/tests/e3_mutantes.py:23-26,87-88`; `app/chamados/tests/test_e3_app_orm.py:41-50,62-74`)
- **Tripwire não foi afrouxado pelo diff:** duas entradas API viraram uma entrada do serviço; teto caiu de 26 para 25, mantendo `SEM_JUIZ=1` e as verificações de sítio novo, declaração sobrando e testemunhas. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:81-85,147-148,370-399`)
- **Fila — cor sem mordida:** cor fixa no template sobrevive aos sete testes em memória; o ORM também não compara o estilo do painel. O código atual, contudo, lê a cor correta da origem. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:39-47`; `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:114-145`; `app/templates/colaboradores/partials/painel_colaborador.html:190`)

**6. Classes recorrentes e demais achados**

**Não identifiquei segundo juiz registrado nem segundo escritor persistente introduzido.** A projeção reaproveita C6, registrado em `core/juizes.py`, e monta somente dados de apresentação. (`app/core/juizes.py:278`; `app/chamados/services/pergunta_no_app.py:47-104`; `app/docs/JUIZES.md:10-14`)

**Relógio:** a função nova não lê relógio; existe fallback herdado `date.today()` no caminho dos juízes. Esse trecho não foi alterado pelo diff fornecido. (`app/chamados/services/pergunta_no_app.py:78-104`; `app/chamados/juizes.py:569-577`)

Além do bloqueio do selo e das filas já apontadas:

- **Fila — faixa desatualiza após envio pelo próprio painel.** O POST grava nota DP, mas devolve somente a lista; o form troca apenas `#msgs-lista`. A faixa e o contador permanecem com o primeiro render. Reprodução em navegador: **[nao verificado]**. (`app/chamados/views.py:973-986,995-1003`; `app/templates/colaboradores/partials/painel_colaborador.html:190-205`)
- **Fila — entrega Haiku não demonstrada.** O golden contém pergunta histórica por dia, outro contrato; a pergunta nova e seu leitor da projeção são **[nao verificado]**. (`eval/golden.py:35-37`)
- Smoke de clique do Ronald, encaixe na fila 2/RELATO e comprovação independente do hash/base: **[nao verificado]**.

## 11/11 12:22 · sessao fbcbddda6571 · - · - · origem=Ronald (tmux)

Censo concluído; somente leitura.

- **Juiz:** `dia_do_fato(chamado, fortes=False) -> date | None`, escada célula → chave declarada no catálogo/`data_turno` → batida → disputa sem conversas → timestamps fracos; `fortes=True` encerra antes da batida. `app/chamados/juizes.py:87`, `:98`, `:113`, `:127`, `:129`, `:133`, `:138`.
- **Fonte dos módulos:** `COBRANCAS_DO_TURNO = ('turno_aberto', 'batida_ausente')`; único consumidor de produção encontrado: seleção da web. `app/chamados/catalogo/modulos.py:83`; `app/ponto/views.py:1616`.
- **Função já existente:** `cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None) -> ChamadoColaborador | None`; módulos string ou coleção, `VIVOS`, mais novo por `-criado_em,-pk`, confirmação por `dia_do_fato(fortes=True)`. O pré-filtro só admite JSON `data_turno` ou célula naquela data. `app/chamados/juizes.py:2151`, `:2161`, `:2163`, `:2167`.
- **Consumidores dessa função:** reatar perguntas e gate do fechamento (`app/chamados/models.py:390`, `:748`); triagem (`app/ponto/services/triagem_batida.py:589`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:28`); cartório (`app/ponto/services/cartorio.py:1641`).
- **Estados:** `VIVOS` inclui `registrado`; `COBRANCA` contém apenas `aberto,em_analise`. `app/chamados/catalogo/motor.py:24`, `:29`.
- **Envelope:** único nascimento literal de produção encontrado em `api_justificativas`, quando falta chamado de turno: `ChamadoColaborador.abrir`, categoria `justificativa`, módulo `justificativa_colab`, `area='dp'`, subcategoria tipo, contexto com dia. Dia está em `defaults`, não no lookup. `app/api/views.py:1977`, `:1981`, `:1983`, `:1990`.
- **Catálogo do envelope:** chave de data `None`, escopo `NENHUM`, dono `colab`; mesmo assim o juiz admite o legado `data_turno` presente no contexto. `app/chamados/catalogo/modulos.py:190`; `app/chamados/juizes.py:117`.
- **Ponteiros:** API grava `Justificativa.chamado_origem` e usa `vinculos.apontar_justificativa` para `ChamadoColaborador.justificativa_relacionada`; esta porta preserva primeiro ponteiro. `app/api/views.py:2000`, `:2010`, `:2014`; `app/chamados/services/vinculos.py:81`.
- **Leitores explícitos do módulo:** painel marca `origem_destaque`; lista mostra pílula “Justificativa”. `app/chamados/services/painel.py:393`, `:489`; `app/templates/chamados/partials/_lista_chamados.html:62`.
- **Leitores genéricos:** ocorrências do sistema pela origem (`app/chamados/utils.py:2`); painel lê origem/followups (`app/ponto/views.py:1683`); template mostra origem (`app/templates/ponto/partials/painel_justificativa.html:128`); aprovação retrata origem (`app/ponto/services/justificativa.py:33`); pendência bloqueia fecho (`app/chamados/models.py:136`); aprovada regulariza premissa (`app/chamados/models.py:1266`); reconciliador retém justificativa alheia e auto-rejeita pendente na retratação (`app/chamados/reconciliador.py:451`, `:529`).
- **Classificação de aba:** envelope não pertence aos módulos de solicitação (`chamados`, `app_colaborador`); a view o separa para pendências pelo juiz. `app/chamados/rotulos.py:26`, `:36`; `app/chamados/views.py:612`.

Lista completa de chamadores diretos/aliases de `chamados.juizes.dia_do_fato` em produção já enviada ao pai por mensagem, com arquivos e linhas; inclui views, modelos, reconciliador, serviços, sinais e commands.

## 11/11 12:22 · sessao c0ce86a26e9d · - · - · origem=Ronald (tmux)

Censo em leitura; caminhos abaixo relativos à raiz da cópia.

1. Escritores de entrada/criação de `ponto.Justificativa`

| Porta/escritor | Grava | Dia e vínculo | Envelope/chamado |
|---|---|---|---|
| `api_justificativas` | Procura pendente por `(colaborador, data_turno)`, menor id; atualiza `tipo`, `descricao`, `chamado_origem` somente se nulo; ou cria com esses campos e `status='pendente'`. `app/api/views.py:1994`, `app/api/views.py:2004` | `data_turno` ISO recebido do cliente. Cobrança por lista fixa `['turno_aberto','batida_ausente']`, `VIVOS`, mesmo colab; dia por `contexto_json.data_turno` ou, ausente, `criado_em__date` — mistura dia declarado com dia civil de criação. `app/api/views.py:1952`, `app/api/views.py:1960`, `app/api/views.py:1967` | Se não acha, chama `ChamadoColaborador.abrir` com `categoria='justificativa'`, `modulo_origem='justificativa_colab'`, `subcategoria=tipo`, descrição `tipo: descricao`, `area='dp'`, urgência normal e contexto com dia. Depois aponta inverso pela porta `vinculos.apontar_justificativa`. `app/api/views.py:1977`, `app/api/views.py:2012` |
| `_abrir_justificativa_fluxo` web | Mesma procura de pendente; atualiza `tipo`, `descricao`, `saida_informada` se fornecida, `batida_entrada` e `chamado_origem` se nulos; ou cria com esses campos e pendente. `app/ponto/views.py:1621`, `app/ponto/views.py:1635` | Dia ISO recebido; acha entrada por `timestamp__date=data_turno` (civil). Para cobrança usa `COBRANCAS_DO_TURNO`, `VIVOS`, colab, menor pk cujo `dia_do_fato(c)==data_turno`, com célula carregada. `app/ponto/views.py:1594`, `app/ponto/views.py:1597`, `app/ponto/views.py:1612` | Sem cobrança, grava com origem nula e não há abertura explícita de envelope; havendo, aponta inverso por `apontar_justificativa`. `app/ponto/views.py:1643`, `app/ponto/views.py:1645` |
| `regularizar_turno_orfao` | `get_or_create` por colab, batida E, dia; defaults `tipo='esqueci_saida'`, saída informada, descrição contendo motivo do colab, pendente. Se existente sem saída, atualiza saída, descrição e status. `app/ponto/services/regularizacao.py:19`, `app/ponto/services/regularizacao.py:91`, `app/ponto/services/regularizacao.py:102` | Dia civil local da batida E, `tz.localtime(batida_e.timestamp).date()`; valida turno pela geometria `turnos_do_colab`. Não grava novo `chamado_origem` nem aponta inverso. `app/ponto/services/regularizacao.py:59`, `app/ponto/services/regularizacao.py:73`, `app/ponto/services/regularizacao.py:91` | Não abre envelope; põe em análise todos os chamados vivos do colab em lista fixa de cinco módulos, **sem filtro de dia**, anexando ocorrência com os motivos. `app/ponto/services/regularizacao.py:126` |
| `ramo_regularizar_saida`, caminho de bater ponto | `_Just.objects.update_or_create` por colab, dia, entrada; defaults `tipo='esqueci'`, saída informada, descrição montada com horário informado pelo colab, pendente. `app/ponto/services/triagem_batida.py:227`, `app/ponto/services/triagem_batida.py:258` | Dia civil local da entrada: `_ent['entrada'].date()`; a entrada do contexto vem de `timezone.localtime(_t.entrada.timestamp)`. Não grava vínculo novo. `app/ponto/services/triagem_batida.py:180`, `app/ponto/services/triagem_batida.py:260` | Não abre envelope; escala urgência dos chamados vivos `entrada_bloqueada_turno_aberto` do colab, sem filtro de dia. `app/ponto/services/triagem_batida.py:287` |
| Cron `_abrir_justificativa_automatica` | `get_or_create` por colab e dia; defaults entrada, `esqueci_saida`, descrição automática, pendente e chamado origem. Só quando criado aponta inverso pela porta. `app/ponto/management/commands/processar_alertas_turno.py:392`, `app/ponto/management/commands/processar_alertas_turno.py:394`, `app/ponto/management/commands/processar_alertas_turno.py:409`, `app/ponto/management/commands/processar_alertas_turno.py:426` | Dia recebido de `tm.data_turno`; cobrança nasce antes, no mesmo dia, módulo `turno_aberto`, contexto `data_turno`, pela porta `abrir`. `app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:324`, `app/ponto/management/commands/processar_alertas_turno.py:335` | Abre cobrança crítica, **não** envelope `justificativa_colab`; se a porta recusa cobrança, o fluxo não cria justificativa. `app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:321` |

2. Cascas adicionais e admin

- A regularização é chamada pelo web `regularizar_retorno`, por `api/views.py::api_regularizar_turno` e pelo gêmeo em `api/views_core.py::api_regularizar_turno`. `app/ponto/views.py:3153`, `app/ponto/views.py:3215`, `app/api/views.py:2821`, `app/api/views.py:2866`, `app/api/views_core.py:1143`, `app/api/views_core.py:1188`.
- O ramo de saída na tela de bater ponto é acionado quando `tipo=='regularizar_saida'`. `app/ponto/views.py:333`.
- Não encontrei `api_justificativas` nem importação/uso de `Justificativa` no corpo de `api/views_core.py`; o gêmeo relacionado encontrado é o de regularização acima. Ausência global [nao verificado].
- `JustificativaAdmin` herda diretamente de `admin.ModelAdmin`, sem `SoLeitura`, sem `readonly_fields` e sem override de persistência no bloco; portanto há também a superfície genérica do Django admin, sem lógica própria de dia, cobrança ou envelope. `app/ponto/admin.py:18`. Sua utilização efetiva em produção [nao verificado].
- Não encontrei form Python escritor de `Justificativa`; `app/ponto/forms.py` contém `AusenciaForm`, não justificativa. `app/ponto/forms.py:41`; ausência global [nao verificado].

3. Mutadores de decisão/automáticos, distintos da entrada da mensagem

| Escritor | Campos/efeitos |
|---|---|
| `aprovar_justificativa_dp` | `batida_gerada`, status aprovada, usuário e instante de aprovação; salva; invalida fechamento e encerra origem. `app/ponto/services/justificativa.py:132`, `app/ponto/services/justificativa.py:204`, `app/ponto/services/justificativa.py:214` |
| `rejeitar_justificativa_dp` | Status rejeitada, usuário, instante, motivo; salva e abre chamado módulo **`justificativa`**, não `justificativa_colab`, com dia em contexto. `app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:283`, `app/ponto/services/justificativa.py:291` |
| `materializar_saida_dp` | Batida gerada, saída informada, status aprovada, usuário e instante; depois fecha cascata do turno e origem. `app/ponto/services/justificativa.py:331`, `app/ponto/services/justificativa.py:380`, `app/ponto/services/justificativa.py:393`, `app/ponto/services/justificativa.py:408` |
| `aprovar_lote_justificativas` | Grava diretamente status aprovada, usuário, instante e eventual batida gerada; encerra origem. O próprio comentário registra dívida por gravar fora da porta de aprovação. `app/ponto/views.py:1763`, `app/ponto/views.py:1796`, `app/ponto/views.py:1800`, `app/ponto/views.py:1831`, `app/ponto/views.py:1837` |
| `rejeitar_lote_justificativas` | Grava diretamente status rejeitada, usuário, instante e motivo; manda push. `app/ponto/views.py:1859`, `app/ponto/views.py:1874` |
| `resolver_cobranca_da_entrada` | Auto-rejeita justificativa pendente vinculada e acrescenta nota à descrição depois que a cobrança reconcilia. `app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:31`, `app/ponto/services/entrada_sincronizada.py:37` |
| Resolver de batida em triagem | Auto-rejeita pendente vinculada e acrescenta nota à descrição após reconciliar cobrança. `app/ponto/services/triagem_batida.py:588`, `app/ponto/services/triagem_batida.py:597`, `app/ponto/services/triagem_batida.py:603` |
| Retratação do reconciliador | Auto-rejeita pendente vinculada, acrescentando nota à descrição, dentro do atomic da retratação. `app/chamados/reconciliador.py:522`, `app/chamados/reconciliador.py:529` |

4. Modelo e efeito comum dos saves

- `Justificativa` guarda pessoa, entrada, `data_turno` (DateField), tipo, `saida_informada` (DateTimeField), descrição, coordenadas, decisão, batida gerada, origem e timestamps. `app/ponto/models.py:115`, `app/ponto/models.py:136`, `app/ponto/models.py:144`, `app/ponto/models.py:149`, `app/ponto/models.py:154`, `app/ponto/models.py:159`, `app/ponto/models.py:167`, `app/ponto/models.py:173`.
- Há unicidade por `(colaborador,data_turno,batida_entrada)` e uma pendente por `(colaborador,data_turno)`. `app/ponto/models.py:187`, `app/ponto/models.py:189`.
- Todo `post_save` de `Justificativa`, salvo `raw`, manda julgar exatamente `instance.data_turno` no cartório; esse é efeito adicional dos saves acima, separado da abertura explícita de envelope. `app/ponto/signals.py:161`.
- Há comando destrutivo explícito que apaga todas as justificativas; não é escritor de mensagem de colaborador. `app/ponto/management/commands/zerar_dados_operacionais.py:95`.

5. Barreira do GET atual

- `abrir_justificativa` redireciona qualquer GET sem uma justificativa pendente do colab para `chamados:abrir`; POST ou GET com pendência entra no fluxo. Assim, apenas mudar o href do dashboard ainda atravessaria esse redirecionamento. `app/ponto/views.py:1552`, `app/ponto/views.py:1561`, `app/ponto/views.py:1564`.

## 11/11 12:22 · sessao e9c852c5cec4 · - · - · origem=Ronald (tmux)

Complemento do censo, separando ofertas diretas de status e ações sobre fatos:

| Controle adicional | Efeito confirmado e caminho |
|---|---|
| Iniciar disputa dos órfãos | `app/templates/chamados/partials/modal_fio.html:628` → `app/chamados/services/fio.py:549` → `app/chamados/services/acoes_disputa.py:222` → `app/chamados/services/disputa_emissao.py:1085` e `:1159`: `analisar()` nos chamados filhos. |
| Pedir confirmação de âncora | modal `:644` → `acoes_disputa.py:189` → `disputa_emissao.py:1379`: `chamado_limbo.analisar()`. |
| Aplicar declaração escrita no fio | modal `:140` → `app/chamados/views.py:478` → `app/chamados/services/declaracao_texto.py:696`: `validar_pergunta()`, que entra no caminho de materialização/reconciliação já indicado. |
| Corroborar régua | modal `:566` → `views.py:438` → `app/escala/regua_defesa.py:662`: grava confiança/corroboracão do vínculo e acrescenta nota em `:672`; não escreve status diretamente nesse corpo. Efeitos posteriores sobre estado: `[nao verificado]`. |
| Realocar régua | modal `:575` → `views.py:449` → `regua_defesa.py:329` → `:321`, `executar_vinculo`; este chama reconciliação posterior em `app/colaboradores/services/vinculo.py:329`, `:330`. |
| Reabrir questionário | modal `:368`, detalhe local `:9` → `views.py:1372`/`:1420` → `app/chamados/services/acoes_disputa.py:102`, `:103` → `disputa_emissao.py:1593`, `:1632`: zera resposta/carimbos da **Pergunta**, e acorda células em `:1633`. Não é o mesmo ato de reabrir diretamente `status_local`; efeitos posteriores: `[nao verificado]`. |
| Desligar colaborador | `app/templates/colaboradores/editar.html:271` → `app/colaboradores/views.py:1434` → `app/colaboradores/services/desligamento.py:150`, `:155`: encerra chamados pelo modelo. |
| Salvar vínculo de escala | `app/templates/colaboradores/partials/painel_vinculo.html:136`, `:175` → `app/colaboradores/services/vinculo.py:329`, `:330` → `app/colaboradores/services/pos_vinculo.py:42`: reconcilia chamado de adesão. O ramo legado `modo=aceite` encerra chamados anteriores em `vinculo.py:387`. |
| Regularizar retorno | `app/ponto/views.py:3153`, `:3215` → `app/ponto/services/regularizacao.py:126`, `:137`: põe chamados relacionados em análise. Rotas web/API: `app/ponto/urls.py:45`, `app/api/urls.py:106`. |

Nenhuma dessas ações oferece uma lista de códigos de `status_local`; o selo se restringe a opções HTML e às tags dos cinco seletores, em `app/chamados/tests/test_bo_rotulo_encerrado.py:41`, `:52`, `:99`, `:117`, `:127`. Cobertura dessas ações pelo selo: **não**, conforme esses percursos do teste.

O censo integral de todo efeito indireto de qualquer alteração de fato do sistema permanece `[nao verificado]`; o censo confirmado de controles que **oferecem estados como destinos** continua sendo os cinco seletores da primeira resposta, com as mesmas referências.

## 11/11 12:22 · sessao 21dc35eb6d42 · - · - · origem=Ronald (tmux)

Selos/tripwires encontrados, sem executar testes e sem escrever arquivos: [nao verificado]

- **Paridade dos kernels:** `test_contract_paridade_kernels` extrai todas as funções de topo de `api/views.py` e `api/views_core.py`, compara a AST normalizada dos nomes presentes nos dois e exige igualdade; `DIVERGENCIA_CONSCIENTE` está vazio. Uma alteração somente no corpo de `api_justificativas` de um kernel seria acusada; uma função nova somente em um arquivo não entra na interseção de nomes. `app/api/tests/test_contract_paridade_kernels.py:12`, `:18`, `:25`, `:31`, `:36`.

- **Não encontrei selo atual que obrigue as portas de submissão a chamar uma única função de serviço.** Essa ausência global permanece **[nao verificado]**; os selos localizados abaixo têm outros escopos. `Justificativa` não está no mapa completo `FAMILIA_DE`; entidades fora do mapa são declaradas “sem família”. Portanto o gate da família `chamado` não vigia a criação de `Justificativa` como entidade sua. `app/core/portas.py:38`, `:40`, `:87`; o gate ignora entidades cuja família não seja `chamado` em `app/chamados/tests/test_chokepoint_chamado_gate.py:320`.

- **Gate do chamado:** `ALLOWLIST=()`, `TETO_POR_ENTIDADE={}` e os testes exigem zero escrita fora dos módulos registrados em `core.portas`. Uma função nova de serviço que grave diretamente `ChamadoColaborador` fora dessas portas seria acusada quando o censo conseguir atribuir a escrita à entidade; chamar `ChamadoColaborador.abrir` e a porta de vínculos preserva o caminho já registrado. `app/chamados/tests/test_chokepoint_chamado_gate.py:56`, `:312`, `:316`, `:347`, `:371`; portas de `ChamadoColaborador` em `app/core/portas.py:196` e `:217`.

- **Porta de vínculo já protegida:** testes exigem que `apontar_justificativa` preencha vínculo ausente, devolva `True`, preserve o primeiro vínculo diante de segunda justificativa e devolva `False` nesse segundo apontamento; `None` é erro. Esses testes não elegem o dia nem a cobrança. `app/chamados/tests/test_porta_vinculos.py:130`, `:136`, `:172`; limite do juiz de dia declarado em `:44`.

- **Deduplicação das duas portas:** os testes enviam POST web/API depois de existir a justificativa automática e exigem exatamente uma justificativa e `chamado_origem` apontando ao `turno_aberto`. Uma função nova continua sujeita a esses efeitos observáveis; o teste não impõe nome nem escritor único. `app/ponto/tests/test_justificativa_dedup.py:56`, `:65`, `:71`, `:80`.

- **Cross-day web:** o teste cria chamado no dia seguinte ao `data_turno`, envia a justificativa para o dia do turno e exige vínculo com esse chamado. Ele cobre `contexto_json.data_turno`; o caso específico “dia encontrado somente pela célula/outros degraus do juiz” não aparece nesse teste. `app/ponto/tests/test_vinculo_justificativa_cross_day.py:30`, `:38`, `:44`, `:48`, `:56`.

- **Unicidade física:** constraint parcial limita a uma pendente por `(colaborador,data_turno)`, além do `unique_together(colaborador,data_turno,batida_entrada)`; testes exigem `IntegrityError` para segunda pendente e permitem uma nova quando a anterior foi aprovada. `app/ponto/models.py:187`, `:189`; `app/ponto/tests/test_justificativa_pendente_unica.py:29`, `:35`.

- **GET de justificativas/API e juiz de batidas:** AST exige que `api_justificativas` e `api_espelho_v2` chamem `dia_das_batidas`/`_ddb_j`; proíbe `strftime('%Y-%m-%d')` dentro de `api_justificativas`, exige a lápide de ausência de fallback e mantém `localtime` para a hora. Uma extração que retirasse essas chamadas do corpo da função seria acusada mesmo delegando a um helper. Esse selo olha `api.views`, com o gêmeo coberto separadamente pela paridade. `app/api/tests/test_o14_app_le_o_juiz_do_dia.py:27`, `:48`, `:60`, `:90`, `:100`.

- **Formulário/lista web:** teste existente exige que a lista `turnos_abertos` ofereça a entrada real de um turno com intervalo, nunca a volta do intervalo; sua fixture cria uma justificativa pendente para permitir o GET. `app/ponto/tests/test_turnos_abertos_nucleo.py:46`, `:58`, `:64`, `:76`.

- **Juiz do dia do chamado:** o selo AST próprio proíbe `.get('data_turno')` e `['data_turno']` fora do dono, com pendências vazias, mas sua raiz de varredura é apenas **`app/chamados/`**. Uma função nova em `ponto/services/` não é alcançada por esse selo específico. Ele também fixa os quatro degraus do juiz. `app/chamados/tests/test_contract_dia_do_fato.py:33`, `:42`, `:59`, `:83`, `:98`, `:124`. O contrato S5 chama um varredor de autoridades e tem contraprova de dia cru em `ponto/novo.py`, além de exigir uma única definição de `COBRANCA` no motor. `app/chamados/tests/test_contract_juiz_chamado.py:43`, `:49`, `:71`. Os padrões e exceções declarados estão em `app/core/juizes.py:1409` e `:1460`; não atribuir cobertura universal desses padrões sem executar suas verificações **[nao verificado]**.

- **Aprovação/fechamento da origem:** AST varre produção e exige que toda função que atribui `<x>.status='aprovada'` chame `encerrar_chamado_de_origem` depois do `save` e dentro do mesmo `with ...atomic()`. Há reconhecimento explícito de três escritores: `aprovar_justificativa_dp`, `materializar_saida_dp`, `aprovar_lote_justificativas`. Uma função nova que apenas recebe/submete pendente não é acusada por esse critério; uma que também aprove passa a sê-lo. `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:47`, `:94`, `:110`, `:127`, `:145`. Limites explícitos: não vê `update(status='aprovada')`, `create(status='aprovada')` ou o admin genérico; a presença de chamada sob ramo morto pode passar. `:8`, `:12`.

- **Estado/juiz novo:** P13 enumera campos de estado sancionados e acusa arquivos novos escrevendo esses campos; não é censo específico de submissão de justificativas. `app/core/tests/test_contract_escritores_estado.py:15`, `:180`, `:202`. A trava juiz novo só compara autoridades registradas em `core/juizes.py` com a base e exige frase de nascimento no `CORTES.md`; criar uma função de serviço, por si só, não corresponde ao critério do script. `bin/tests/test_juiz_novo_tem_corte.sh:27`, `:37`, `:41`; regra documental em `CLAUDE.md:74`.

- **Exceções engolidas:** `_abrir_justificativa_fluxo` e `abrir_justificativa` estão nominalmente no passivo. O teste acusa views novas engolindo `Exception` e exige retirar entradas do passivo que deixem de corresponder ao código; extrair um bloco desse tipo pode afetar a verificação de “passivo podre”. `app/core/tests/test_contract_excecoes_engolidas.py:45`, `:99`, `:115`, `:121`.

O teste chamado `test_contract_mensageria_le_o_juiz` vigia exclusivamente `api/views_mensageria.py` e os modelos `ChamadoColaborador`, `Batida`, `Ausencia`; o nome não o torna um selo da obra MSG-0/submissão de justificativas. `app/api/tests/test_contract_mensageria_le_o_juiz.py:49`, `:52`, `:138`.

## 11/11 12:22 · sessao 979168e33aec · - · - · origem=Ronald (tmux)

Sem enfraquecimento demonstrado na allowlist ou nos três mutantes E3 deslocados; há limites de cobertura nas cascas e na cor. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:81`, `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:154`)

- **Bloqueia o aceite integral do selo:** “renderiza as duas cascas” foi substituído por inspeção do grafo de includes, strings JavaScript e rota; o ORM renderiza apenas o partial pela view. O próprio docstring declara essa escolha. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:83`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:5`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:98`)
- **Fila — cor sem mordida:** o template atualmente lê a cor da origem, mas o selo AST não exige essa leitura e o ORM compara texto, números e badge da API, sem verificar o estilo do painel. Um mutante em memória com cor fixa passou os sete testes AST; execução ORM desse mutante: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_colaborador.html:190`, `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:39`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:114`)
- **AST morde os quatro mutantes fornecidos:** executei sete testes verdes; `painel_query` e `template_rotulo` produziram 1 failure cada; `api_ui_precedencia` e `api_core_precedencia`, 3 cada; todos sem errors. Os alvos e as assertivas estão em `app/colaboradores/tests/ui_painel_fontes.py:19` e `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:25`.
- **E3 preservado:** os mutantes de disputa agora corrompem apenas `questionarios_pendentes`; a matriz existente continua exigindo 2 perguntas na mesma disputa e conferindo esse campo. O mutante de autor mudou para o local real da query; a matriz distingue um chamado do DP de dois do colaborador. O selo novo exige failure sem error para os três alvos. (`app/chamados/tests/e3_mutantes.py:23`, `app/chamados/tests/e3_mutantes.py:87`, `app/chamados/tests/test_e3_app_orm.py:41`, `app/chamados/tests/test_e3_app_orm.py:47`, `app/chamados/tests/test_e3_app_orm.py:62`, `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:154`)
- **Tripwire preservado:** dois sítios API viram um sítio do serviço; teto cai para 25, `SEM_JUIZ` permanece 1, e continuam as verificações de sítio novo, declaração sobrando, teto e testemunhas. Executei os 18 testes com `SimpleTestCase` substituído por `unittest.TestCase` em memória: verdes; execução Django real: **[nao verificado]**. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:81`, `app/core/tests/test_tripwire_contagem_de_pendencia.py:147`, `app/core/tests/test_tripwire_contagem_de_pendencia.py:370`)
- **ORM e mutantes ORM:** execução nesta cópia **[nao verificado]**, pois `import django` retornou `ModuleNotFoundError`. O código cobre perguntas na mesma disputa, precedência sobre mensagem, chamados versus followups, vazio, origem variável, identidade dos itens e ausência de carimbo de leitura. (`app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:114`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:147`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:163`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:180`)

## 11/11 12:21 · sessao 67a589fcc055 · - · - · origem=Ronald (tmux)

1. Há UMA regra do badge dos três leitores exigidos: `projecao_do_app` escreve `questionarios_pendentes`, `chamados_nao_lidos` e o dicionário `{count, cor, rotulo}` em `app/chamados/services/pergunta_no_app.py:89-104`. Os leitores apenas propagam: `app/api/views.py:315-319`, `app/api/views_core.py:303-307`, `app/colaboradores/views.py:940-941` e contexto `:1006`; o template lê os campos prontos em `app/templates/colaboradores/partials/painel_colaborador.html:190-198`.

   A afirmação de unicidade **global** exige ressalva: o dashboard web ativo continua contando perguntas por `len(consulta_do_app(...))` em `app/core/views.py:252-253` e decidindo texto/cor/número no template `app/templates/core/dashboard_colaborador_v2.html:78-80`; esse template é usado em `app/core/views.py:270-271`. É outra derivação preexistente, fora do diff informado. **Fila**, caso “app” inclua essa superfície; não comprova nova duplicação introduzida pela obra.

   O JS de abas decide visibilidade e cores de seleção de abas, não o badge do app: `app/templates/core/_drawer_generico.html:118-145`. Código Android/iOS e seu rendering efetivo: **[nao verificado]**.

2. Pelo diff fornecido, a extração preserva os três campos da resposta e a precedência: perguntas pendentes vencem mensagens não lidas; depois `None`. Hoje a regra está em `app/chamados/services/pergunta_no_app.py:90-104`; os endpoints expõem as mesmas três chaves em `app/api/views.py:358-360` e `app/api/views_core.py:346-348`. Antes/depois esperado:

   - 2 perguntas, inclusive na mesma disputa, e mensagem não lida → `Responder DP`, `2`, `#f59e0b`.
   - Nenhuma pergunta, 1 chamado vivo com nota DP não lida → `Chamados`, `1`, `#ef4444`.
   - Nenhum dos dois → `chamados_badge=None`, contadores zero.

   A implementação atual está em `app/chamados/services/pergunta_no_app.py:89-104`; comparação histórica independente do anexo **[nao verificado]**, porque esta cópia não contém `.git`. A ordem das leituras de banco mudou: o diff fazia contagem de não lidos antes da consulta C6; hoje chama C6 primeiro (`:89`), depois conta (`:91-96`). A prioridade do badge permaneceu igual (`:97-102`); igualdade sob concorrência durante essas leituras **[nao verificado]**.

3. Unidades e escritores/leitores:

| Campo | Escritor e significado | Leitores |
|---|---|---|
| `pendentes`, `historico` | `consulta_do_app`, pergunta C6 viva, separada por resposta e disputa fechada; pessoa inteira, sem corte de vínculo/mês: `app/chamados/services/pergunta_no_app.py:45-75` | projeção `:89`; `/chamados/` `app/api/views.py:992-1028`; perguntas `app/api/views.py:2530-2532`; painel `app/templates/colaboradores/partials/painel_colaborador.html:193-198` |
| `questionarios_pendentes` | Número de **perguntas**, não de disputas: `app/chamados/services/pergunta_no_app.py:90` | dois `/me/` `app/api/views.py:318,359`, `app/api/views_core.py:306,347`; botão soma esse valor `app/templates/colaboradores/partials/painel_colaborador.html:39-40` |
| `chamados_nao_lidos` | Número de **chamados vivos** da pessoa com ≥1 nota DP não lida, não número de notas: `app/chamados/services/pergunta_no_app.py:91-96`; `VIVOS` inclui registrado em `app/chamados/catalogo/motor.py:24` | APIs `app/api/views.py:317,358`, `app/api/views_core.py:305,346`; escolha do badge `app/chamados/services/pergunta_no_app.py:99-100` |
| `count`, `cor`, `rotulo` | Projeção `app/chamados/services/pergunta_no_app.py:97-104`; `count` usa a unidade indicada pelo rótulo | APIs propagam `app/api/views.py:319,360`, `app/api/views_core.py:307,348`; faixa lê `app/templates/colaboradores/partials/painel_colaborador.html:190-191` |
| `no_app` | Variável local e contexto, não coluna persistida: `app/colaboradores/views.py:941,1006` | Template `app/templates/colaboradores/partials/painel_colaborador.html:39,190-198` |
| `chamados` do painel | Últimos **5 chamados de qualquer status**, sem filtro de não lido: `app/colaboradores/views.py:936-939,1005` | botão `app/templates/colaboradores/partials/painel_colaborador.html:39-40`; lista mostra até 5 followups por chamado `app/templates/colaboradores/partials/msgs_lista.html:1-11` |

   Não há troca detectada de significado dos campos do endpoint. O botão “Mensagens” usa outra medida autorizada: últimos cinco chamados + perguntas; não deve ser confundido com o `count` da faixa (`app/colaboradores/views.py:936-939`; template `:39-40,190-191`).

4. Para os insumos persistidos: notas nascem por `nota.anexar_ocorrencia`, que grava autor e conteúdo em `app/chamados/services/nota.py:75-109`; leitura do colaborador é carimbada por `marcar_lidas`, `:126-133`, chamado pelo GET `/chamados/` em `app/api/views.py:1037-1038`. O painel novo não chama essa porta; somente lê a projeção (`app/colaboradores/views.py:940-941`). Pergunta/texto e dia nascem em `PerguntaDisputa._nascer`, com escrita do dia por `gravar_dia_do_fato`, `app/chamados/models.py:2825-2828,3013-3017`; marco/identidade são derivados na inserção em `:3425-3447`. O dia exibido é o dia persistido da pergunta, não “hoje” nem mês do Espelho (`app/templates/colaboradores/partials/painel_colaborador.html:198`).

5. **Fila — faixa fica desatualizada após enviar mensagem pelo próprio painel.** O form substitui apenas `#msgs-lista` (`app/templates/colaboradores/partials/painel_colaborador.html:203-205`); o POST cria/reutiliza chamado e grava nota DP (`app/chamados/views.py:973-986`), devolvendo somente a lista (`:995-1003`). Exemplo dedutível: aberto sem nada, envio cria nota → próximo `/me/` tem `Chamados,1` pela regra `app/chamados/services/pergunta_no_app.py:91-100`, enquanto a faixa permanece `nada pendente` do primeiro render (`app/templates/colaboradores/partials/painel_colaborador.html:190-191`). Reprodução em navegador **[nao verificado]**.

## 11/11 12:21 · sessao e89b8a12ffef · - · - · origem=Ronald (tmux)

- **Questão 3:** o aviso do painel lê `projecao_do_app(colab)` e recebe `no_app` no contexto; não monta consulta própria de pendências. A projeção chama `consulta_do_app`, que lê `perguntas_do_app` e reaproveita `q_vivas`/`pergunta_viva`. As portas estão em `app/colaboradores/views.py:940`, `app/colaboradores/views.py:1006`, `app/chamados/services/pergunta_no_app.py:89`, `app/chamados/services/pergunta_no_app.py:70` e `app/chamados/services/pergunta_no_app.py:47`.

- **Universos diferentes, explicitáveis:** a consulta antiga `chamados` continua sendo os últimos cinco chamados da pessoa, sem filtro de status; o contador da aba soma essa lista às perguntas pendentes. Portanto, esse contador pode diferir do `count` do aviso — comportamento coerente com a soma pedida no aval. `app/colaboradores/views.py:937`; `app/templates/colaboradores/partials/painel_colaborador.html:39`. O aviso lê diretamente cor/rótulo/count da projeção e lista perguntas sem controles de escrita. `app/templates/colaboradores/partials/painel_colaborador.html:190`.

- **Unidades:** `questionarios_pendentes` conta perguntas individuais, inclusive várias na mesma disputa; `chamados_nao_lidos` conta chamados vivos com ao menos um followup DP sem leitura, não quantidade de notas. Ambos são por pessoa, sem filtro de vínculo ou dia; `VIVOS` inclui `registrado`. `app/chamados/services/pergunta_no_app.py:49`; `app/chamados/services/pergunta_no_app.py:69`; `app/chamados/services/pergunta_no_app.py:90`; `app/chamados/services/pergunta_no_app.py:91`; `app/chamados/catalogo/motor.py:24`.

- **Questão 6:** não identifiquei segundo juiz ou escritor persistente introduzido nessa projeção. Ela reaproveita a seleção C6 e o catálogo VIVOS; só cria um dicionário de apresentação. C6 está registrado como `chamados/juizes.py::pergunta_viva`, e a tranca como C20. `app/chamados/services/pergunta_no_app.py:78`; `app/core/juizes.py:278`; `app/core/juizes.py:292`. O registro vivo prevalece sobre o lookup segundo `app/docs/JUIZES.md:10`.

- **Relógio:** a função nova não lê relógio; o painel lê na borda da view. O caminho herdado do C6 possui fallback `date.today()`, mas esse trecho não é alterado pelo diff fornecido. `app/chamados/services/pergunta_no_app.py:78`; `app/colaboradores/views.py:930`; `app/chamados/juizes.py:569`. Não atribuir esse relógio à obra nova.

- **Tela/Espelho:** o código mantém quatro abas, o aviso entra no topo de Mensagens, e Espelho conserva seu container/carregamento próprio. `app/templates/colaboradores/partials/painel_colaborador.html:29`; `app/templates/colaboradores/partials/painel_colaborador.html:181`; `app/templates/colaboradores/partials/painel_colaborador.html:188`; `app/templates/core/_drawer_generico.html:132`.

- **Fila — linha Haiku incompleta:** os leitores de produção encontrados são somente os dois kernels e o painel; a pergunta pedida sobre “o que o app mostra agora” não aparece no golden. O caso existente pergunta sobre pergunta histórica em um dia, outro contrato. `app/api/views.py:315`; `app/api/views_core.py:303`; `app/colaboradores/views.py:940`; `eval/golden.py:5`; `eval/golden.py:35`; `eval/golden.py:347`.

- Smoke de clique Ronald e decisão de encaixe da fila 2 no RELATO: **[nao verificado]**. Histórico Git/base disponível localmente: **[nao verificado]**; a pasta desta cópia não é repositório Git.

## 11/11 12:19 · sessao ba041376f50f · - · - · origem=Ronald (tmux)

Q3 — `DESTINOS` reproduz os cinco seletores e suas ordens: quatro destinos nos dois detalhes e três nos demais; o patch fornecido preserva os `value` e a ordem da base. Fontes atuais: `app/chamados/tests/test_bo_rotulo_encerrado.py:64`, `app/templates/chamados/detalhe.html:130`, `app/templates/chamados/detalhe_local.html:393`, `app/templates/chamados/fio_colaborador.html:265`, `app/templates/chamados/partials/modal_fio.html:665`, `app/templates/chamados/partials/painel_chamado_turno.html:74`. Comparação da base decorre do anexo fornecido; checkout da base `[nao verificado]`.

Isso **não equivale a uma lista de transições sempre aceitas**. Os receptores locais reconhecem todo `STATUS_CHOICES` e delegam ao modelo (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:502`, `app/chamados/services/fio.py:719`). O modelo delega a decisão geométrica ao motor e aplica guardas de conteúdo (`app/chamados/models.py:119`, `app/chamados/models.py:128`, `app/chamados/models.py:135`, `app/chamados/models.py:148`, `app/chamados/models.py:181`).

**Fila — “Reabrir” pode ser recusado:** o rótulo envia `aberto`; `resolvido → aberto` não pertence à matriz humana comum, embora `em_analise → aberto` pertença. [nome] tem exceção; destino igual à origem continua recusado (`app/chamados/services/verbos.py:136`, `app/chamados/catalogo/motor.py:99`, `app/chamados/catalogo/motor.py:148`, `app/chamados/catalogo/motor.py:153`).

**Fila — detalhe GLPI oferece status sem aplicá-lo:** o formulário envia ao followup, cujo receptor lê somente conteúdo e envia a mensagem ao GLPI; não lê `novo_status` (`app/templates/chamados/detalhe.html:122`, `app/chamados/urls.py:12`, `app/chamados/views.py:292`, `app/chamados/views.py:304`, `app/chamados/views.py:310`).

Q4 — escritores/leitores:

- **Código/value:** chamadas literais dos cinco templates → `opcao_mover(status_local)` → `value=status_local`; receptores leem o código, verificam `STATUS_CHOICES`, chamam `transicionar` e finalmente `_gravar_estado` grava esse código. Nenhum desses caminhos interpreta o rótulo como código (`app/chamados/templatetags/chamados_tags.py:68`, `app/chamados/templatetags/chamados_tags.py:77`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:719`, `app/chamados/models.py:199`, `app/chamados/models.py:228`, `app/chamados/models.py:447`).
- **Rótulo da opção:** definição única em `MOVER_PARA`; `rotulo_de_movimento` lê pelo código e a tag escreve o texto HTML (`app/chamados/services/verbos.py:132`, `app/chamados/services/verbos.py:146`, `app/chamados/templatetags/chamados_tags.py:76`).
- **Title:** definição compartilhada `ESTADO_TITLE`; lida por `rotulo_de_movimento`, `rotulo_de_linha` e `rotulo_para_pessoa`; a tag escreve o atributo, enquanto as pílulas usam `esp.1` (`app/chamados/services/verbos.py:100`, `app/chamados/services/verbos.py:146`, `app/chamados/services/verbos.py:155`, `app/chamados/services/verbos.py:166`, `app/templates/chamados/detalhe_local.html:70`).
- **Filtro `espera`:** permanece delegado a `rotulo_para_pessoa`, que usa `ESPERA_COLAB`, onde resolvido/fechado compartilham “Encerrado”; continua nas pílulas e deixou as opções alteradas pelo patch. Isso responde à apresentação do estado atual, distinta da opção de movimento (`app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/templatetags/chamados_tags.py:64`, `app/chamados/services/verbos.py:114`, `app/chamados/services/verbos.py:162`, `app/templates/chamados/detalhe.html:36`, `app/templates/chamados/detalhe_local.html:68`, `app/templates/chamados/fio_colaborador.html:188`, `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/painel_chamado_turno.html:18`).

Q5 — não vejo **segundo juiz de transição** introduzido: o registrado continua `motor.decidir`, e a tag só consulta apresentação (`app/core/juizes.py:274`, `app/chamados/models.py:123`, `app/chamados/templatetags/chamados_tags.py:74`). `DESTINOS` congela a oferta de UI; não decide transições (`app/chamados/tests/test_bo_rotulo_encerrado.py:62`, `app/chamados/tests/test_bo_rotulo_encerrado.py:127`). Os mapas de movimento/pílula respondem a perguntas diferentes e seus consumidores permanecem separados, sem sobrecarga nova demonstrada (`app/chamados/services/verbos.py:125`, `app/chamados/services/verbos.py:140`, `app/chamados/services/verbos.py:162`).

O censo não pode ficar vazio e manter verde: há mínimo de cinco arquivos e comparação exata com `DESTINOS` (`app/chamados/tests/test_bo_rotulo_encerrado.py:95`, `app/chamados/tests/test_bo_rotulo_encerrado.py:131`). Entretanto, há candidato forte a **BLOQUEIA — selo sem mordida suficiente**: manter todas as tags e acrescentar, antes de `</select>` no detalhe local, duas opções com fechamento implícito:

```html
<option value="resolvido">Encerrado
<option value="fechado">Encerrado
```

O reconhecedor exige `</option>` e `DESTINOS` conta somente tags `opcao_mover`, logo esses acréscimos escapam à lógica estática (`app/templates/chamados/detalhe_local.html:398`, `app/chamados/tests/test_bo_rotulo_encerrado.py:23`, `app/chamados/tests/test_bo_rotulo_encerrado.py:104`, `app/chamados/tests/test_bo_rotulo_encerrado.py:111`, `app/chamados/tests/test_bo_rotulo_encerrado.py:127`). Execução da mutação e renderização em navegador `[nao verificado]`.

## 11/11 12:19 · sessao e9c852c5cec4 · - · - · origem=Ronald (tmux)

Censo direto da cópia: cinco seletores oferecem `status_local`, todos com `{% opcao_mover %}`, e todos entram em `DESTINOS` e nas verificações de literal/conjunto/ordem: `app/chamados/tests/test_bo_rotulo_encerrado.py:64`, `:99`, `:127`.

| Controle | Destinos na ordem | Receptor e cobertura |
|---|---|---|
| `app/templates/chamados/detalhe.html:130` | `em_analise,resolvido,fechado,aberto` | POST para `followup` em `:122`; `app/chamados/urls.py:12` → `app/chamados/views.py:292` e `:304`, que só envia conteúdo ao GLPI. **Fila:** status exibido mas ignorado nesse caminho. |
| `app/templates/chamados/detalhe_local.html:393` | mesmos quatro | `app/chamados/views.py:547` → `app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`; testa código em `STATUS_CHOICES` e chama `transicionar`. |
| `app/templates/chamados/fio_colaborador.html:265` | três primeiros | `app/chamados/views.py:669` → `app/chamados/services/fio.py:502`, `:709`, `:719`, `:721`; exige conteúdo e permissão contextual, depois transiciona. |
| `app/templates/chamados/partials/modal_fio.html:665` | três primeiros | `app/chamados/views.py:953` → mesmo processador do fio. |
| `app/templates/chamados/partials/painel_chamado_turno.html:74` | três primeiros | Form envia ao detalhe local em `:68`; mesmo receptor do detalhe local. |

A cobertura percorre **todos os arquivos `.html` sob `app/templates/`**, mas só inspeciona fonte: `_html` em `app/chamados/tests/test_bo_rotulo_encerrado.py:33`, censo em `:55` e duplicidade textual em `:120`. A proibição de opções literais fora de seletores `novo_status` só percorre `templates/chamados` (`:110`). Não percorre `.js`, Python ou forms Django (`:36`); portanto não protege futuros status construídos nesses lugares (`:33`, `:52`, `:117`).

Busca produtiva no repositório não encontrou outro seletor de destinos, JS que ofereça esses destinos nem choices montadas em views/forms; ausência absoluta de construção indireta: `[nao verificado]`. Os receptores encontrados leem somente o código (`acoes_chamado.py:287`; `fio.py:502`).

Controles de ações com efeito sobre o estado, **fora do selo de opções**, incluindo omissões do censo da v1:

| Ação | Controle e caminho |
|---|---|
| Aprovar/rejeitar benefício | `app/templates/colaboradores/painel_solicitacao_beneficio.html:23`, `:24`; também JS em `app/templates/colaboradores/fila_solicitacoes_beneficio.html:138`, `:139` → `app/colaboradores/views.py:2068`, `resolvido`/`fechado`. |
| Aceitar/recusar escala | `app/templates/chamados/detalhe_local.html:163`, `:169` → `app/chamados/services/acoes_chamado.py:149`, `resolvido`. |
| Aceitar/recusar agenda | mesmo template `:200`, `:209` → `acoes_chamado.py:108`, `resolvido`. |
| Aprovar exceção | detalhe local `:256`; modal `:598` → `acoes_chamado.py:70`, `resolvido`. |
| Autorizar geofence, autorizar com advertência, recusar | detalhe local `:285`, `:286`, `:287`; modal `:506`, `:517`, `:528` → `acoes_chamado.py:222`, `:277`, `resolvido`; preparação de `registrado → em_analise` em `:232`. |
| Abrir registro de ausência | detalhe local `:233` → `app/chamados/views.py:1513`, `analisar()`. |
| Reabrir administrativamente disputa | modal `:390` → `app/chamados/services/acoes_disputa.py:269`, `:270`, `reabrir(humano=True)`. |
| Resgatar chamado encerrado pela tranca | `app/templates/chamados/partials/_lista_chamados.html:85` → `app/chamados/views_arquivar.py:64`, `:70`, `reabrir(humano=True)`. |
| **Aceitar folga sugerida** | modal `:550` → `app/chamados/views.py:431` → `app/chamados/services/limbo_folgas.py:98`, `:101`, reconcilia os chamados do mesmo template. |
| **Validar resposta / validar assim mesmo** | modal `:265`, `:281`; também detalhe local roteia em `app/chamados/views.py:483` → `app/chamados/services/acoes_disputa.py:18` → `app/chamados/services/validacao.py:295`, `:296` → `app/ponto/services/veredito_celula.py:202`, reconcilia chamados do dia. |
| **Validar inline** | `_lista_chamados.html:82` → `app/chamados/views_cobrar.py:208`, mesma `acao_validar_pergunta`. |
| **Validar em lote** | `app/templates/chamados/painel_gestao.html:14`, `app/templates/chamados/partials/_lote_preview.html:21`; `app/chamados/views_cobrar.py:247`, `:261`, delega ao validador de lote. |
| **Regularizar disputa e fechar** | modal `:355` → `app/chamados/services/acoes_disputa.py:139`, materializa perguntas validadas; fio tem processamento em `app/chamados/services/fio.py:695`. Efeito exato de todo caso sobre `status_local`: `[nao verificado]`. |

Filtros de estado não são controles de transição: histórico em `app/templates/relatorios/historico_chamados.html:54` oferece `aberto/em_analise/resolvido/fechado` (`:56`–`:59`), e o receptor aplica filtro em `app/relatorios/views.py:920`. O teste global também enxerga essas opções, pois reconhece qualquer `value` presente em `ESTADOS` (`app/chamados/tests/test_bo_rotulo_encerrado.py:46`), independentemente de ser filtro.

## 11/11 12:19 · sessao c7e0251bc2ef · FIO-E0-V4-E4B · censo · origem=Code

**O achado 1 procede quanto à interrupção dos carimbos e da varredura de fecho. Há duas correções: o status do chamado já foi salvo quando S143 recebe `{0,0}`; e uma disputa que ainda contém pergunta viva já não poderia fechar por C7.** O retorno antecipado também impede examinar outras disputas abertas do colaborador, inclusive órfãs fecháveis. (`app/chamados/models.py:296`; `app/chamados/signals.py:115`; `app/chamados/models.py:1853`; `app/chamados/services/disputa_emissao.py:1946`, `:2021`, `:2086`)

**1. Censo dos chamadores e alcance da reabertura**

Há cinco chamadas de produção do resolver; `models.py` apenas reexporta a função. (`app/chamados/signals.py:115`; `app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`, `:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `app/chamados/models.py:4093`)

| Chamador | Gatilho e estado ao entrar | Pode alcançar reabertura/ressuscitação/S2? |
|---|---|---|
| `app/chamados/signals.py:115` | `post_save` de chamado em `ENCERRADOS`, quando o save escreveu `status_local`. Inclui encerramento novo e save nomeando o mesmo status encerrado. (`:75`, `:85`, `:90`, `:96`, `:104`) | Sim, passa pelo resolver após o save do chamado. (`app/chamados/models.py:296`; `app/chamados/services/disputa_emissao.py:1945`) |
| `app/chamados/services/becos.py:54` | Evento de pergunta ou command: reúne pai/destinos **já terminais**, incluindo cancelado/superado. (`:11`, `:21`, `:42`, `:46`) | Condicional. O evento exige `FECHAVEL`; uma viva na própria disputa impede essa entrada. O resolver pode encontrar perguntas de outras disputas ancoradas no mesmo chamado. (`:33`, `:74`; `app/chamados/models.py:344`; `app/chamados/services/disputa_emissao.py:1938`) |
| `app/chamados/reconciliador.py:536` | `_retratar`: só chega depois de `ch.reconciliar()` retornar verdadeiro. Portanto entra após o encerramento feito nessa operação; o save já acionou também S143. (`:522`, `:523`, `:536`; `app/chamados/models.py:654`, `:296`) | Sim, sujeito às guardas comuns abaixo. Não é, por si, varredura de passivo já encerrado: exige uma reconciliação aceita antes. (`app/chamados/reconciliador.py:523`; `app/chamados/services/disputa_emissao.py:1945`) |
| `app/chamados/reconciliador.py:1648` | `reatar_pergunta_orfa`: candidato histórico único vem de consulta a `ENCERRADOS`; exige reaponte aceito. É entrada explícita com chamado histórico resolvido/fechado. (`:1586`, `:1587`, `:1623`, `:1642`, `:1648`) | Sim, sujeito às guardas comuns. (`app/chamados/services/disputa_emissao.py:1945`) |
| `app/ponto/management/commands/reconciliar_fantasmas.py:281` | Somente apply; consulta inicial contém **apenas VIVOS**. Primeiro tenta `retratar`; chama explicitamente o resolver somente se `rec=False`. O comentário menciona chamado já resolvido, mas a consulta não seleciona esse passivo. (`:153`, `:157`, `:159`, `:269`, `:273`, `:280`, `:281`) | Pode executar a porta, mas não é uma varredura inicial de chamados fechados. O cron declarado usa `--alarme`, que desliga apply. (`:153`; `app/config/crons.py:392`) |

A porta comum exige pergunta sem `respondida_em`, viva pelo C6 e credor fora de `VIVOS`. Mede a carga inteira de relavra/ressuscitação, recusa competência exportada **ou indeterminada**, recusa limbo e consulta as guardas existentes de reabertura. Só então chama `credor.reabrir()`. (`app/chamados/services/disputa_emissao.py:1792`, `:1821`, `:1827`, `:1835`, `:1837`, `:1846`, `:1849`)

Quando aceita, a cadeia é efetiva: `reabrir` grava o estado e chama `ressuscitar_perguntas`; a mudança de fronteira de `ENCERRADOS` permite relavra S2 das perguntas por destino; a ressuscitação limpa carimbos e pode reabrir disputas fechadas. O resolver usa `executar=True`, sem DRY próprio. (`app/chamados/models.py:1440`, `:1452`, `:273`, `:303`, `:312`, `:1562`, `:1575`; `app/chamados/services/disputa_emissao.py:1805`, `:1945`)

Os gatilhos indiretos encontrados são:

| Entrada | Caminho |
|---|---|
| Batida salva | Signal → reconciliação do colaborador → retratação e reata de órfãs. (`app/ponto/signals.py:206`, `:214`; `app/chamados/reconciliador.py:1432`, `:1464`, `:1526`) |
| Filho deixa `VIVOS` | Signal agenda a mesma reconciliação após commit. (`app/chamados/signals.py:349`, `:366`, `:369`) |
| Pergunta salva | Signal agenda `fechar_beco_da_disputa` após commit. (`app/chamados/signals.py:227`, `:237`, `:238`) |
| Emissão de pergunta órfã | Emissor chama `reatar_pergunta_orfa`. (`app/chamados/services/disputa_emissao.py:459`, `:465`) |
| Outbox de célula | Consumidor chama retratação por célula e por premissa; cron declarado a cada minuto. (`app/chamados/reconciliador.py:1368`, `:1372`; `app/config/crons.py:394`) |
| Ação humana no detalhe/fio | Serviços transitam o chamado → núcleo salva → S143. (`app/chamados/views.py:423`; `app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`, `:315`; `app/chamados/services/fio.py:719`; `app/chamados/models.py:450`, `:296`) |
| Justificativa e geofence | Aprovação de justificativa e aplicação de geofence delegam a `retratar`. (`app/ponto/services/justificativa.py:46`; `app/ponto/services/geofence.py:441`) |
| Outros commands | Alertas de turno, grade, vínculo e lei-cala delegam a `retratar`; os três últimos condicionam a escrita a apply. (`app/ponto/management/commands/processar_alertas_turno.py:216`; `app/ponto/management/commands/reconciliar_grade.py:305`, `:309`; `app/ponto/management/commands/reconciliar_vinculo.py:58`, `:68`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:54`, `:59`) |

Os crons declarados de **fantasmas**, **reconciliar chamados** e **becos** usam `--alarme`; essa declaração não demonstra execução mutante do resolver. Alertas de turno têm agendamento `*/5`. Execução real desses crons: **[nao verificado]**. (`app/config/crons.py:362`, `:392`, `:395`, `:663`; `app/ponto/management/commands/reconciliar_fantasmas.py:153`; `app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54`)

Há ainda um chamador direto da **porta de reabertura**, sem passar pelo resolver: o backfill E4B. Ele planeja com `executar=False`, executa em `_executar_dia` e possui DRY com rollback e apply cercado. Portanto esse command não deve ser confundido com os eventos comuns. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:90`, `:135`, `:175`, `:300`, `:308`, `:353`)

**ANTES versus DEPOIS:** a comparação literal com `fb9269ea9`, inclusive por chamador, fica **[nao verificado]**: o corpo dessa árvore base não está disponível no material consultado. O comentário atual declara que a base já carregava S128; o recibo descreve diferenças de escopo de `tranca_da_consulta`, mas não substitui o código antigo. (`app/chamados/services/disputa_emissao.py:1926`; `app/docs/FIO-E0-V4-E4B-c4-v14.txt:17`)

Na cópia atual, a ordem comprovada é **tentar devolver alcance → eventualmente retornar `{0,0}` → somente depois viriam S128, HX-RESPOSTA-VISIVEL, HX-DISPUTA-ORFA e C7**. Atribuir precisamente a introdução de cada trecho ao diff informado permanece **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1945`, `:1947`, `:1981`, `:1998`, `:2021`, `:2086`)

**2. Todos os consumidores do retorno e comportamento com `{0,0}`**

| Consumidor | O que faz |
|---|---|
| S143 | Descarta o retorno. O status já foi salvo; a recusa não provoca rollback. (`app/chamados/signals.py:115`; `app/chamados/models.py:296`) |
| `fechar_beco` | Descarta o retorno, relê a disputa e ainda tenta fechá-la se C7 permitir. Portanto a recusa do resolver não impede necessariamente esse fecho posterior. (`app/chamados/services/becos.py:54`, `:55`, `:56`, `:57`) |
| `_retratar` | Descarta o retorno, anexa ocorrência de retratação e retorna `True`. (`app/chamados/reconciliador.py:536`, `:538`, `:541`) |
| `reatar_pergunta_orfa` | Descarta o retorno e retorna `True` pelo reaponte aceito; isso não comprova carimbo nem fecho de disputa. (`app/chamados/reconciliador.py:1642`, `:1648`, `:1649`) |
| `reconciliar_fantasmas` | **Único leitor das contagens:** com `rec=False` e `{0,0}`, calcula `mudou=False`, pula ocorrência/evento e apenas imprime. Não grava status depois desse retorno. (`app/ponto/management/commands/reconciliar_fantasmas.py:281`, `:282`, `:283`, `:306`) |

O wrapper público apenas devolve o dict do corpo, sem interpretar recusa. (`app/chamados/services/disputa_emissao.py:1873`, `:1879`)

Assim, **sim, o código permite conservar chamado encerrado com disputas abertas após essa recusa**. A ordem principal, porém, é “salvou status → S143 → retorno ignorado”, e não “leu zero → depois gravou fechado”. O núcleo ainda pode executar sua relavra S2 após o receiver. (`app/chamados/models.py:296`, `:303`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1947`)

`{0,0}` também não significa ausência de escrita: a porta anexa ocorrência de recusa antes de devolvê-la. As duas contagens representam carimbos S128 e fechos; os carimbos do ramo HX de órfãs nem incrementam `perguntas_resolvidas`. (`app/chamados/services/disputa_emissao.py:1855`, `:1859`, `:1984`, `:2055`, `:2088`)

O fechamento comum pode chegar à recusa: F1 exclui exportadas/indeterminadas da lista que bloquearia o fechamento; a porta de reabertura recusa precisamente essa carga. O teste existente exige que a recusa interrompa outros carimbos e fechos. Ocorrência concreta em produção: **[nao verificado]**. (`app/chamados/models.py:364`, `:365`; `app/chamados/services/disputa_emissao.py:1835`, `:1946`; `app/chamados/tests/test_e4b_classe_e_memoria.py:960`, `:970`, `:972`)

A correção do contraexemplo é esta: **viva exportada + mortas na mesma disputa demonstra mortas sem carimbo; não demonstra que essa mesma disputa deveria fechar**, porque C7 já barra sua pergunta viva. O alcance adicional perdido é a varredura das demais disputas abertas do colaborador. (`app/chamados/models.py:1853`; `app/chamados/services/disputa_emissao.py:1947`, `:1981`, `:2022`, `:2086`)

**3. Contadores/vigias e seus limites**

| Detector | O que perceberia |
|---|---|
| Auditor **I2** | Pergunta sem materialização apontando a chamado terminal; exclui do alarme a respondida em disputa aberta. Detecta o resíduo com destino terminal, mas não a órfã sem destino. (`app/chamados/management/commands/auditar_invariantes_chamados.py:587`, `:591`, `:594`) |
| Vigia de becos | Varre abertas e lista `FECHAVEL`/`BARRADA`; `--alarme` falha somente para `FECHAVEL`. Órfã sem destino e sem resposta fica fora do recorte; disputa com viva pode ser apenas listada como barrada. (`app/chamados/management/commands/fechar_becos_disputa.py:32`, `:39`, `:44`, `:67`; `app/chamados/services/becos.py:29`, `:33`) |
| Relatório de pendências | Aberta sem nenhuma pergunta viva aparece em `PRONTA_FECHAR`; é relatório, não guarda de fechamento. (`app/chamados/relatorios_disputa.py:18`, `:20`, `:41`) |
| `auditar_disputas` | Conta toda disputa aberta há mais de sete dias, sem identificar a causa da recusa. (`app/chamados/management/commands/auditar_disputas.py:132`, `:135`) |
| Alerta de pendências | Seleciona abertas e alerta colaborador/supervisão; cron declarado às 08h. Não identifica especificamente “órfã causada pela recusa”. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `:48`, `:73`, `:103`; `app/config/crons.py:745`) |
| `vigia_fio.censo_alcance` | Pode classificar viva sob chamado terminal, dentro dos dias retidos/competências a lavrar; não cobre universalmente órfãs mortas ou exportadas. (`app/chamados/services/vigia_fio.py:577`, `:587`, `:687`) |

Portanto, **não procede dizer que não existe detector algum**; os detectores têm recortes distintos e não comprovam cobertura universal dessa recusa. Execução e resultados na frota: **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:587`; `app/chamados/management/commands/fechar_becos_disputa.py:67`)

**Quem escreve e quem lê os campos omitidos**

O quadro abaixo identifica os escritores do caminho interrompido e os leitores funcionais relevantes; não é inventário de toda referência textual a esses campos. (`app/chamados/services/disputa_emissao.py:1981`, `:2055`, `:2087`)

| Campos | Quem escreveria | Quem lê |
|---|---|---|
| `materializada_em`, `materializada_por`, `via_resolucao` | S128 e HX chamam `carimbar`; a atribuição canônica fica em `_escrever_carimbo`. (`app/chamados/services/disputa_emissao.py:1981`, `:2055`; `app/chamados/models.py:3097`) | C6 lê carimbo/via; estado da pergunta lê materialização; materializador seleciona materialização nula. O autor também é lido em registros de auditoria. (`app/chamados/juizes.py:1342`, `:1368`, `:1405`; `app/chamados/models.py:3046`; `app/chamados/services/materializacao.py:1372`; `app/chamados/reconciliador.py:1145`) |
| `respondida_em`, `validada_em`, `validada_por` | `carimbar` pode preenchê-los com texto não vazio; S128 usa `resposta='validar'`, HX usa `'responder'`. (`app/chamados/models.py:3130`, `:3134`; `app/chamados/services/disputa_emissao.py:1982`, `:2055`) | C6, respostas sem veredito/C7, estado da pergunta e fila do materializador. (`app/chamados/juizes.py:1331`, `:1347`; `app/chamados/models.py:1843`, `:1853`, `:3041`; `app/chamados/services/materializacao.py:1369`) |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | S128 solicita relavra no mesmo save do carimbo. (`app/chamados/services/disputa_emissao.py:1982`; `app/chamados/models.py:3149`, `:3151`) | São a lavra persistida associada ao carimbo; a recusa pula essa chamada S128, mas não elimina a relavra independente do núcleo do chamado. (`app/chamados/models.py:3149`, `:303`, `:312`) |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | O laço final chama `d.fechar`; a porta grava os quatro juntos. (`app/chamados/services/disputa_emissao.py:2087`; `app/chamados/models.py:1932`, `:1936`) | `aberta`, consulta do app, lookup do fio e relatórios leem o estado aberto; signals usam fecho/autor/resolução para cascatas; a porta lê o conjunto para idempotência; o vigia conta motivo `vazia`. (`app/chamados/models.py:1824`, `:1880`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/fio.py:131`; `app/chamados/relatorios_disputa.py:41`; `app/chamados/signals.py:45`, `:136`, `:163`; `app/chamados/services/vigia_fio.py:734`) |

Por fim, o item dos tetos comentados **não procede nesta cópia**: 213 e 269 são entradas ativas e são cobradas por `assertLessEqual`. Resultado de execução desses testes: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:292`, `:294`)

## 11/11 12:17 · sessao c17d1b00836f · - · - · origem=Ronald (tmux)

- **Único leitor semântico do retorno:** `app/ponto/management/commands/reconciliar_fantasmas.py:281` recebe o dict; `:282` calcula `mudou` pelas duas contagens. Com `rec=False` e `{0,0}`, pula nota e evento (`:283-305`), apenas imprime (`:306-309`). Não grava status depois desse retorno. Os demais chamadores descartam o retorno: `app/chamados/signals.py:115`, `app/chamados/services/becos.py:54`, `app/chamados/reconciliador.py:536`, `:1648`.

- **Fechamento comum permanece gravado:** `_gravar_estado` salva o chamado em `app/chamados/models.py:296`; o S143 recebe o estado encerrado (`app/chamados/signals.py:96`) e ignora o retorno (`:115`). A recusa normal `{0,0}` não lança exceção nem determina rollback; após o receiver, `_gravar_estado` ainda pode relavrar S2 (`app/chamados/models.py:303-312`). Portanto a persistência do chamado terminal com disputas abertas é sustentada pelo código; não decorre de um leitor explícito dizendo “nada a fazer”.

- **`{0,0}` não significa zero escrita:** o helper grava ocorrência de recusa antes de retornar (`app/chamados/services/disputa_emissao.py:1855-1859`). O laço pode reabrir uma âncora anterior antes de encontrar outra que recusa (`:1941-1947`); o `return` não desfaz isso. As contagens só incrementam carimbo S128 (`:1984`) e fechamento de disputa (`:2088`); nem carimbos HX de órfãs (`:2055`) entram em `perguntas_resolvidas`.

**Vigias/contadores existentes e limites:**

| Leitor | O que perceberia |
|---|---|
| `app/chamados/management/commands/fechar_becos_disputa.py:32-49` | Varre disputas abertas; lista FECHAVEL/BARRADA. `--alarme` dá exit 2 somente para FECHAVEL (`:67-70`). A classificação exige pai terminal e nenhuma pergunta sem resposta com destino não terminal (`app/chamados/services/becos.py:25-33`). Uma órfã sem destino e sem resposta passa fora desse recorte; viva exportada na mesma disputa pode produzir BARRADA, listada sem alarme. Cron declarado às 07:10 em `app/config/crons.py:663`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1601-1605` | C2 conta pergunta morta pela célula com `materializada_em`, `validada_em`, `respondida_em` nulos e veredito não acusador. Não é detector universal de órfãs e o filtro não exige disputa aberta. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:805-807` | I7 só cobre disputa aberta com pai inexistente ou cancelado; pai resolvido/fechado não entra. |
| `app/chamados/relatorios_disputa.py:18-20` e `:40-47` | Disputa aberta sem nenhuma viva aparece em `PRONTA_FECHAR`; é relatório, não cura nem guarda. Comando `relatorio_pendencias_disputa` usa esse coletor (`app/chamados/management/commands/relatorio_pendencias_disputa.py:15`); declarado sob demanda em `app/config/crons.py:1448`. |
| `app/chamados/management/commands/auditar_disputas.py:132-143` e `:203` | Toda disputa ainda aberta há mais de sete dias entra em `disputas_paradas_7d`; não identifica especificamente a recusa. Sob demanda: `app/config/crons.py:1450`. |
| `app/ponto/management/commands/alertar_disputas_pendentes.py:33-35` | Toda aberta entra no alerta genérico: push colaborador (`:48-65`), agregado supervisão (`:67-70`, `:103-115`), `disputa_velha` se >7 dias (`:73-85`). Cron declarado 08:00 diário: `app/config/crons.py:745`. |
| `app/chamados/services/vigia_fio.py:577-587` e `:687-699` | `censo_alcance` mede dias retidos em competências a lavrar; pode classificar `pergunta_viva_sob_chamado_terminal`. Não garante perceber órfã morta ou pergunta exportada fora desse universo. |

Execução real dos crons: **[nao verificado]**.

**Escritores dos campos omitidos pelo retorno antecipado:**

- S128 chama `carimbar(resposta='validar', lavrar_veredito=True)`: `app/chamados/services/disputa_emissao.py:1981-1982`.
- HX órfã chama `carimbar(resposta='responder')`: `app/chamados/services/disputa_emissao.py:2055`.
- Porta canônica escreve `materializada_em`, `materializada_por_id`, `via_resolucao`: `app/chamados/models.py:3097-3101`; pode preencher `respondida_em`, `validada_em`, `validada_por` apenas com texto não vazio (`:3130-3137`). S128 também lavra `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`:3149-3152`).
- Fecho final chama `d.fechar`: `app/chamados/services/disputa_emissao.py:2086-2088`; a porta escreve `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` e evento: `app/chamados/models.py:1932-1940`.

**Leitores canônicos e efeitos desses campos:**

- C6: `validada_em` (`app/chamados/juizes.py:1331`), via/carimbo (`:1342`, `:1368`), resposta/carimbo (`:1347`), materialização (`:1374`, `:1405`).
- Estado da pergunta: `app/chamados/models.py:3041-3048`; pré-filtro `q_vivas`: `:3348-3359`.
- Respostas sem veredito e C7: `app/chamados/models.py:1842-1853`.
- Fila do materializador: `app/chamados/services/materializacao.py:1369-1373`; S144 seleciona materialização nula (`:1418-1419`).
- Disputa aberta: property `app/chamados/models.py:1823-1824`; apresentação app `app/chamados/services/pergunta_no_app.py:71`; lookup fio `app/chamados/services/fio.py:131-135`; relatório `app/chamados/relatorios_disputa.py:41`.
- `fechada_em` também dispara S88 para reconciliar pai (`app/chamados/signals.py:28-42`), retomada de regularizações (`:129-144`) e S144 (`:153-162`). Ao não fechar a disputa, esses eventos não são emitidos por esse caminho.

Esse último bloco é mapa dos leitores canônicos/efeitos relevantes, **não um censo literal de toda referência desses campos na árvore**.

**Correção importante sobre beco:** o evento só chama `fechar_beco` quando `classificar==FECHAVEL` (`app/chamados/services/becos.py:74-78`), e FECHAVEL depende de C7 (`:33`), que exige nenhuma viva na própria disputa (`app/chamados/models.py:1853`). Logo uma viva na própria disputa não demonstra esse evento chegando ao helper: seria BARRADA. O resolver pode, contudo, carregar âncoras do chamado pertencentes a outras disputas (`app/chamados/services/disputa_emissao.py:1938`).

`_C14` não é vigia de órfãs: pergunta pela competência do chamado, em `app/core/juizes.py:122`, registrada em `:286`; o I14 do auditor trata junção de célula (`app/chamados/management/commands/auditar_invariantes_chamados.py:1022`).

_3848 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
