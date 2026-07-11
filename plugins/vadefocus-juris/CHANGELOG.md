# Changelog — vadefocus (plugin publico)

Formato baseado em [Keep a Changelog](https://keepachangelog.com/); versionamento [SemVer](https://semver.org/).

## [2.0.0] — 2026-07-10

Corte MCP v3: renomeacao da superficie de tools para o vocabulario final (``buscar_*`` busca ranqueada, ``obter_*`` fetch de um recurso, ``listar_*`` enumeracao, ``jurimetria_*`` agregados). Bump MAJOR porque nomes de tool mudam (quebra clientes que chamavam os nomes antigos). Contagem final de tools por perfil: iajus 34 · vadefocus 32.

### Mudado

De-para das tools que este plugin referencia:

| Nome antigo | Nome novo |
|---|---|
| `buscar_legislacao_federal` | `buscar_norma_fonte_oficial` |
| `consultar_codigo` | `obter_codigo` |
| `consultar_grafo_legislacao` | `obter_grafo_norma` |
| `consultar_informativos_stf` | `buscar_informativos_stf` |
| `consultar_informativos_stj` | `buscar_informativos_stj` |
| `consultar_legislacao_estadual` | `buscar_norma_fonte_oficial` |
| `consultar_legislacao_federal` | `buscar_norma_fonte_oficial` |
| `consultar_legislacao_municipal` | `buscar_norma_fonte_oficial` |
| `consultar_ontologia_juridica` | `obter_ontologia_juridica` |
| `consultar_qualificada` | `buscar_qualificada` |
| `legislacao_estadual_status` | `obter_cobertura_legislacao` |
| `legislacao_municipal_status` | `obter_cobertura_legislacao` |
| `ler_dispositivo_legal` | `obter_dispositivo_legal` |
| `listar_legislacao_federal` | `listar_normas` |
| `obter_texto_legislacao` | `obter_texto_norma` |
| `obter_texto_legislacao_estadual` | `obter_texto_norma` |
| `obter_texto_legislacao_markdown` | `obter_texto_norma` |
| `obter_texto_legislacao_municipal` | `obter_texto_norma` |
| `pesquisar_legislacao` | `buscar_dispositivos` |
