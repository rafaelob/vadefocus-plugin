---
name: consultar-legislacao
description: Consulta legislação federal brasileira real (leis, decretos, MPVs, LCs, emendas) e localiza dispositivos pelo MCP VadeFocus, com texto íntegra do Planalto. Acione quando o usuário pedir o texto de uma lei, um artigo, a redação vigente de um dispositivo, normas por tema/ramo, ou perguntar "qual lei regula Y", "o que diz o art. X da lei Z", "esse dispositivo está em vigor". Localiza por nome, conceito, expressão literal ou ramo OJBU. NÃO use para acórdãos nem doutrina.
allowed-tools: mcp__iajus__obter_codigo, mcp__plugin_vadefocus-juris_iajus__obter_codigo, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__listar_normas, mcp__plugin_vadefocus-juris_iajus__listar_normas, mcp__iajus__buscar_norma_por_nome, mcp__plugin_vadefocus-juris_iajus__buscar_norma_por_nome, mcp__iajus__buscar_norma_por_numero, mcp__plugin_vadefocus-juris_iajus__buscar_norma_por_numero, mcp__iajus__obter_grafo_norma, mcp__plugin_vadefocus-juris_iajus__obter_grafo_norma, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida
---

# Consultar legislação federal brasileira (VadeFocus)

Você tem acesso ao servidor MCP VadeFocus para legislação federal brasileira (leis,
decretos, MPVs, leis complementares, emendas), com DOIS caminhos complementares:
as **tools dedicadas de legislação federal** (norma por identidade tipo+número+ano,
texto do Planalto, códigos premium com hierarquia completa) e as **modalidades de
busca** (com `family="legislacao"`). Use o MCP em vez de citar de memória: **o texto
da fonte é a verdade.**

## "O que diz o art. X?" — playbook por dispositivo (testado ao vivo)

Para um artigo específico de um código/lei, na ordem:

1. **`obter_codigo`** — acervo PREMIUM curado, hierarquia completa
   (Livro→Título→Capítulo→Seção→Artigo→§→inciso→alínea), por alias amigável:
   ```json
   obter_codigo {"codigo": "CP",  "artigo": 121}     ← funciona (subtree completa)
   obter_codigo {"codigo": "CPP", "artigo": 492}     ← funciona
   obter_codigo {"codigo": "CC",  "termo": "união estável", "limit": 5}
   ```
2. **`obter_texto_norma`** — texto consolidado do Planalto, artigo isolado:
   ```json
   obter_texto_norma {"tipo": "DEL", "numero": "2848", "ano": 1940, "artigo": 61}
   ```
   (retorna a redação CONSOLIDADA, com os marcadores "Redação dada pela Lei…";
   `artigo_fim` para faixa inclusiva.)
3. Se (1) e (2) falharem, **`obter_texto_norma`** (norma inteira em
   markdown hierárquico — ex.: LGPD ≈ 63 KB) e localize o artigo no texto.

Identidades canônicas: CP = `DEL 2848/1940`, CPP = `DEL 3689/1941`, CC = `LEI
10406/2002`, CPC = `LEI 13105/2015`, CDC = `LEI 8078/1990`, CLT = `DEL 5452/1943`,
LGPD = `LEI 13709/2018`. Tipos aceitos: `LEI`, `DEL` (decreto-lei), `DEC`, `MPV`,
`LCP` (lei complementar), `EMC` (emenda constitucional).

Armadilhas REAIS (vividas em teste ao vivo — seja honesto quando ocorrerem):
- `obter_texto_norma` pode responder `{"erro": "artigo nao encontrado no texto"}`
  para artigos que existem (ex. vivido: CPP art. 492) — a extração do artigo no texto
  do Planalto falha em alguns diplomas. **Caia para `obter_codigo`** (que resolveu
  o CPP 492) ou para o markdown integral.
- **Artigos com milhar** (ex.: CC arts. 1.000, 1.723, 2.044) funcionam por número —
  `obter_codigo {"codigo": "CC", "artigo": "1.723"}` e `{"artigo": 1723}` retornam
  o dispositivo. Se ainda assim vier `total: 0`, use `obter_codigo` com **`termo=`**
  (FTS dentro do código, ex. `termo="união estável"`) e diga qual caminho usou. Nunca
  apresente `total: 0` como "o artigo não existe".

## Norma inteira, metadados e vigência

| Necessidade | Tool | Como |
|---|---|---|
| Metadados + resumo de alterações ("a LGPD foi alterada?") | `buscar_norma_fonte_oficial` | `{"tipo": "LEI", "numero": "13709", "ano": 2018}` → ementa, apelidos, publicações DOU, `total_alteracoes` + resumo. |
| Texto integral em markdown hierárquico | `obter_texto_norma` | `{"tipo": "LEI", "numero": "13709", "ano": 2018}`. Normas longas: ver nota de tamanho abaixo. |
| Achar a norma pelo tema da ementa | `buscar_norma_fonte_oficial` | `{"consulta": "proteção de dados pessoais", "limite": 5}` (param canônico `consulta`, uniforme com as demais buscas; `termo` ainda aceito como alias). |
| Localizar a norma pelo NOME/apelido, com status | `buscar_norma_por_nome` | `{"nome": "Lei Maria da Penha"}` → norma(s) que casam o nome/título, cada uma COM status de vigência. |
| Localizar a norma pelo NÚMERO+tipo+ano, com status | `buscar_norma_por_numero` | `{"tipo": "LEI", "numero": "11340", "ano": 2006}` → a norma exata, COM status de vigência. |
| "Quais leis saíram em 2024?" | `listar_normas` | `{"tipo": "LEI", "ano": 2024, "limite": 20}`. |
| Cadeia de alterações / conversão MPV→LEI | `obter_grafo_norma` | `{"norma_ref": "LEI_8078_1990", "max_depth": 1}`. **`norma_ref` usa o ID canônico underscore `TIPO_NUMERO_ANO`** (ex.: `LEI_8078_1990`, `DEL_2848_1940`), o mesmo `norma_id` emitido pelos demais tools — **NÃO** use "LEI 8078/1990" (casamento exato, sem normalização → volta vazio). Pode vir vazia para normas ainda não projetadas no grafo — diga isso, não conclua "nunca alterada". |

## Como consultar (modalidades, com `family="legislacao"`)

| Necessidade | Tool | Como |
|---|---|---|
| Localizar a norma/dispositivo por nome ou termo ("CDC", "Lei Geral de Proteção de Dados", "boa-fé objetiva") | `buscar_fts` | `consulta=<termo>`, `family="legislacao"`. Stemming PT, insensível a acento. `phrase=true` para expressão exata. |
| Buscar por conceito/intenção quando o usuário não sabe a redação | `buscar_semantica` | `consulta=<descrição>`, `space=…`. Vetorial. |
| Citação ou redação literal ("art. 5º", "§ 2º do art. 18") | `buscar_regex` | Padrão POSIX com **≥3 caracteres literais** (âncora do índice). |
| "Quais leis tratam de um ramo do direito" | `buscar_por_ontologia` | `l1_code` TPU (ou L2/L3), `family="legislacao"`. Subárvore OJBU. Ex.: `l1_code=1156` (Consumidor → CDC), `14` (Tributário), `899` (Civil → CC). |
| Melhor resultado geral | `buscar_hibrida` | Funde os sinais via RRF. |

Cada resultado de legislação traz a estrutura da norma em `family_meta`
(obra/norma, artigo/dispositivo, path, `unit_kind`, `title`, `external_uri`) e o
deep-link canônico em `link_completo` (`url_oficial`). Filtros comuns: `family`,
`orgao_code`, `ano_min`/`ano_max`, `unit_kind` (`artigo`, `secao`, …), `k` (1-100).

## Regras de citação (obrigatório)

- Cite o número da norma, o artigo e a redação como retornada pela fonte. Sempre
  cite o `link_completo` (URL oficial) e **nunca invente** número de lei, redação
  de artigo ou link.
- Se o registro indicar dispositivo **revogado/alterado**, diga isso
  explicitamente — não apresente texto revogado como vigente. Quando a relação
  alteradora aparecer no grafo (`altera_norma`), aponte a norma e a data.
- Preserve a grafia e os diacríticos exatamente como na fonte (UTF-8).
- Se a norma não estiver na base (`total: 0` ou nenhum resultado),
  **diga isso** em vez de improvisar.
- A cobertura é de **legislação federal**; jurisprudência tem skill própria
  (`pesquisar-jurisprudencia`).

## Boas práticas

- Artigo/dispositivo conhecido → playbook do topo (`obter_codigo` →
  `obter_texto_norma`). Tema sem norma conhecida → `buscar_fts` com
  `family="legislacao"` (refine com `unit_kind="artigo"`), `buscar_semantica` ou
  `buscar_por_ontologia` pelo ramo.
- **O parâmetro de texto das modalidades chama-se `consulta`** (e `padrao` no regex).
  Nome errado de argumento volta com mensagem ACIONÁVEL ("argumento desconhecido:
  'termo' — confira os argumentos aceitos") — corrija o nome e repita; não é
  indisponibilidade do serviço.
- As modalidades `buscar_fts`/`buscar_regex` aceitam paginação keyset (`page_size`,
  `cursor` → `page_info.next_cursor`); se o servidor recusar o `cursor` (versão
  anterior), repita sem os parâmetros de paginação.
- **Tamanho de resposta:** normas inteiras em markdown podem ser grandes (LGPD ≈
  63 KB; códigos muito maiores). O plugin aceita o `userConfig` opcional
  `max_output_kb` (header `X-Iajus-Max-Output-Kb`, 16-8192 KB) que fixa o orçamento
  por chamada no servidor — acima dele a resposta vem truncada com aviso explícito.
  No Claude Code o corte local é `MAX_MCP_OUTPUT_TOKENS`; para citar um artigo,
  prefira `artigo=` / `obter_codigo` em vez do diploma inteiro.
- A chave `ik_*` é injetada pelo cliente MCP no header `Authorization: Bearer`
  (Claude Code: `userConfig`/keychain; Cowork: `managedMcpServers`; Codex:
  `bearer_token_env_var`). **Em 401, repita UMA vez** (cold-start transitório); só
  então peça para revisar a chave configurada — nunca cole a chave em chat.
- **Levantamento amplo / material de estudo:** para varrer um tema em muitas normas,
  paralelize com **subagentes** (ver "Pesquisa em profundidade" em
  **pesquisar-jurisprudencia**). Esta skill alimenta **montar-apostila-didatica** e
  **gerar-questoes-concurso**: ao retornar uma norma/dispositivo, **carregue o
  `link_completo`** (URL oficial do Planalto) — é ele que vira a **citação inline com
  hyperlink** no material em HTML/PDF.
