# VadeFocus — plugin Claude Code (jurisprudência + legislação BR)

Um plugin: você ganha **skills** que ensinam o agente a pesquisar, **redigir** e **citar**
jurisprudência, legislação e doutrina brasileira **+** o **servidor MCP remoto VadeFocus** já
configurado. Não precisa configurar o MCP na mão.

## O que vem no plugin

| Componente | Conteúdo |
|---|---|
| `skills/pesquisar-jurisprudencia/` | quando e **como** buscar e **citar** acórdãos/súmulas/RG pelas modalidades, **filtrar por relator** e usar a **jurimetria agregada exata**, e delegar a **subagentes** em levantamentos |
| `skills/consultar-legislacao/` | como localizar leis/artigos **federais** por termo, tema (ontologia) ou citação literal |
| `skills/consultar-legislacao-estadual/` | como consultar legislação **estadual** ao vivo (RJ e MG) por tipo + número + ano |
| `skills/consultar-doutrina/` | como localizar e citar doutrina de autor + a referência ABNT da obra |
| `skills/redigir-peca-juridica/` | redige **peça/parecer/memorial/voto/ementa** com **linguagem jurídica** correta e **ementa no padrão CNJ** (Recomendação 154/2024), ancorada em fonte real; método relator/revisor/redator |
| `skills/montar-apostila-didatica/` | monta **apostilas/material de estudo** de várias fontes (juris + lei + doutrina) e gera **HTML + PDF** com fontes rastreáveis |
| `skills/gerar-questoes-concurso/` | gera **questões de concurso** (MP/Magistratura — C/E, múltipla, discursiva, peça/sentença, oral) ancoradas em fonte real, com **gabarito comentado**, em **HTML + PDF** |
| `.mcp.json` | servidor MCP (streamable-HTTP) com a chave `ik_*` **já embutida** no header `Authorization: Bearer` |

As skills são model-invoked: o Claude as usa sozinho quando a tarefa pede
jurisprudência, doutrina ou legislação. Após instalar/habilitar, rode `/reload-plugins`.

> **Nota de nomenclatura.** A marca do produto é **VadeFocus**, mas a *chave do servidor*
> MCP no `.mcp.json` é `iajus` — porque as tools do backend são expostas com o prefixo
> `mcp__iajus__*` e as skills as referenciam por esse nome. Manter a chave `iajus` garante
> que as referências de tool das skills continuem válidas contra o mesmo backend. Tudo o
> que é voltado ao usuário (plugin, README, skills) é VadeFocus.

### As 6 modalidades de busca + jurimetria agregada (tools do MCP)

| Tool | Para quê |
|---|---|
| `buscar_semantica` | busca vetorial/densa por significado (padrão para perguntas conceituais) |
| `buscar_hibrida` | fusão RRF (densa + FTS + trigram + CNJ + ontologia) — melhor relevância geral; aceita faceta `relator_norm` + `classe_cnj_code` (jurisprudência) |
| `buscar_fts` | full-text pt_unaccent, stemming PT, insensível a acento |
| `buscar_regex` | regex POSIX (exige ≥3 caracteres literais para ancorar o índice) |
| `buscar_por_cnj` | número de processo CNJ (exato ou por componentes) |
| `buscar_por_ontologia` | ramo/sub-área OJBU via subárvore ltree (L1/L2/L3 TPU + temas transversais) |
| `jurimetria_volume` / `jurimetria_relator` / `jurimetria_classe` / `jurimetria_orgao_julgador` / `jurimetria_resultado` / `jurimetria_lag_publicacao` | jurimetria agregada EXATA do read-model (volume por órgão × ano, ranking de relatores, classe CNJ, câmara/turma, taxa de desfecho com denominador duplo, lag de publicação), com envelope de honestidade |

Todas retornam o mesmo envelope (`{ modalidade, total, resultados:[…] }`), cobrem as
famílias `jurisprudencia` + `legislacao` (+ `doutrina`) e são read-only.

## Como é distribuído (bundle pré-configurado, chave embutida)

VadeFocus é entregue como um **bundle pré-keyed** — um pacote que já vem com a sua chave
`ik_*` **embutida** no `.mcp.json` e o endpoint do MCP fixado. **Não há marketplace git
público para adicionar**: o pacote é gerado por assinante (uma chave por assinante) e
entregue pela plataforma (Cowork), pronto para uso. Você instala o pacote e usa — não
digita chave, não adiciona marketplace.

O endpoint de produção (serviço dedicado VadeFocus, perfil curado) já vem fixado no pacote:

```
https://vadefocus-mcp.iajus.com.br/mcp
```

A chave `ik_*` é validada server-side (hash SHA-256) e gateia os **dados** na camada MCP:
sem chave válida → todo `buscar_*` responde **401**. O "restrito" é imposto no servidor MCP
(os dados), não num repositório de marketplace.

> **Onde vive a chave.** A chave embutida existe SÓ no pacote entregue ao assinante (o
> artefato keyed, gerado fora do repositório). O repositório **nunca** carrega a chave: o
> `.mcp.json` versionado traz apenas o placeholder `SUA_CHAVE_ik`, substituído pela chave
> real na geração do pacote. Nunca cole a chave em commit, chat, print ou na URL.

## Compatibilidade de clientes

> OAuth (padrão): Claude.ai web, ChatGPT e Claude Desktop conectam com zero chave, via
> fluxo de navegador contra o AS self-hosted `app.iajus.com.br` (DCR, sem client secret).
> O bundle pré-keyed (Bearer `ik_*` embutido) é o caminho para clientes headless que
> aceitam token fixo: Claude Code, Cursor, Gemini CLI, Codex (headless). A chave é validada
> server-side; nunca trafega em log.
