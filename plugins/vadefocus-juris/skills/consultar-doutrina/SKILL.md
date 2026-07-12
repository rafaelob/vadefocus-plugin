---
name: consultar-doutrina
description: Consulta e cita DOUTRINA jurídica brasileira real (livros, manuais e tratados de autores) pelo MCP VadeFocus, com a referência ABNT. Acione quando o usuário quiser o ensinamento doutrinário - o que a DOUTRINA/um DOUTRINADOR ensina sobre um instituto, "o que Hely Lopes/Pacelli/a doutrina diz sobre X", "qual livro trata de Y", citar um manual/tratado, ou a referência bibliográfica de uma obra. NÃO use para acórdãos/súmulas (pesquisar-jurisprudencia) nem texto de leis (consultar-legislacao).
allowed-tools: mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa, mcp__iajus__obter_secao_doutrina, mcp__plugin_vadefocus-juris_iajus__obter_secao_doutrina
---

# Consultar doutrina jurídica brasileira (VadeFocus)

Você tem acesso ao servidor MCP VadeFocus, que indexa **doutrina** jurídica brasileira
- livros, manuais e tratados de autores, decompostos na árvore documental real da
obra (livro → parte → título → capítulo → seção, até 6 níveis) e classificados pela
ontologia OJBU alinhada ao CNJ/TPU (21 ramos L1 → sub-áreas L2/L3). **Use o MCP em
vez de inventar o que "a doutrina diz".**

A doutrina vive na família `doutrina` do acervo unificado (`search_units`), ao lado de
`jurisprudencia` e `legislacao`. As mesmas tools de busca alcançam as três famílias; o
que muda é a intenção: **doutrina = ensinamento de autor**, não precedente nem norma.

## Quando NÃO usar esta skill

- Precedente, acórdão, súmula, repercussão geral, número de processo → use
  **pesquisar-jurisprudencia**.
- Texto de lei, artigo de código, decreto, vigência de norma → use
  **consultar-legislacao**.
- Esta skill é para o **conteúdo doutrinário** (o que o autor/manual ensina) e para a
  **referência bibliográfica** das obras do acervo.

## Escolha da modalidade

Todas as tools retornam o mesmo envelope uniforme (`{ modalidade, total, resultados:[…] }`),
são read-only, e aceitam o filtro de família. **Restrinja a `family="doutrina"`** (ou o
parâmetro de família equivalente da tool) para não misturar acórdãos/leis nos resultados.

| Pergunta do usuário | Tool | Por quê |
|---|---|---|
| Conceito/instituto ("o que a doutrina ensina sobre posse vs propriedade") | `buscar_semantica` | Vetorial/densa - casa por significado. **Padrão para perguntas conceituais.** Requer o índice denso da família doutrina (premium gemini-2). Se a família ainda não estiver densa, caia para `buscar_por_ontologia` + `buscar_fts`. |
| Melhor resultado geral | `buscar_hibrida` | Funde semântica + FTS + trigram + ontologia via RRF. |
| Termo técnico literal ("usucapião extraordinária", "tipicidade conglobante") | `buscar_fts` | Full-text pt_unaccent, stemming PT, insensível a acento; `phrase=true` exige a ordem. |
| Forma literal / nº de artigo citado no livro ("art. 1.228", "Súmula 7") | `buscar_regex` | Regex POSIX; **exija ≥3 caracteres literais** (âncora do índice). |
| "Toda a doutrina de um ramo" ("livros de Direito Penal") | `buscar_por_ontologia` | Subárvore ltree por `l1_code` TPU; combine com `family="doutrina"`. Ex.: `l1_code=287` (Penal), `899` (Civil), `9985` (Administrativo). |

Exemplo real de chamada (o parâmetro de texto chama-se **`consulta`** - nunca `termo`/
`query`; nome errado volta como o erro genérico *"erro interno ao processar a
consulta"*, então ao ver essa mensagem confira primeiro os nomes dos argumentos):

```json
buscar_semantica {"consulta": "função social da posse", "family": "doutrina", "space": "premium", "k": 10}
buscar_fts       {"consulta": "tipicidade conglobante", "family": "doutrina", "k": 5}
```

Notas de uso:
- **Filtro de órgão NÃO se aplica à doutrina** (livro não tem tribunal). Use o filtro de
  **família** (`family="doutrina"`); `buscar_por_ontologia`/`buscar_fts`/`buscar_regex`
  filtram por família/`l1_code`, não por `tribunal`.
- **Trecho completo (seção inteira da obra):** os hits trazem `snippet` (recorte) +
  `family_meta` com `unit_id`, `external_uri` e `ref_interna` (que carrega `documento_id`
  e `secao_anchor`). Para o texto integral da SEÇÃO use
  `obter_secao_doutrina(documento_id=family_meta.ref_interna.documento_id, anchor=family_meta.ref_interna.secao_anchor)`
  - devolve `obra_titulo`, `section_title`, `breadcrumbs` e páginas (sujeito ao teto de
  direitos autorais; `anchor` é o nome certo do argumento, não `secao_anchor`/`unit_id`).
  Sem o anchor, dá para localizar por `obter_secao_doutrina(documento_id=…, titulo_secao="…")`
  (se ambíguo, devolve `candidatos[]` com os anchors). `obter_secao_doutrina` também aceita
  **`entity_id`** da obra no lugar de `documento_id` (informe UM dos dois) - mas sempre com
  `anchor` OU `titulo_secao` junto; `entity_id` **sozinho**, sem seção, não resolve.
  Alternativa por unidade:
  `obter_unidade_completa(unit_id=family_meta.unit_id, family="doutrina")` (esta usa `unit_id`,
  não `entity_id`). Na falta de ambos, cite pelo snippet + referência.
- `buscar_semantica`/`buscar_hibrida` aceitam `space` (`premium` = gemini-2 hierárquico,
  o tier da doutrina) e `k` (1-100). Prefira `premium` para doutrina - o embedding é
  hierárquico (seção + breadcrumb), pensado para texto de manual.
- Se `buscar_semantica` vier fraco, **escale para `buscar_hibrida`** (mesma consulta)
  antes de desistir. Se a família densa ainda não estiver pronta, use `buscar_por_ontologia`
  (recorte por ramo) + `buscar_fts` (termo) como caminho léxico.

## Ontologia OJBU (ramos reais - use estes códigos)

`buscar_por_ontologia` recebe `l1_code` (código TPU do ramo, inteiro) + `family="doutrina"`.
Os ramos com obras no acervo (entre os 21 L1):

| `l1_code` | Ramo | | `l1_code` | Ramo |
|---|---|---|---|---|
| 14 | Tributário | | 1209 | Processual Penal |
| 287 | Penal | | 7724 | Empresarial |
| 899 | Civil | | 8826 | Processual Civil e do Trabalho |
| 9985 | Administrativo e Dir. Público | | | |

(Os demais ramos L1 e os temas transversais existem na ontologia, mas o acervo atual de
doutrina concentra-se nesses; obras meta/transversais - teoria geral, prática - podem não
ter um ramo único e aparecem sem classificação L1, honestamente, em vez de um ramo forçado.)

## Acervo de obras (catálogo - 41 obras)

O acervo tem **41 obras** de doutrina: **38 já pesquisáveis** + **3 pendentes de ingestão**
(ainda NÃO no MCP - *Direito Administrativo* de Ricardo Alexandre e João de Deus; *Código de
Processo Civil Comentado* de Daniel Amorim; *Direito Tributário* de Ricardo Alexandre). A lista
completa por ramo, com a referência de cada obra e o que está pendente, está em
**`references/CATALOGO_DOUTRINA.md`** - **consulte-a** para: (a) confirmar se uma obra/autor existe
no acervo antes de citar (anti-alucinação); (b) **não** citar as 3 pendentes como se já estivessem
disponíveis; (c) lembrar que as referências ABNT estão **incompletas na fonte** (sentinels). Se uma
busca retornar obra fora do catálogo, confie na busca - o catálogo é um snapshot (2026-06-22).

## Como citar (obrigatório)

- Cite pelos campos que as tools **realmente devolvem**: `obter_secao_doutrina` retorna
  `autor`, `obra_titulo`, `section_title`, `breadcrumbs` e `page_start`/`page_end`; os hits de
  busca trazem `family_meta.title`, `family_meta.unit_id`, `family_meta.ref_interna`,
  `family_meta.external_uri` e - quando a obra tem esse dado no acervo - `family_meta.referencia_abnt`
  (a referência ABNT já pronta) + `referencia_completa`/`pagina_secao` (o `breadcrumbs` vem **só**
  de `obter_secao_doutrina`, não dos hits). **Se vier `referencia_abnt`, use-a**; ela costuma vir
  **incompleta** (a fonte não traz local/editora/ano - ver abaixo), então complete os campos que
  faltarem com os marcadores ABNT em vez de descartá-la. Quando não vier, **monte** a referência a
  partir dos demais campos - `AUTOR. *Título*. seção, p. X` (ABNT NBR 6023).
- **Atenção honesta:** a fonte (PDF) **não traz local, editora nem ano**, então a referência fica
  **incompleta** - use os marcadores ABNT `[S. l.]` (local), `[s. n.]` (editora) e `[20--?]` (ano).
  **Não invente** esses dados para "completar"; sinalize que a fonte não os informou. A lista das
  41 obras do acervo está em `references/CATALOGO_DOUTRINA.md`.
- Cite o capítulo/seção (`breadcrumb`/`section_title`) e a página (`page_start`) quando
  presentes - a doutrina é citada por trecho, não pela obra inteira.
- Resuma o trecho retornado (`snippet`) em 1-2 frases; ofereça abrir o trecho pelo
  `external_uri` quando houver.
- Se a busca **não** retornar trecho relevante (`total: 0` ou hits fracos), **diga isso
  honestamente** - não preencha a lacuna com uma "posição da doutrina" fabricada nem com
  um autor que não está no acervo.

## Boas práticas

- Comece restrito (conceito + ramo via `l1_code` + `family="doutrina"`) e amplie só se vier vazio.
- Distinga DOUTRINA de JURISPRUDÊNCIA na resposta: o que o autor *defende* (doutrina) não
  é o que o tribunal *decidiu* (jurisprudência). Se o usuário quer os dois, combine esta
  skill com **pesquisar-jurisprudencia** e deixe claro o que é cada um.
- **Levantamento amplo:** para varrer um ramo inteiro ou montar a base de um material,
  paralelize com **subagentes** (ver a seção "Pesquisa em profundidade" da skill
  **pesquisar-jurisprudencia**) - um subagente por sub-tema/autor. Esta skill alimenta
  **montar-apostila-didatica** e **gerar-questoes-concurso**: quando o destino for material
  de estudo, já retorne `autor` + `obra_titulo` + seção (`breadcrumb`/`section_title`) + `page_start`
  prontos - use o `family_meta.referencia_abnt` do hit quando vier, e monte a referência a partir
  desses campos quando não vier (a string pronta pode ou não acompanhar o hit).
- Preserve diacríticos e UTF-8 exatamente como na fonte.
- A chave `ik_*` é injetada pelo cliente MCP no header `Authorization: Bearer`
  (Claude Code: `userConfig`/keychain; Cowork: `managedMcpServers`; Codex:
  `bearer_token_env_var`). **Em 401, repita UMA vez** (o primeiro acesso após
  cold-start pode falhar transitoriamente); só se persistir avise o usuário para
  revisar a chave configurada - nunca cole a chave em chat nem em commit.
- **Respostas grandes:** o plugin aceita o `userConfig` opcional `max_output_kb`
  (header `X-Iajus-Max-Output-Kb`, 16-8192 KB) que fixa o orçamento de resposta por
  chamada no servidor; acima dele a resposta vem truncada com aviso explícito. No
  Claude Code o corte local é `MAX_MCP_OUTPUT_TOKENS`.
