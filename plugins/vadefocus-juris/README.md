# VadeFocus — plugin Claude Code (jurisprudência + legislação BR)

Um plugin: você ganha **skills** que ensinam o agente a pesquisar/citar
jurisprudência e legislação brasileira **+** o **servidor MCP remoto VadeFocus** já
configurado. Não precisa configurar o MCP na mão.

## O que vem no plugin

| Componente | Conteúdo |
|---|---|
| `skills/pesquisar-jurisprudencia/` | quando e **como** buscar e **citar** acórdãos/súmulas/RG pelas modalidades, **filtrar/agrupar por relator** (jurimetria) e delegar a **subagentes** em levantamentos |
| `skills/consultar-legislacao/` | como localizar leis/artigos **federais** por termo, tema (ontologia) ou citação literal |
| `skills/consultar-legislacao-estadual/` | como consultar legislação **estadual** ao vivo (RJ e MG) por tipo + número + ano |
| `skills/consultar-doutrina/` | como localizar e citar doutrina de autor + a referência ABNT da obra |
| `skills/montar-apostila-didatica/` | monta **apostilas/material de estudo** de várias fontes (juris + lei + doutrina) e gera **HTML + PDF** com fontes rastreáveis |
| `skills/gerar-questoes-concurso/` | gera **questões de concurso** (MP/Magistratura — C/E, múltipla, discursiva, peça/sentença, oral) ancoradas em fonte real, com **gabarito comentado**, em **HTML + PDF** |
| `.mcp.json` | servidor MCP (streamable-HTTP) com header `Authorization: Bearer ${user_config.api_token}` |

As skills são model-invoked: o Claude as usa sozinho quando a tarefa pede
jurisprudência, doutrina ou legislação. Após instalar/habilitar, rode `/reload-plugins`.

> **Nota de nomenclatura.** A marca do produto é **VadeFocus**, mas a *chave do servidor*
> MCP no `.mcp.json` é `iajus` — porque as tools do backend são expostas com o prefixo
> `mcp__iajus__*` e as skills as referenciam por esse nome. Manter a chave `iajus` garante
> que as referências de tool das skills continuem válidas contra o mesmo backend. Tudo o
> que é voltado ao usuário (marketplace, plugin, README, skills) é VadeFocus.

### As 7 modalidades de busca (tools do MCP)

| Tool | Para quê |
|---|---|
| `buscar_semantica` | busca vetorial/densa por significado (padrão para perguntas conceituais) |
| `buscar_hibrida` | fusão RRF (densa + FTS + trigram + CNJ + ontologia) — melhor relevância geral |
| `buscar_fts` | full-text pt_unaccent, stemming PT, insensível a acento |
| `buscar_regex` | regex POSIX (exige ≥3 caracteres literais para ancorar o índice) |
| `buscar_por_cnj` | número de processo CNJ (exato ou por componentes) |
| `buscar_por_ontologia` | ramo/sub-área OJBU via subárvore ltree (L1/L2/L3 TPU + temas transversais) |
| `buscar_jurimetria` | listar/agrupar acórdãos por **relator** e outras colunas estruturadas (relator, tribunal, ano, classe, resultado) — browse tipado ou `count(*)` por bucket |

Todas retornam o mesmo envelope (`{ modalidade, total, resultados:[…] }`), cobrem as
famílias `jurisprudencia` + `legislacao` (+ `doutrina`) e são read-only.

## Como a chave entra (mecanismo `userConfig` — SOTA)

O plugin declara `userConfig.api_token` marcado `sensitive: true` e `required: true`.
Ao **habilitar** o plugin, o Claude Code pede a sua chave `ik_*`, **mascara** o
input e a guarda no **keychain do sistema** — nunca em `settings.json`, nunca no
repositório. O `.mcp.json` injeta `Authorization: Bearer ${user_config.api_token}`
em toda request. **É a instalação de credencial única**: uma coisa instala o plugin
e autentica o MCP.

A chave é validada server-side (hash SHA-256). Sem chave válida → todo `buscar_*`
responde **401**. O "restrito" é imposto na **camada MCP** (os dados), não no
repositório do marketplace (que só tem manifesto, **zero segredo**).

## Instalar

```text
/plugin marketplace add vadefocus/vadefocus-plugin   # repo público do marketplace (cliente)
/plugin install vadefocus-juris@vadefocus
/plugin enable vadefocus-juris@vadefocus              # aqui o Claude pede sua chave ik_* (guardada no keychain)
/reload-plugins                                       # conecta o servidor MCP com a chave
```

O plugin vem **habilitado por padrão** (`defaultEnabled: true`); na instalação o Claude
pede a sua chave `ik_*` (campo obrigatório, guardada no keychain). O endpoint do MCP já
vem fixado no padrão de produção (serviço dedicado VadeFocus, perfil curado):

```
https://vadefocus-mcp.iajus.com.br/mcp
```

### Respostas maiores (opcional — `max_output_kb`)

O campo opcional **`max_output_kb`** define o orçamento de tamanho da resposta de
cada ferramenta (header `X-Iajus-Max-Output-Kb`, aceito entre **16 e 8192 KB**; acima
do orçamento o servidor trunca com aviso explícito). Em branco (padrão), vale o
default do deploy (`IAJUS_MAX_TOOL_OUTPUT_KB`; `0` = sem limite servidor-side). Para
textos integrais de lei/ementas longas no Claude Code, lembre que o corte mais comum
é **do cliente**: aumente também a env `MAX_MCP_OUTPUT_TOKENS`.

> **Plano de distribuição.** O marketplace voltado ao **cliente** é um repositório
> **público** (só manifesto, sem segredo) — porque um marketplace git **privado**
> exigiria que o instalador tivesse credenciais git (token/SSH GitHub), e o cliente
> tem apenas a `ik_*`. O `ik_*` gateia os **dados** no MCP, não o manifesto.

## Codex (mesmo MCP remoto)

O Codex consome MCP via `~/.codex/config.toml`. Aponte para o mesmo endpoint com a
chave por variável de ambiente:

```bash
codex mcp add vadefocus --url https://vadefocus-mcp.iajus.com.br/mcp \
  --bearer-token-env-var VADEFOCUS_API_TOKEN
export VADEFOCUS_API_TOKEN=ik_live_XXXXXXXX_....   # nunca commitar/colar em chat
```

## Compatibilidade de clientes

> OAuth (padrão): Claude.ai web, ChatGPT e Claude Desktop conectam com zero chave,
> via fluxo de navegador contra o AS self-hosted `app.iajus.com.br` (DCR, sem client
> secret). Bearer estático `ik_*` é o fallback documentado para clientes headless que
> aceitam token fixo: Claude Code, Cursor, Gemini CLI, Codex (headless). A chave é
> validada server-side; nunca trafega em log. **Não cole a chave em commits ou chat.**
