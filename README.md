# VadeFocus — marketplace de plugin Claude Code

Marketplace público do plugin **VadeFocus** (jurisprudência, doutrina e legislação
brasileira via MCP remoto). Este repositório contém **apenas manifesto + skills —
zero segredo**: o acesso aos dados é gateado pela sua chave `ik_*` na camada MCP.

| Item | Valor |
|---|---|
| Marketplace | `vadefocus` (este repo) |
| Plugin | [`vadefocus-juris`](plugins/vadefocus-juris/README.md) |
| Servidor MCP | remoto (streamable-HTTP), autenticado por `Authorization: Bearer ik_*` |
| Skills | pesquisar-jurisprudencia (com filtro por relator), consultar-legislacao, consultar-legislacao-estadual, consultar-doutrina, montar-apostila-didatica, gerar-questoes-concurso (material em HTML+PDF) |

## Instalação em 5 passos

1. **Adicionar o marketplace** (terminal ou `/plugin` dentro do Claude Code):

   ```bash
   claude plugin marketplace add rafaelob/vadefocus-plugin
   ```

2. **Instalar o plugin**:

   ```bash
   claude plugin install vadefocus-juris@vadefocus
   ```

   No Claude Code interativo: `/plugin install vadefocus-juris@vadefocus`.

3. **Habilitar e configurar** — o plugin já vem **habilitado por padrão**
   (`defaultEnabled: true`); na primeira ativação o Claude Code pede a configuração:

   ```bash
   claude plugin enable vadefocus-juris@vadefocus
   ```

   - **VadeFocus API Key** (`api_token`, obrigatória): sua chave `ik_…`.
     É mascarada e guardada no keychain do sistema — nunca em `settings.json`
     nem neste repositório. Solicite em <https://vadefocus.com.br>.
   O endpoint do MCP já vem **fixado no padrão de produção** (serviço dedicado
   VadeFocus, perfil curado): `https://vadefocus-mcp.iajus.com.br/mcp`.

   Depois rode `/reload-plugins` (ou reinicie a sessão) para conectar o MCP.

4. **Testar** — em uma sessão do Claude Code, peça:

   > liste as tools do servidor iajus

   Esperado: as 8 modalidades de busca (`buscar_semantica`, `buscar_hibrida`,
   `buscar_fts`, `buscar_regex`, `buscar_por_cnj`, `buscar_por_ontologia`,
   `buscar_grafo`, `buscar_jurimetria` — filtro/agrupamento por relator) e demais
   tools do perfil curado VadeFocus.

5. **Se receber 401** — a chave não foi aceita. Repita **uma vez**
   (cold-start do serviço pode atrasar a primeira validação); se o 401
   persistir, a chave está errada, expirada ou revogada — confira o valor
   `ik_…` na configuração do plugin (`/plugin` → vadefocus-juris → Configure)
   e solicite uma nova chave se necessário.

## Segurança

- Este repositório é **público por desenho**: só manifesto, skills e
  documentação. Nenhuma chave, nenhum dado.
- A chave `ik_*` autentica **os dados** no servidor MCP (validação server-side,
  SHA-256). Sem chave válida, toda tool responde 401.
- **Nunca** cole sua chave em commits, issues ou chat.

## Documentação do plugin

Detalhes das skills, das 8 modalidades de busca e do uso via Codex/outros
clientes: [`plugins/vadefocus-juris/README.md`](plugins/vadefocus-juris/README.md).
