---
name: pesquisar-jurisprudencia
description: Pesquisa jurisprudência brasileira real no MCP VadeFocus (STF, STJ, TSE, TREs, TRFs, TJ-RJ, TJ-MG) — busca semântica/híbrida/full-text/regex, número de processo CNJ, ramo OJBU, consulta estruturada de súmula/súmula vinculante/tema/OJ por número e informativos STF/STJ. Use sempre que o usuário pedir precedente, acórdão, súmula, tema de repercussão geral, entendimento de tribunal ou informativo — nunca responda jurisprudência de memória. Não use para doutrina de autor (consultar-doutrina) nem texto de lei (consultar-legislacao).
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__consultar_qualificada, mcp__plugin_vadefocus-juris_iajus__consultar_qualificada, mcp__iajus__consultar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__consultar_informativos_stf, mcp__iajus__consultar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__consultar_informativos_stj, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa
---

# Pesquisar jurisprudência brasileira

## Escopo

O servidor indexa acórdãos colegiados, súmulas, precedentes qualificados e informativos
dos órgãos do perfil VadeFocus: **STF, STJ, TSE, TRF1-6, os 27 TREs, TJ-RJ e TJ-MG**
(2013-2026; TREs 2016-2026), com classificação OJBU alinhada ao CNJ/TPU. Outros órgãos
(TST, TRTs, TCU, demais TJs, STM) retornam vazio por estarem fora do perfil — informe
isso ao usuário em vez de tentar contornar.

## Escolha da ferramenta

Decida pela INTENÇÃO da pergunta, nesta ordem de verificação:

1. O usuário deu o NÚMERO de uma súmula/SV/tema/OJ → `consultar_qualificada`.
2. O usuário deu um número de processo CNJ → `buscar_por_cnj`.
3. Pergunta conceitual/temática → `buscar_semantica`; se vier fraco, `buscar_hibrida`.
4. Expressão técnica literal → `buscar_fts`; padrão de forma (regex) → `buscar_regex`.
5. "Todos os acórdãos do ramo X" → `buscar_por_ontologia`.
6. "O que saiu no informativo sobre X" → `consultar_informativos_stf` / `_stj`.

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
`8826`/`DIR_PROC_CIVIL`, `1209`/`DIR_PROC_PENAL`. Sub-área via `l2_code`/`l3_code`
(exigem o `l1_code` do ramo). Temas transversais via `tema_transversal`: `DDG`
(digital/dados), `DER` (econômico), `DFN` (financeiro), `DAG` (agrário), `DID` (idoso).

```json
{"l1_code": "DIR_CIVIL", "k": 5}
```

Sob carga, a combinação de facetas (`orgao_code` + `ano_min`/`ano_max`) pode exceder
o tempo — se vier o erro de tempo, repita primeiro só com `l1_code`, depois refine.

### consultar_informativos_stf / consultar_informativos_stj — informativos

Entradas por tema dos informativos de jurisprudência (STF edições 1-1218,
STJ 511-889). Parâmetros: `query`, `limite`.

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
