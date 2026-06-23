---
name: pesquisar-jurisprudencia
description: Pesquisa jurisprudência brasileira real no MCP VadeFocus (STF, STJ, TSE, TREs, TRFs, TJ-RJ, TJ-MG) — busca semântica/híbrida/full-text/regex, número de processo CNJ, ramo OJBU, filtro e agrupamento por RELATOR (jurimetria), consulta estruturada de súmula/súmula vinculante/tema/OJ por número e informativos STF/STJ. Use sempre que o usuário pedir precedente, acórdão, súmula, tema de repercussão geral, entendimento de tribunal, "acórdãos do relator X", panorama/levantamento jurisprudencial ou informativo — nunca responda jurisprudência de memória. Para um levantamento que exija várias buscas, delegue a subagentes de pesquisa em paralelo (ver seção "Pesquisa em profundidade"). Não use para doutrina de autor (consultar-doutrina) nem texto de lei (consultar-legislacao).
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_jurimetria, mcp__plugin_vadefocus-juris_iajus__buscar_jurimetria, mcp__iajus__consultar_qualificada, mcp__plugin_vadefocus-juris_iajus__consultar_qualificada, mcp__iajus__consultar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__consultar_informativos_stf, mcp__iajus__consultar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__consultar_informativos_stj, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa
---

# Pesquisar jurisprudência brasileira

## Escopo

O servidor indexa acórdãos colegiados, súmulas, precedentes qualificados e informativos
dos órgãos do acervo VadeFocus: **STF, STJ, TSE, TRF1-6, os 27 TREs, TJ-RJ e TJ-MG**
(2013-2026; TREs 2016-2026), com classificação OJBU alinhada ao CNJ/TPU. Se o usuário
pedir um órgão que não esteja nesta lista, diga com honestidade que o acervo não cobre
esse órgão e ofereça os que cobre — não tente contornar nem invente resultado.

## Escolha da ferramenta

Decida pela INTENÇÃO da pergunta, nesta ordem de verificação:

1. O usuário deu o NÚMERO de uma súmula/SV/tema/OJ → `consultar_qualificada`.
2. O usuário deu um número de processo CNJ → `buscar_por_cnj`.
3. Pergunta conceitual/temática → `buscar_semantica`; se vier fraco, `buscar_hibrida`.
4. Expressão técnica literal → `buscar_fts`; padrão de forma (regex) → `buscar_regex`.
5. "Todos os acórdãos do ramo X" → `buscar_por_ontologia`.
6. "Acórdãos do relator X", "quantos acórdãos por relator", "liste/agrupe por relator,
   tribunal, ano, classe ou resultado" → `buscar_jurimetria` (lista ou agrega por colunas
   estruturadas). Para **filtrar uma busca temática por relator**, passe `relator_norm`
   nas modalidades de busca (ver "Filtrar por relator").
7. "O que saiu no informativo sobre X" → `consultar_informativos_stf` / `_stj`.

## Ferramentas

Todas são read-only e retornam o envelope `{modalidade, total, resultados: […]}`.
Cada exemplo abaixo foi executado com sucesso contra o serviço em produção.

### consultar_qualificada — súmula/SV/tema/OJ pelo número

Lookup estruturado exato. `numero` é obrigatório; `tipo` (`sumula`, `sv`, `tema`,
`oj`, `pn`) e `orgao` (sigla, ex. `"STF"`) desambiguam. Zeros à esquerda e acentos
são normalizados. Canceladas vêm incluídas e MARCADAS em `status_vigencia` — ao citar
uma, avise o usuário da vigência.

```json
{"numero": "11", "tipo": "sv"}
{"numero": "145", "tipo": "sumula", "orgao": "STF"}
{"numero": "1234", "tipo": "tema"}
```

### buscar_semantica — significado, não palavra

Padrão para perguntas conceituais. Parâmetros: `consulta` (texto), `k` (1-100),
`tribunal` (sigla maiúscula, opcional), `ano` (um ano), `space` (`default` |
`premium`).

```json
{"consulta": "boa-fé objetiva", "k": 10}
```

### buscar_hibrida — relevância máxima (fusão de sinais)

Funde semântica + full-text + trigram + CNJ + ontologia via RRF com reranker.
É a modalidade mais pesada — use quando a semântica vier fraca, e prefira `k` baixo.
Parâmetros: `consulta`, `k`, `tribunal`, `ano_min`/`ano_max`, `ramo_l1`.

```json
{"consulta": "responsabilidade civil por inscrição indevida em cadastro de inadimplentes", "tribunal": "STJ", "k": 5}
```

### buscar_fts — texto exato com stemming

Full-text pt_unaccent, insensível a acento. Parâmetros: `consulta`, `k`,
`orgao_code` (slug MINÚSCULO, ex. `"stf"`), `ano_min`/`ano_max`, `phrase`
(exige a ordem das palavras — use só quando a ordem importa; restringe muito).
Quando a consulta cita uma qualificada por número ("súmula 637"), o match exato
da própria súmula vem PREPENDADO e sinalizado em `signals.qualificada_numero`.

```json
{"consulta": "súmula 637", "orgao_code": "stf", "k": 5}
```

### buscar_regex — padrão literal de forma

Regex POSIX; exija ≥3 caracteres literais no padrão (âncora do índice).
Parâmetros: `padrao`, `k`, `orgao_code`.

```json
{"padrao": "usucapi[aã]o extraordin", "orgao_code": "stj", "k": 5}
```

### buscar_por_cnj — número de processo

Prefira o número CNJ COMPLETO (com ou sem máscara) — casamento exato indexado.
Componentes decompostos (`sequencial`, `ano`, `segmento`, `tribunal_cnj`,
`origem`) existem, mas consultas só por componentes amplos podem exceder o tempo.

```json
{"numero": "0081107-51.2015.8.13.0439"}
```

### buscar_por_ontologia — ramo do direito (OJBU)

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
o tempo — se vier o erro de tempo, repita primeiro só com `l1_code`, depois refine.

### buscar_jurimetria — listar/agrupar acórdãos por relator (e outras colunas)

Browse por COLUNAS ESTRUTURADAS dos acórdãos, em dois modos:

- **Lista** (sem `group_by`): devolve os acórdãos que casam os `filters`, mais recentes
  primeiro — um *browse* tipado. Use para "todos os acórdãos do relator X no STJ".
- **Agregado** (`group_by`): `count(*)` por bucket. Use para "quantos acórdãos por relator",
  "distribuição por tribunal/ano/resultado". Dimensões: `relator`, `tribunal`, `orgao_code`,
  `ano`, `orgao_julgador`, `classe`, `uf`, `resultado`, `votacao`, `tema_tipo`.

Filtros (no objeto `filters`): `relator_norm` (relator NORMALIZADO — igualdade exata),
`relator_nome` (casa por substring no nome completo), `tribunal`, `orgao_code`,
`classe_cnj_code`, `uf_origem`, `ano_min`/`ano_max`, `resultado_code`, `is_repercussao_geral`,
`numero_processo`/`numero_acordao` (casam por dígitos). Chaves desconhecidas são ignoradas.

```json
{"filters": {"relator_nome": "Barroso", "tribunal": "STF"}, "k": 20}
{"filters": {"tribunal": "STJ", "ano_min": 2022}, "group_by": "relator"}
```

> Só órgãos do acervo VadeFocus aparecem (o filtro de órgão é aplicado no próprio SQL);
> um `relator` é "normalizado" — se não souber a forma exata, comece por `relator_nome`
> (substring) ou descubra o nome num hit de `buscar_semantica`/`buscar_hibrida` e refine.

### Filtrar por relator em QUALQUER busca (`relator_norm`)

Para **estreitar uma busca temática a um relator**, passe `relator_norm` (relator
normalizado, igualdade exata) — disponível em `buscar_hibrida`, `buscar_fts`, `buscar_regex`
e `buscar_por_ontologia`. Para busca **densa por significado** com filtro de relator, use
`buscar_hibrida` (que funde a perna densa) com `relator_norm` — a `buscar_semantica` pura
não recebe esse filtro. O campo está preenchido em ~99,2% do corpus e só se aplica à
família jurisprudência.

```json
{"consulta": "dano moral coletivo", "relator_norm": "<forma normalizada>", "k": 10}
```

Se você só tem o nome "humano" do relator (ex.: "Min. Luís Roberto Barroso"), use primeiro
`buscar_jurimetria` com `relator_nome` (substring) para confirmar a forma normalizada; depois
reaproveite esse `relator_norm` nas demais modalidades.

### consultar_informativos_stf / consultar_informativos_stj — informativos

Entradas por tema dos informativos de jurisprudência (STF Informativos 690-1200+,
STJ 511-888+; cobertura 2013-2026). Parâmetros: `query`, `limite`.

```json
{"query": "fraude à execução", "limite": 5}
```

### obter_unidade_completa — texto integral de um hit

Quando um hit trouxer `unit_id` (em `family_meta`), use-o para recuperar o texto
completo antes de citar trecho longo. Não chame com `entity_id` (não resolve) —
nesse caso cite pelo `ementa_snippet` + `link_completo`.

```json
{"unit_id": "<family_meta.unit_id de um hit>"}
```

## Pesquisa em profundidade — paralelize com SUBAGENTES

Uma única chamada serve a uma pergunta pontual. Mas quando a tarefa é um **levantamento**
("levante todos os precedentes sobre X", "monte o panorama jurisprudencial de Y", "qual a
tese firmada e os acórdãos divergentes sobre Z", "prepare a base para uma apostila/questões"),
**não faça tudo numa thread só**: **delegue a subagentes de pesquisa rodando em PARALELO** —
é mais rápido e cada subagente mantém o próprio contexto limpo, sem poluir o seu.

**Como decompor (1 subagente por eixo independente):** por **sub-tema** (cada tese/instituto),
por **órgão** (STF vs STJ vs TJ), por **modalidade** (um varre semântica/híbrida, outro a
ontologia do ramo, outro as qualificadas por número), ou por **relator** quando o foco é a
posição de um julgador. Lance os subagentes **numa só leva** (em paralelo) e só então
consolide. Dê a cada um: o recorte exato, as tools que pode usar (as deste plugin) e o
formato de retorno (**dossiê citável**: órgão, nº processo/acórdão, relator, data, ementa em
trecho e `link_completo` — **nada de memória**).

- **Claude / Claude Code:** abra subagentes via a ferramenta de tarefas (Task/Agent) do
  seu cliente, lançando-os **na mesma mensagem** para correrem em paralelo.
- **ChatGPT / Codex e afins:** dispare as buscas independentes **em lote** (várias tool-calls
  na mesma rodada) ou, se o cliente suportar, sub-tarefas paralelas — o efeito é o mesmo:
  cobrir vários ângulos de uma vez.

**Regras do levantamento:** (1) comece denso (`buscar_semantica`/`buscar_hibrida`) e cruze
com `buscar_fts`/`buscar_regex`/`buscar_por_ontologia`; (2) **refine** reusando os termos
que apareceram nos primeiros hits (relator, tese, dispositivo) até a cobertura estabilizar
(rodadas sem resultado novo); (3) para a tese vinculante prefira `consultar_qualificada`;
(4) ao consolidar, **deduplique** por `numero_processo`/`link_completo` e agrupe por tese;
(5) feche com as **lacunas** (o que NÃO foi achado) para o solicitante saber o limite da
cobertura. Esta skill alimenta **montar-apostila-didatica** e **gerar-questoes-concurso** —
quando o objetivo final for material de estudo ou questões, já entregue o dossiê nesse formato.

## Tratamento de erros

- **Mensagens de argumento são acionáveis**: "argumento desconhecido: 'termo' —
  confira os argumentos aceitos" significa nome errado de parâmetro (o texto chama-se
  `consulta`; em regex, `padrao`). Corrija e repita — não é indisponibilidade.
- **`{"erro": "a consulta excedeu o tempo limite…"}`**: restrinja o escopo (órgão,
  ano, `k` menor) ou simplifique a consulta e repita uma vez.
- **`aviso_busca` presente**: a perna principal da busca falhou, mas o lookup exato
  de qualificada respondeu — os `resultados` exibidos são confiáveis para citar.
- **401 na primeira chamada**: repita UMA vez (validação de chave no cold-start).
  Se persistir, oriente o usuário a revisar a chave configurada — nunca exiba a chave.
- **`total: 0`**: diga honestamente que não encontrou; ofereça reformular ou ampliar.
  Nunca preencha a lacuna com precedente inventado.

## Regras de citação (obrigatório)

- Cite SEMPRE o `link_completo` do registro (URL estável da fonte oficial). Nunca
  invente número de processo, ementa ou link.
- Inclua `tribunal`, `numero_processo` (ou `numero_processo_cnj`), `relator` e
  `data_julgamento` quando presentes; resuma a `ementa_snippet` em 1-2 frases.
- Para "entendimento atual", prefira qualificadas (súmula/SV/tema) via
  `consultar_qualificada` a um acórdão isolado.
- Preserve diacríticos e UTF-8 exatamente como na fonte.
