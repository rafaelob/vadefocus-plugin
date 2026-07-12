---
name: pesquisar-jurisprudencia
description: "Pesquisa jurisprudência brasileira real no MCP VadeFocus: busca semântica, híbrida, full-text, regex, CNJ, OJBU, relator, jurimetria agregada, súmulas, temas, OJ e informativos STF/STJ. Use para precedente, acórdão, entendimento de tribunal, acórdãos do relator X, estatística de julgados ou levantamento jurisprudencial. Não use para doutrina nem lei."
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__jurimetria_volume, mcp__plugin_vadefocus-juris_iajus__jurimetria_volume, mcp__iajus__jurimetria_relator, mcp__plugin_vadefocus-juris_iajus__jurimetria_relator, mcp__iajus__jurimetria_classe, mcp__plugin_vadefocus-juris_iajus__jurimetria_classe, mcp__iajus__jurimetria_orgao_julgador, mcp__plugin_vadefocus-juris_iajus__jurimetria_orgao_julgador, mcp__iajus__jurimetria_resultado, mcp__plugin_vadefocus-juris_iajus__jurimetria_resultado, mcp__iajus__jurimetria_lag_publicacao, mcp__plugin_vadefocus-juris_iajus__jurimetria_lag_publicacao, mcp__iajus__buscar_qualificada, mcp__plugin_vadefocus-juris_iajus__buscar_qualificada, mcp__iajus__obter_versoes_qualificada, mcp__plugin_vadefocus-juris_iajus__obter_versoes_qualificada, mcp__iajus__buscar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stf, mcp__iajus__buscar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stj, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa
---

# Pesquisar jurisprudência brasileira

## Escopo

O servidor indexa acórdãos colegiados, súmulas, precedentes qualificados e informativos
dos órgãos do acervo VadeFocus: **STF, STJ, TSE, TRF1-6, os 27 TREs, TJ-RJ e TJ-MG**
(2000-2026 - controle concentrado do STF desde 1988, TRF6 desde 2022; **no TJ-RJ a matéria
CÍVEL tem cobertura mais ampla e a CRIMINAL - Câmaras Criminais e Seção Criminal - segue
em coleta, ainda parcial: rode a busca normalmente em qualquer matéria e só sinalize
possível lacuna se voltar vazia/fraca, oferecendo então STJ/STF. Não afirme cobertura
completa de nenhum órgão sem que o próprio resultado a sustente**), com classificação OJBU
alinhada ao CNJ/TPU. Se o usuário pedir um órgão que não esteja nesta lista, diga com
honestidade que o acervo não cobre esse órgão e ofereça os que cobre - não tente contornar
nem invente resultado.

## Escolha da ferramenta

Decida pela INTENÇÃO da pergunta, nesta ordem de verificação:

1. O usuário deu o NÚMERO de uma súmula/SV/tema/OJ → `buscar_qualificada`. Se ele quiser
   saber se a redação MUDOU (histórico de versões/alterações), use `obter_versoes_qualificada`
   com o `entity_id` que veio do hit da `buscar_qualificada`.
2. O usuário deu um número de processo CNJ → `buscar_por_cnj`.
3. Pergunta conceitual/temática → `buscar_semantica`; se vier fraco, `buscar_hibrida`.
4. Expressão técnica literal → `buscar_fts`; padrão de forma (regex) → `buscar_regex`.
5. "Todos os acórdãos do ramo X" → `buscar_por_ontologia`.
6. "Acórdãos do relator X": para o **agregado** ("quem mais relata", "quantos por relator")
   → `jurimetria_relator`; para **listar** os acórdãos de um relator, use `buscar_hibrida` /
   `buscar_fts` com o filtro `relator_norm` (ver "Filtrar por relator").
7. Pergunta QUANTITATIVA/estatística EXATA ("quantas decisões o TJRJ julgou por ano", "quem
   mais relata no STJ", "quais câmaras/classes dominam", "qual a taxa de provimento", "quanto
   tempo o STJ leva para publicar após julgar") → as tools de **jurimetria agregada**
   (`jurimetria_volume` / `jurimetria_relator` / `jurimetria_classe` /
   `jurimetria_orgao_julgador` / `jurimetria_resultado` / `jurimetria_lag_publicacao`) - dão
   contagens EXATAS do read-model, com envelope de honestidade (ver "Jurimetria agregada").
8. "O que saiu no informativo sobre X" → `buscar_informativos_stf` / `_stj`.

## Ferramentas

Todas são read-only e retornam o envelope `{modalidade, total, resultados: […]}`.
Cada exemplo abaixo foi executado com sucesso contra o serviço em produção.

### buscar_qualificada - súmula/SV/tema/OJ pelo número

Lookup estruturado exato. `numero` é obrigatório; `tipo` (`sumula`, `sv`, `tema`,
`oj`, `pn`) e `orgao` (sigla, ex. `"STF"`) desambiguam. Zeros à esquerda e acentos
são normalizados. Canceladas vêm incluídas e MARCADAS em `status_vigencia` - ao citar
uma, avise o usuário da vigência.

```json
{"numero": "11", "tipo": "sv"}
{"numero": "145", "tipo": "sumula", "orgao": "STF"}
{"numero": "1234", "tipo": "tema"}
```

### obter_versoes_qualificada - histórico de redações de uma qualificada

Reader POR ID: dado o `entity_id` de uma qualificada (obtido de um hit da
`buscar_qualificada`), devolve a **linha do tempo das redações** + os **eventos de
alteração**. Use quando o usuário perguntar se uma súmula/tese "mudou de redação", "foi
alterada", "desde quando vale a redação atual" ou quiser a versão histórica.

Parâmetro: `entity_id` (string, obrigatório - pegue-o no campo do hit da qualificada).
Retorna `{tribunal, tipo, numero, status_vigencia, total_versoes, teve_alteracao_de_redacao,
versoes: [{ordem, enunciado, tese, vigencia_inicio, vigencia_fim, is_vigente, status}],
eventos: [{tipo, data_evento, data_publicacao, decisao_origem, ...}]}`. Quando `total_versoes ≤ 1`
e não há eventos, vem um `aviso` de que só a redação vigente/baseline está modelada - reporte
isso em vez de afirmar que nunca houve alteração.

```json
{"entity_id": "<entity_id de um hit de buscar_qualificada>"}
```

Não enumera órgãos: o chamador já precisa ter o `entity_id` (que só sai de uma
`buscar_qualificada` órgão-escopada), então é seguro citar a redação vigente e as anteriores.

### buscar_semantica - significado, não palavra

Padrão para perguntas conceituais. Parâmetros: `consulta` (texto), `k` (1-100),
`tribunal` (sigla maiúscula, opcional), `ano` (um ano), `space` (`default` |
`premium`).

```json
{"consulta": "boa-fé objetiva", "k": 10}
```

### buscar_hibrida - relevância máxima (fusão de sinais)

Funde semântica + full-text + trigram + CNJ + ontologia via RRF com reranker.
É a modalidade mais pesada - use quando a semântica vier fraca, e prefira `k` baixo.
Parâmetros: `consulta`, `k`, `tribunal`, `ano_min`/`ano_max`, `ramo_l1`.

```json
{"consulta": "responsabilidade civil por inscrição indevida em cadastro de inadimplentes", "tribunal": "STJ", "k": 5}
```

### buscar_fts - texto exato com stemming

Full-text pt_unaccent, insensível a acento. Parâmetros: `consulta`, `k`,
`orgao_code` (slug MINÚSCULO, ex. `"stf"`), `ano_min`/`ano_max`, `phrase`
(exige a ordem das palavras - use só quando a ordem importa; restringe muito).
Quando a consulta cita uma qualificada por número ("súmula 637"), o match exato
da própria súmula vem PREPENDADO e sinalizado em `signals.qualificada_numero`.

```json
{"consulta": "súmula 637", "orgao_code": "stf", "k": 5}
```

### buscar_regex - padrão literal de forma

Regex POSIX; exija ≥3 caracteres literais no padrão (âncora do índice).
Parâmetros: `padrao`, `k`, `orgao_code`.

```json
{"padrao": "usucapi[aã]o extraordin", "orgao_code": "stj", "k": 5}
```

### buscar_por_cnj - número de processo

Prefira o número CNJ COMPLETO (com ou sem máscara) - casamento exato indexado.
Componentes decompostos (`sequencial`, `ano`, `segmento`, `tribunal_cnj`,
`origem`) existem, mas consultas só por componentes amplos podem exceder o tempo.

```json
{"numero": "0081107-51.2015.8.13.0439"}
```

### buscar_por_ontologia - ramo do direito (OJBU)

`l1_code` aceita o código TPU (int) ou o slug do ramo. Principais: `899`/`DIR_CIVIL`,
`287`/`DIR_PENAL`, `14`/`DIR_TRIB`, `864`/`DIR_TRAB`, `9985`/`DIR_ADMIN`,
`195`/`DIR_PREV`, `1156`/`DIR_CONS_CONSUMIDOR`, `11428`/`DIR_EL`, `10110`/`DIR_AMB`,
`8826`/`DIR_PROC_CIVIL` (Processual Civil **e do Trabalho**), `1209`/`DIR_PROC_PENAL`. Sub-área via `l2_code`/`l3_code`
(exigem o `l1_code` do ramo). Temas transversais via `tema_transversal`: `DDG`
(digital/dados), `DER` (econômico), `DFN` (financeiro), `DAG` (agrário), `DID` (idoso).

```json
{"l1_code": "DIR_CIVIL", "k": 5}
```

Sob carga, a combinação de facetas (`orgao_code` + `ano_min`/`ano_max`) pode exceder
o tempo - se vier o erro de tempo, repita primeiro só com `l1_code`, depois refine.

### Jurimetria AGREGADA - números EXATOS do read-model (não do texto)

Para perguntas QUANTITATIVAS exatas (volumes, rankings, taxas de desfecho, lag de
publicação), prefira as tools abaixo a contar hits de busca: elas leem os **rollups
agregados** do banco (`agg_decisions_*`), com contagens exatas e um envelope de honestidade
`jurimetria {snapshot_id, as_of, denominator_definition, value_kind, coverage_pct, truncado}`.
Só órgãos do acervo VadeFocus (STF, STJ, TSE, TRFs, TREs, TJ-RJ, TJ-MG) entram na contagem
(o escopo é aplicado no próprio SQL) - um `orgao` fora do acervo volta vazio, não erro.

| Tool | Assinatura | Para quê |
|---|---|---|
| `jurimetria_volume` | `orgao?` e/ou `ano?` (ou `ano_de`/`ano_ate`); `top` 1-200 (padrão 50). **Pelo menos um recorte é obrigatório.** | Volume por órgão × ano. Ex.: `jurimetria_volume(orgao="tjrj")` → série anual do TJ-RJ; `jurimetria_volume(ano=2024, top=20)` → maiores órgãos do acervo em 2024. |
| `jurimetria_relator` | `orgao` (OBRIGATÓRIO), `relator?` (substring, sem acento), `top` 1-50 (padrão 25) | Quem mais relata num órgão + período de atuação. **LGPD:** figura nominal com n<20 é suprimida ("amostra insuficiente"). |
| `jurimetria_classe` | `orgao` (OBRIGATÓRIO), `ano?` ou `ano_de`/`ano_ate`, `top` 1-100 (padrão 25) | Volume por classe processual CNJ. O bucket `classe_cnj_code=-1` é a massa SEM classe resolvida; o envelope traz `cobertura_classe_pct`. |
| `jurimetria_orgao_julgador` | `orgao` (OBRIGATÓRIO), `filtro?` (substring), `top` 1-100 (padrão 25) | Volume por câmara/turma/seção (unidade organizacional - sem supressão LGPD). Ex.: "quais câmaras do TJ-RJ mais julgam?". |
| `jurimetria_resultado` | `orgao?` e/ou recorte de ano (`ano?` ou `ano_de`/`ano_ate`). **Pelo menos um recorte é obrigatório.** | Taxas de DESFECHO (provimento/improvimento) por órgão × ano. Cada desfecho vem com **denominador duplo** rotulado: `share_over_known` (n/decididas) E `share_over_all` (n/total). **LGPD:** recortes com <20 decisões conhecidas são suprimidos. |
| `jurimetria_lag_publicacao` | `orgao?` OU `ano?`. **Pelo menos um recorte é obrigatório.** | Lag de publicação (dias `data_publicacao − data_julgamento`, p50/p90) por órgão × ano. **NÃO é duração do processo** - só o intervalo publicação−julgamento. Órgãos sem `data_publicacao` (vários TJs) saem sem lag. |

Regras de honestidade (obrigatório):

- **Taxa de desfecho SÓ via `jurimetria_resultado`** - nunca infira provimento/improvimento de
  contagens de volume. Reporte a taxa SEMPRE com o denominador (`share_over_known` vs
  `share_over_all`) e o `coverage_pct`; cobertura baixa NÃO é representativa (o extractor
  abstém em dispositivo ambíguo) - diga "baixa cobertura", não uma taxa nua.
- **`jurimetria_lag_publicacao` NÃO é duração do processo.** Quando o coverage é baixo ou o
  órgão não expõe `data_publicacao`, reporte "sem cobertura" e NÃO estime.
- `aviso: "sem cobertura para o recorte"` = lacuna de ingestão/rollup, **não** volume zero no
  mundo real. Reporte `as_of` (data do snapshot) quando o número embasar uma afirmação.
- Sem recorte (`orgao`/`ano`) a tool recusa com erro pedindo o recorte - não insista; peça-o
  ao usuário. Rótulos de código de classe via `obter_ontologia_juridica`.

```json
{"orgao": "tjrj"}
{"orgao": "stj", "relator": "Nancy"}
{"orgao": "stj", "ano_de": 2020, "ano_ate": 2024}
```

### Filtrar por relator / classe em QUALQUER busca

Para **estreitar uma busca temática**, as modalidades aceitam facetas estruturadas além do texto:
- **`relator_norm`** (relator normalizado, igualdade exata) - em `buscar_hibrida`, `buscar_fts`,
  `buscar_regex` e `buscar_por_ontologia`. Preenchido em ~99,2% do corpus (faceta de 1ª linha).
  A `buscar_semantica` pura **não** recebe relator - para busca densa por significado com filtro
  de relator use `buscar_hibrida` (que funde a perna densa) com `relator_norm`.
- **`classe_cnj_code`** (código CNJ da classe, inteiro) - em `buscar_hibrida`. É **esparso**
  (~6% do corpus tem código CNJ normalizado), então use como faceta **opcional**: ela restringe
  à minoria com classe normalizada. Para o panorama por classe em larga escala prefira
  `jurimetria_classe` (volume exato por classe CNJ de um órgão, com `cobertura_classe_pct`).

Ambas só se aplicam à família jurisprudência.

```json
{"consulta": "dano moral coletivo", "relator_norm": "<forma normalizada>", "k": 10}
{"consulta": "responsabilidade do Estado", "classe_cnj_code": 1116, "k": 10}
```

Se você só tem o nome "humano" do relator (ex.: "Min. Luís Roberto Barroso"), use primeiro
`jurimetria_relator` com `relator` (substring, sem acento) no órgão em questão para confirmar
a forma normalizada; depois reaproveite esse `relator_norm` nas modalidades de busca. O nome
também aparece nos hits de `buscar_semantica`/`buscar_hibrida`.

### buscar_informativos_stf / buscar_informativos_stj - informativos

Entradas por tema dos informativos de jurisprudência (STF Informativos 690-1200+,
STJ 511-888+). Parâmetros: `query`, `limite`.

```json
{"query": "fraude à execução", "limite": 5}
```

### obter_unidade_completa - texto integral de um hit

Quando um hit trouxer `unit_id` (em `family_meta`), use-o para recuperar o texto
completo antes de citar trecho longo. Não chame com `entity_id` (não resolve) -
nesse caso cite pelo `ementa_snippet` + `link_completo`.

```json
{"unit_id": "<family_meta.unit_id de um hit>"}
```

## Pesquisa em profundidade - paralelize com SUBAGENTES

Uma única chamada serve a uma pergunta pontual. Mas quando a tarefa é um **levantamento**
("levante todos os precedentes sobre X", "monte o panorama jurisprudencial de Y", "qual a
tese firmada e os acórdãos divergentes sobre Z", "prepare a base para uma apostila/questões"),
**não faça tudo numa thread só**: **delegue a subagentes de pesquisa rodando em PARALELO** -
é mais rápido e cada subagente mantém o próprio contexto limpo, sem poluir o seu.

**Como decompor (1 subagente por eixo independente):** por **sub-tema** (cada tese/instituto),
por **órgão** (STF vs STJ vs TJ), por **modalidade** (um varre semântica/híbrida, outro a
ontologia do ramo, outro as qualificadas por número), ou por **relator** quando o foco é a
posição de um julgador. Lance os subagentes **numa só leva** (em paralelo) e só então
consolide. Dê a cada um: o recorte exato, as tools que pode usar (as deste plugin) e o
formato de retorno (**dossiê citável**: órgão, nº processo/acórdão, relator, data, ementa em
trecho e `link_completo` - **nada de memória**).

- **Claude / Claude Code:** abra subagentes via a ferramenta de tarefas (Task/Agent) do
  seu cliente, lançando-os **na mesma mensagem** para correrem em paralelo.
- **ChatGPT / Codex e afins:** dispare as buscas independentes **em lote** (várias tool-calls
  na mesma rodada) ou, se o cliente suportar, sub-tarefas paralelas - o efeito é o mesmo:
  cobrir vários ângulos de uma vez.

**Regras do levantamento:** (1) comece denso (`buscar_semantica`/`buscar_hibrida`) e cruze
com `buscar_fts`/`buscar_regex`/`buscar_por_ontologia`; (2) **refine** reusando os termos
que apareceram nos primeiros hits (relator, tese, dispositivo) até a cobertura estabilizar
(rodadas sem resultado novo); (3) para a tese vinculante prefira `buscar_qualificada`;
(4) ao consolidar, **deduplique** por `numero_processo`/`link_completo` e agrupe por tese;
(5) feche com as **lacunas** (o que NÃO foi achado) para o solicitante saber o limite da
cobertura. Esta skill alimenta **montar-apostila-didatica** e **gerar-questoes-concurso** -
quando o objetivo final for material de estudo ou questões, já entregue o dossiê nesse formato.

## Tratamento de erros

- **Mensagens de argumento são acionáveis**: "argumento desconhecido: 'termo' -
  confira os argumentos aceitos" significa nome errado de parâmetro (o texto chama-se
  `consulta`; em regex, `padrao`). Corrija e repita - não é indisponibilidade.
- **`{"erro": "a consulta excedeu o tempo limite…"}`**: restrinja o escopo (órgão,
  ano, `k` menor) ou simplifique a consulta e repita uma vez.
- **`aviso_busca` presente**: a perna principal da busca falhou, mas o lookup exato
  de qualificada respondeu - os `resultados` exibidos são confiáveis para citar.
- **401 na primeira chamada**: repita UMA vez (validação de chave no cold-start).
  Se persistir, oriente o usuário a revisar a chave configurada - nunca exiba a chave.
- **`total: 0`**: diga honestamente que não encontrou; ofereça reformular ou ampliar.
  Nunca preencha a lacuna com precedente inventado.

## Regras de citação (obrigatório)

- Cite SEMPRE o `link_completo` do registro (URL estável da fonte oficial). Nunca
  invente número de processo, ementa ou link.
- Inclua `tribunal`, `numero_processo` (ou `numero_processo_cnj`), `relator` e
  `data_julgamento` quando presentes; resuma a `ementa_snippet` em 1-2 frases.
- Para "entendimento atual", prefira qualificadas (súmula/SV/tema) via
  `buscar_qualificada` a um acórdão isolado.
- Preserve diacríticos e UTF-8 exatamente como na fonte.
