---
name: pesquisar-jurisprudencia
description: Pesquisa e cita jurisprudência brasileira real (STF, STJ, TSE, TREs, TRFs, TJ-RJ, TJ-MG) pelo MCP VadeFocus, com as 7 modalidades de busca (semântica, híbrida, FTS, regex, CNJ, ontologia OJBU, grafo de citações). Acione sempre que o usuário pedir precedente, acórdão, súmula, repercussão geral, número de processo CNJ ou o grafo de citações de uma súmula/tema — ou perguntar "o que os tribunais decidiram sobre X", "tem precedente sobre Y", "qual o entendimento atual" — em vez de responder de memória. NÃO use para doutrina de autor (skill consultar-doutrina) nem para o texto de leis (skill consultar-legislacao).
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_grafo, mcp__plugin_vadefocus-juris_iajus__buscar_grafo, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa
---

# Pesquisar jurisprudência brasileira (VadeFocus)

Você tem acesso ao servidor MCP VadeFocus, que indexa jurisprudência brasileira dos
órgãos do escopo VadeFocus — **STF, STJ, TSE, os 6 TRFs, os 27 TREs, TJ-RJ e TJ-MG**
(acórdãos colegiados 2013-2026 — TREs 2016-2026 —, súmulas, precedentes qualificados,
repercussão geral) com classificação alinhada ao CNJ/TPU (ontologia OJBU: 21 ramos L1
→ sub-áreas L2/L3). Consultas a outros órgãos (TST, TRTs, TCU, demais TJs, STM…) ficam
fora do escopo e retornam vazio. **Use o MCP em vez de inventar precedentes.**

## Escolha da modalidade (7 tools de busca)

Comece pela modalidade certa para a pergunta. Todas retornam o mesmo envelope
uniforme (`{ modalidade, total, resultados:[…] }`) e são read-only.

| Pergunta do usuário | Tool | Por quê |
|---|---|---|
| Tema/conceito ("o que decidiram sobre dano moral por inscrição indevida") | `buscar_semantica` | Vetorial/densa — casa por significado, não por palavra. **Padrão para perguntas conceituais.** |
| Quer o melhor resultado geral (relevância máxima) | `buscar_hibrida` | Funde semântica + FTS + trigram + CNJ + ontologia via RRF, com boost de classificação. |
| Expressão exata / termo técnico literal ("usucapião extraordinária") | `buscar_fts` | Full-text pt_unaccent, stemming PT, insensível a acento; `phrase=true` exige a ordem. |
| Padrão literal / forma de citação ("Súmula 7", "art. 1.228") | `buscar_regex` | Regex POSIX; **exija ≥3 caracteres literais** no padrão (âncora do índice). |
| Número de processo CNJ | `buscar_por_cnj` | `numero` completo = casamento exato; ou componentes (ano, tribunal, origem). |
| "Todos os acórdãos de um ramo do direito" | `buscar_por_ontologia` | Subárvore ltree por `l1_code` TPU (ou L2/L3, ou `tema_transversal`). Ex.: `l1_code=287` (Penal), `899` (Civil), `9985` (Administrativo). |
| Quem citou uma súmula/tema, ou o que um acórdão cita | `buscar_grafo` | `legal_edges` single-hop; `normalized_ref="Súmula 279"` traz quem aplicou. |

### Chamadas que funcionam (testadas ao vivo contra o serviço)

```json
buscar_semantica  {"consulta": "boa-fé objetiva", "k": 10}
buscar_hibrida    {"consulta": "responsabilidade civil por inscrição indevida em cadastro de inadimplentes",
                   "tribunal": "STJ", "ano_min": 2026, "ano_max": 2026, "k": 10}
buscar_fts        {"consulta": "súmula 637", "orgao_code": "stf", "k": 5}
buscar_regex      {"padrao": "usucapi[aã]o extraordin", "orgao_code": "stj", "k": 5}
buscar_por_cnj    {"numero": "0081107-51.2015.8.13.0439"}
buscar_por_ontologia {"l1_code": 287, "orgao_code": "stj", "ano_min": 2024, "ano_max": 2026, "k": 5}
```

Notas de uso:
- **O parâmetro de texto chama-se `consulta`** em `buscar_semantica`/`buscar_fts`/
  `buscar_hibrida` (e `padrao` em `buscar_regex`). **Nunca** `termo`, `query` ou
  `texto`: nome errado de argumento volta hoje como o erro genérico *"erro interno ao
  processar a consulta"* — quando vir essa mensagem numa busca, **confira primeiro o
  nome dos argumentos** antes de supor indisponibilidade.
- **Filtro de órgão difere por modalidade:** só `buscar_semantica` / `buscar_hibrida`
  aceitam `tribunal` (ex.: `"STF"`). As demais (`buscar_regex`, `buscar_fts`,
  `buscar_por_ontologia`) filtram por **`orgao_code`** — o slug minúsculo do órgão
  (ex.: `"stf"`, `"stj"`). **Não** passe `tribunal` para essas: a tool rejeita o
  argumento. (`buscar_por_cnj` recebe `tribunal_cnj` como componente CNJ.)
- `buscar_semantica` / `buscar_hibrida` aceitam `tribunal`, `space` (`default` =
  text-embedding-3-small; `premium` = gemini-2) e `k` (1-100, padrão 20).
  `buscar_hibrida` aceita `ano_min`/`ano_max` e `ramo_l1` (sinal de ontologia);
  `buscar_semantica` aceita `ano` (um ano único).
- `buscar_semantica` para a intenção temática; **se vier fraco, escale para
  `buscar_hibrida`** (mesma consulta) antes de desistir.
- `buscar_regex` recusa padrões só de metacaracteres (ex.: `^[A-Z]+$`); inclua um
  trecho literal ≥3 chars. Em erro, a tool devolve `{ "erro": "…", "resultados": [] }`
  (nunca stack trace) — leia a mensagem e ajuste.
- **Consultas amplas estouram o tempo** e voltam `{"erro": "a consulta excedeu o tempo
  limite — restrinja o escopo…"}`. Mitigações comprovadas: em `buscar_por_ontologia`
  **sempre** passe `orgao_code` + `ano_min`/`ano_max`; em `buscar_por_cnj` prefira o
  **número CNJ completo** (componentes soltos como só ano+segmento estouram o tempo);
  em `buscar_regex` ancore o padrão num literal longo e filtre por `orgao_code`.
- `buscar_grafo` (`normalized_ref="Súmula 279"`, `direction="in"`) pode retornar
  `total: 0` neste perfil mesmo para súmulas muito citadas (lane do grafo em
  consolidação). **Não conclua "ninguém citou"** — diga que o grafo não retornou
  arestas e ofereça `buscar_fts` com a forma literal (ex.: `"súmula 279"`) como
  alternativa, que funciona.
- **Ementa completa:** os hits trazem `ementa_snippet` (recorte). Quando o registro
  expuser `unit_id`, use `obter_unidade_completa(unit_id=…)` para o texto integral da
  unidade antes de citar trecho longo. Se o hit só trouxer `entity_id` (caso comum
  hoje), **não** chame a tool com ele (não resolve) — cite pelo snippet + `link_completo`.

### Paginação por cursor (keyset — disponível nas modalidades fts/regex/cnj/grafo)

As modalidades `buscar_fts`, `buscar_regex`, `buscar_por_cnj` e `buscar_grafo` aceitam
`sort`, `order`, `page_size` e `cursor` (keyset): a página vem com `page_info`
(`has_more`, `next_cursor`, `truncated`). Para a página seguinte, repita a MESMA
consulta com `cursor=<next_cursor>` — o cursor é opaco e assinado; não o edite.
**Compatibilidade:** se o servidor responder erro à chamada com `cursor` (versão
anterior do serviço ainda sem keyset), repita a chamada **sem** os parâmetros de
paginação e trabalhe com a primeira página.

## Ontologia OJBU (ramos reais — use estes códigos)

`buscar_por_ontologia` recebe `l1_code` (código TPU do ramo, inteiro), opcionalmente
`l2_code`/`l3_code` (a sub-área exige o `l1_code` do seu ramo) **ou** `tema_transversal`.
Os 21 ramos L1 (código TPU → ramo) são os nós reais da ontologia:

| `l1_code` | Ramo | | `l1_code` | Ramo |
|---|---|---|---|---|
| 14 | Tributário | | 9633 | Criança e Adolescente |
| 195 | Previdenciário | | 9985 | Administrativo e Dir. Público |
| 287 | Penal | | 10110 | Ambiental |
| 864 | Trabalho | | 11068 | Penal Militar |
| 899 | Civil | | 11428 | Eleitoral |
| 1156 | Consumidor | | 12480 | Saúde |
| 1209 | Processual Penal | | 12734 | Assistencial |
| 6191 | Internacional | | 12775 | Educação |
| 7724 | Registros Públicos | | 1146 | Marítimo |
| 8826 | Processual Civil e do Trabalho | | 11049 | Processual Penal Militar |

Temas transversais (`tema_transversal=`): `DDG` (Digital e Proteção de Dados),
`DER` (Econômico e Regulação), `DFN` (Financeiro e Orçamentário), `DAG` (Agrário e
Fundiário Rural), `DID` (Pessoa Idosa e Envelhecimento). Use o tema transversal para
recortes que cruzam ramos (ex.: LGPD → `DDG`), e o `l1_code` para o ramo em si.

## Como citar (obrigatório)

- **Sempre** cite o campo `link_completo` do registro retornado — é a URL estável,
  deep-per-record, do acórdão na fonte oficial. **Nunca invente** número de processo,
  ementa ou link.
- Cite `tribunal`, `numero_processo` (ou `numero_processo_cnj`), `relator` e
  `data_julgamento` quando presentes. Resuma a `ementa_snippet` em 1-2 frases.
- Quando útil, mostre a classificação OJBU do registro (`classificacao.l1`,
  `ramo_l1_codes`) e ofereça abrir o inteiro teor pelo `inteiro_teor_url`.
- Se a busca **não** retornar resultado relevante (`total: 0` ou hits fracos),
  **diga isso honestamente** — não preencha a lacuna com um precedente fabricado.

## Boas práticas

- Comece restrito (tema + tribunal + faixa de ano) e amplie só se vier vazio.
- Para "qual o entendimento atual", prefira precedentes qualificados (súmula /
  repercussão geral) a um acórdão isolado: use `buscar_por_ontologia`/`buscar_grafo`
  para chegar aos precedentes qualificados do tema.
- Preserve diacríticos e UTF-8 exatamente como na fonte.
- A chave `ik_*` é injetada pelo cliente MCP no header `Authorization: Bearer`
  (Claude Code: `userConfig`/keychain; Cowork: `managedMcpServers`; Codex:
  `bearer_token_env_var`). **Se uma tool responder 401, repita UMA vez** (o primeiro
  acesso após cold-start pode falhar transitoriamente enquanto o servidor valida a
  chave); só se o 401 persistir avise o usuário para revisar a chave configurada —
  nunca cole a chave em chat nem em commit.
- **Respostas grandes:** o plugin aceita o `userConfig` opcional `max_output_kb`
  (header `X-Iajus-Max-Output-Kb`) que fixa o orçamento de resposta por chamada no
  servidor (16-8192 KB; acima disso a resposta vem truncada com aviso explícito
  `[saida truncada…]`). O corte mais comum, porém, é do PRÓPRIO cliente — no Claude
  Code, respostas acima de `MAX_MCP_OUTPUT_TOKENS` são cortadas localmente; para
  ementas/textos longos prefira `k` menor + a unidade específica em vez de listas
  enormes.
