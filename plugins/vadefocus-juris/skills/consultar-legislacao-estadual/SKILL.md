---
name: consultar-legislacao-estadual
description: Consulta legislação ESTADUAL (RJ e MG) e MUNICIPAL (Rio de Janeiro e Belo Horizonte) ao vivo pelo MCP VadeFocus — leis, decretos e normas das assembleias/câmaras, na fonte oficial. Acione sempre que o usuário pedir uma lei/decreto/norma de ESTADO ou MUNICÍPIO do escopo, o texto íntegra de uma lei estadual/municipal, "lei estadual nº Y de MG", "lei municipal de BH número Z". A consulta é por UF (ou município) + tipo + número + ano (a fonte oficial é a verdade — nunca cite de memória). NÃO use para legislação FEDERAL (skill consultar-legislacao), precedentes/acórdãos (skill pesquisar-jurisprudencia) nem doutrina de autor (skill consultar-doutrina).
allowed-tools: mcp__iajus__consultar_legislacao_estadual, mcp__plugin_vadefocus-juris_iajus__consultar_legislacao_estadual, mcp__iajus__obter_texto_legislacao_estadual, mcp__plugin_vadefocus-juris_iajus__obter_texto_legislacao_estadual, mcp__iajus__legislacao_estadual_status, mcp__plugin_vadefocus-juris_iajus__legislacao_estadual_status, mcp__iajus__consultar_legislacao_municipal, mcp__plugin_vadefocus-juris_iajus__consultar_legislacao_municipal, mcp__iajus__obter_texto_legislacao_municipal, mcp__plugin_vadefocus-juris_iajus__obter_texto_legislacao_municipal, mcp__iajus__legislacao_municipal_status, mcp__plugin_vadefocus-juris_iajus__legislacao_municipal_status
---

# Consultar legislação estadual e municipal ao vivo (VadeFocus)

Você tem acesso ao servidor MCP VadeFocus, que consulta **legislação estadual e
municipal** ao vivo na fonte oficial (assembleia legislativa / câmara municipal /
diário oficial). A consulta é em tempo real — use o MCP em vez de citar de memória:
**o texto da fonte oficial é a verdade**.

Estas tools vivem no **mesmo** servidor MCP `iajus` das demais skills — mesma URL,
mesma chave `ik_*`. Não há credencial nem host novos.

## Escopo do plano VadeFocus (importante — verificado ao vivo)

O serviço VadeFocus cobre **exatamente**:

- **Estadual:** **RJ** e **MG**. Qualquer outra UF (SP, BA, DF, …) é **recusada pelo
  plano** — a resposta vem como erro/negativa, NÃO como "norma não existe". Diga ao
  usuário que a UF está fora da cobertura do plano VadeFocus (hoje a recusa pode
  chegar como um erro técnico curto; trate-a como fora de escopo, nunca como
  indisponibilidade temporária nem como inexistência da norma).
- **Municipal:** **Rio de Janeiro** e **Belo Horizonte** (aliases aceitos: "RJ",
  "Rio", "BH"). Outros municípios são recusados da mesma forma.

Prontidão real por fonte (chame `legislacao_estadual_status` /
`legislacao_municipal_status` na dúvida):

- **MG (ALMG)** — `ready`, verificada ao vivo: uma chamada já traz ementa + texto
  integral. Exemplo real que funciona:
  ```json
  consultar_legislacao_estadual {"uf": "MG", "tipo": "LEI", "numero": "14184", "ano": 2002}
  obter_texto_legislacao_estadual {"uf": "MG", "tipo": "LEI", "numero": "14184", "ano": 2002}
  ```
  (metadados + link oficial ALMG; o texto integral veio com ~25 KB em markdown.)
- **RJ (ALERJ)** — best-effort (`stub`): a base é Lotus Notes/Domino com busca
  imprecisa — a maioria das consultas por número NÃO resolve; quando resolve, o texto
  vem completo e verificado. Se não resolver, **diga que a norma do RJ não pôde ser
  localizada na fonte** — nunca invente.
- **Belo Horizonte (CMBH)** — `ready` para resolução. Exemplo real:
  ```json
  consultar_legislacao_municipal {"municipio": "Belo Horizonte", "tipo": "LEI", "numero": "8616", "ano": 2003}
  ```
  → ementa ("Código de Posturas…") + `link_completo` oficial. **Atenção:** o texto
  integral nem sempre é extraível (`tem_texto_integral: false` /
  `aviso: "…texto integral não pode ser extraído…"`) — nesse caso entregue o
  `link_completo` ao usuário e diga que o inteiro teor está no link.
- **Rio de Janeiro (CMRJ)** — resolve norma + ementa de forma confiável; inteiro
  teor best-effort.

> Regra REAL: nunca afirme que uma norma existe se a tool não a retornou; e nunca
> afirme que "não existe" quando a resposta foi recusa de escopo ou falha da fonte.

## Quando NÃO usar esta skill

- Lei/decreto/norma **FEDERAL** (Planalto, Senado) → skill **consultar-legislacao**.
- Precedente, acórdão, súmula, repercussão geral → skill **pesquisar-jurisprudencia**.
- Doutrina de autor (livro/manual/tratado) → skill **consultar-doutrina**.

## Como consultar

A identidade da norma é obrigatória: **UF (ou município) + tipo + número + ano**
(a consulta resolve por identidade, não por busca textual livre).

| Necessidade | Tool | Argumentos |
|---|---|---|
| Metadados de norma estadual (link, ementa, data) | `consultar_legislacao_estadual` | `uf`, `tipo`, `numero`, `ano` |
| Texto íntegra estadual (markdown hierárquico) | `obter_texto_legislacao_estadual` | `uf`, `tipo`, `numero`, `ano` |
| Metadados de norma municipal | `consultar_legislacao_municipal` | `municipio`, `tipo`, `numero`, `ano` |
| Texto íntegra municipal | `obter_texto_legislacao_municipal` | `municipio`, `tipo`, `numero`, `ano` |
| Prontidão da cobertura | `legislacao_estadual_status` / `legislacao_municipal_status` | (sem argumentos) |

Notas de uso:
- **`uf`/`municipio` é sempre obrigatória.** Sem ela a consulta é ambígua — peça ao
  usuário antes de chamar a tool (leis de mesmo número existem em entes diferentes).
- `tipo` é a espécie normativa (`LEI`, `DECRETO`, `LEI COMPLEMENTAR`, …); `numero` e
  `ano` identificam a norma. Esta consulta **não** faz busca por tema — se o usuário
  só descreve o assunto, peça (ou ajude a descobrir) tipo/número/ano.
- Todos os 4 argumentos são obrigatórios; chamada com argumento faltando volta como o
  erro genérico *"erro interno ao processar a consulta"* — ao ver essa mensagem,
  confira primeiro se passou `uf`/`municipio`, `tipo`, `numero` e `ano` com esses
  nomes exatos.
- A consulta é **ao vivo**: pode ser mais lenta e depende da fonte oficial. Se a
  fonte estiver fora do ar ou não retornar a norma, o campo `erro`/`aviso` diz isso —
  **repasse ao usuário**, não invente.

## Regras de citação (obrigatório)

- Cite o **ente** (UF ou município), o tipo, o número/ano e a redação como retornada
  pela fonte. Sempre inclua o **`link_completo`** (URL oficial deep-per-norma) e
  **nunca invente** número, redação ou link.
- Deixe claro que a fonte é **estadual/municipal** e de **qual ente**.
- Preserve grafia e diacríticos exatamente como na fonte (UTF-8).
- Se a norma não for encontrada, **diga isso** — cobertura ao vivo tem lacunas
  conhecidas (especialmente RJ estadual).

## Boas práticas

- Use `consultar_*` para confirmar a norma + pegar link e data; só então
  `obter_texto_*` se o usuário quiser o inteiro teor.
- **Texto integral grande** (norma extensa): o plugin aceita o `userConfig` opcional
  `max_output_kb` (header `X-Iajus-Max-Output-Kb`, 16-8192 KB) que fixa o orçamento
  de resposta por chamada; acima dele a resposta vem truncada com aviso explícito. No
  Claude Code o corte local é `MAX_MCP_OUTPUT_TOKENS`.
- A chave `ik_*` é injetada pelo cliente MCP no header `Authorization: Bearer`
  (Claude Code: `userConfig`/keychain; Cowork: `managedMcpServers`; Codex:
  `bearer_token_env_var`). **Em 401, repita UMA vez** (cold-start transitório); só
  se persistir avise o usuário para revisar a chave — nunca cole a chave em chat nem
  em commit.
