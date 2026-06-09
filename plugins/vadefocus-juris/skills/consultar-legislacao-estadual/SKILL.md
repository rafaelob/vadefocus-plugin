---
name: consultar-legislacao-estadual
description: Consulta legislação ESTADUAL brasileira ao vivo (SP, MG, BA com texto integral; DF resolve+ementa; RJ best-effort) — leis, decretos e normas das assembleias/diários oficiais estaduais — pelo MCP iaJus, com busca em tempo real na fonte oficial de cada UF. Acione sempre que o usuário pedir uma lei/decreto/norma de um ESTADO específico, o texto íntegra de uma lei estadual, "lei estadual nº Y de SP", "decreto do RJ número Z", "lei complementar do DF". A consulta é por UF + tipo + número + ano (a fonte oficial é a verdade — nunca cite de memória). NÃO use para legislação FEDERAL (skill consultar-legislacao), precedentes/acórdãos (skill pesquisar-jurisprudencia) nem doutrina de autor (skill consultar-doutrina).
allowed-tools: mcp__iajus__consultar_legislacao_estadual, mcp__plugin_vadefocus-juris_iajus__consultar_legislacao_estadual, mcp__iajus__obter_texto_legislacao_estadual, mcp__plugin_vadefocus-juris_iajus__obter_texto_legislacao_estadual, mcp__iajus__legislacao_estadual_status, mcp__plugin_vadefocus-juris_iajus__legislacao_estadual_status
---

# Consultar legislação estadual brasileira ao vivo (iaJus)

Você tem acesso ao servidor MCP `iajus`, que consulta **legislação estadual**
brasileira **ao vivo** na fonte oficial de cada UF (assembleia legislativa /
diário oficial estadual). A consulta é em tempo real — use o MCP em vez de citar de
memória: **o texto da fonte oficial é a verdade**.

Estas tools vivem no **mesmo** servidor MCP `iajus` das demais skills — mesma URL,
mesma chave `ik_*`. Não há credencial nem host novos.

## Cobertura por UF (prontidão real)

Antes de consultar uma UF nova, você pode chamar `legislacao_estadual_status` para
ver a prontidão. Estado atual:

- **Prontas** (`ready` — verificadas ao vivo: fetch 2xx + texto integral real):
  **SP** (ALESP), **MG** (ALMG API v2 — uma chamada já traz ementa + texto integral) e
  **BA** (LegislaBahia — ementa + texto integral).
- **Resolve + ementa** (`resolve_ementa`): **DF** (CLDF PLE API — resolve a norma + ementa
  + link oficial de forma confiável; o inteiro teor vem via SINJ-DF na maioria dos casos,
  mas é best-effort — o campo `tem_texto_integral` por consulta avisa se o texto veio).
- **Não confirmada** (`unconfirmed` — adaptador existe, mas a resolução ao vivo ainda não
  foi confirmada): **CE, PE, RN, RO** — pode resolver ou não; sempre confira com
  `legislacao_estadual_status` e, se falhar, **diga que a UF ainda não está confirmada**.
- **Best-effort** (`stub`): **RJ** (a base da ALERJ é Lotus Notes/Domino com busca
  full-text imprecisa — a maioria das consultas por número NÃO resolve; quando resolve,
  o texto vem completo e o número é verificado no cabeçalho do documento). Se não
  resolver, **diga que a norma do RJ não pôde ser localizada na fonte** — nunca invente.
- **Federado** (`federated`): UFs sem adaptador nativo → tentativa via índice LexML
  federado (só link, sem texto; pode não resolver). Se não resolver, **diga honestamente**
  que a UF ainda não tem cobertura nativa — não improvise a norma.

> Regra REAL: nunca afirme que uma norma existe se a tool não a retornou. `ready` = só as
> UFs com prova ao vivo de texto integral (hoje SP, MG, BA). Para `resolve_ementa`
> (DF), `unconfirmed`, `stub` (RJ) e `federated`, modere a expectativa e seja honesto sobre
> lacunas — a cobertura ao vivo de legislação estadual ainda está em expansão.

## Quando NÃO usar esta skill

- Lei/decreto/norma **FEDERAL** (Planalto, Senado) → skill **consultar-legislacao**.
- Precedente, acórdão, súmula, repercussão geral → skill **pesquisar-jurisprudencia**.
- Doutrina de autor (livro/manual/tratado) → skill **consultar-doutrina**.

## Como consultar

A **UF é obrigatória** em toda consulta (legislação estadual é por estado), junto com
**tipo + número + ano** (a consulta resolve a norma por identidade, não por busca
textual livre).

| Necessidade | Tool | Argumentos |
|---|---|---|
| Metadados de uma norma (link oficial, ementa, data) | `consultar_legislacao_estadual` | `uf`, `tipo`, `numero`, `ano` |
| Texto íntegra (markdown hierárquico) de uma norma | `obter_texto_legislacao_estadual` | `uf`, `tipo`, `numero`, `ano` |
| Saber a prontidão da cobertura por UF | `legislacao_estadual_status` | (sem argumentos) |

Notas de uso:
- **`uf` é sempre obrigatória.** Sem UF a consulta é ambígua — peça a UF ao usuário
  antes de chamar a tool (leis de mesmo número existem em estados diferentes).
- `tipo` é a espécie normativa (`LEI`, `DECRETO`, `LEI COMPLEMENTAR`, …); `numero` e
  `ano` identificam a norma. Esta consulta **não** faz busca por tema/assunto — se o
  usuário só descreve o assunto, peça (ou ajude a descobrir) tipo/número/ano.
- A consulta é **ao vivo**: pode ser mais lenta e depende da fonte oficial. Se a fonte
  estiver fora do ar ou não retornar a norma, o campo `erro`/`aviso` diz isso —
  **repasse ao usuário**, não invente.

## Regras de citação (obrigatório)

- Cite a **UF**, o tipo, o número/ano e a redação como retornada pela fonte. Sempre
  inclua o **`link_completo`** (URL oficial deep-per-norma) e **nunca invente** número,
  redação ou link.
- Deixe claro que a fonte é **estadual** e de **qual UF**.
- Preserve grafia e diacríticos exatamente como na fonte (UTF-8).
- Se a norma não for encontrada (ou a UF for federada/best-effort e falhar), **diga
  isso** — legislação estadual ao vivo tem lacunas de cobertura conhecidas.

## Boas práticas

- Para uma UF que você não sabe se está coberta, chame `legislacao_estadual_status`
  primeiro e ajuste a expectativa (ready vs best-effort vs federated).
- Use `consultar_legislacao_estadual` para confirmar a norma + pegar o link e a data;
  só então `obter_texto_legislacao_estadual` se o usuário quiser o inteiro teor.
- A chave `ik_*` é injetada pelo cliente MCP no header `Authorization: Bearer`
  (Claude Code: `userConfig`/keychain; Cowork: `managedMcpServers`; Codex:
  `bearer_token_env_var`). Se uma tool responder 401, avise o usuário para revisar a
  chave — nunca cole a chave em chat nem em commit.
