---
name: redigir-peca-juridica
description: Redige textos jurídicos em português com técnica e linguagem corretas — petição, contestação, recurso, parecer, sentença/voto e ementa no padrão CNJ — ancorada nas fontes REAIS do MCP VadeFocus (lei + jurisprudência + doutrina), nunca de memória. Acione quando o usuário pedir "redigir", "escrever" ou "revisar" uma peça/parecer/ementa/voto, "linguagem jurídica" ou a metodologia relator/revisor/redator. Aplica estrutura forense e citação ABNT/CNJ. NÃO use para só buscar fonte ou questões.
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__consultar_qualificada, mcp__plugin_vadefocus-juris_iajus__consultar_qualificada, mcp__iajus__consultar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__consultar_informativos_stf, mcp__iajus__consultar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__consultar_informativos_stj, mcp__iajus__consultar_codigo, mcp__plugin_vadefocus-juris_iajus__consultar_codigo, mcp__iajus__consultar_legislacao_federal, mcp__plugin_vadefocus-juris_iajus__consultar_legislacao_federal, mcp__iajus__obter_texto_legislacao, mcp__plugin_vadefocus-juris_iajus__obter_texto_legislacao, mcp__iajus__obter_texto_legislacao_markdown, mcp__plugin_vadefocus-juris_iajus__obter_texto_legislacao_markdown, mcp__iajus__pesquisar_legislacao, mcp__plugin_vadefocus-juris_iajus__pesquisar_legislacao, mcp__iajus__ler_dispositivo_legal, mcp__plugin_vadefocus-juris_iajus__ler_dispositivo_legal, mcp__iajus__consultar_grafo_legislacao, mcp__plugin_vadefocus-juris_iajus__consultar_grafo_legislacao, mcp__iajus__consultar_legislacao_estadual, mcp__plugin_vadefocus-juris_iajus__consultar_legislacao_estadual, mcp__iajus__obter_texto_legislacao_estadual, mcp__plugin_vadefocus-juris_iajus__obter_texto_legislacao_estadual, mcp__iajus__obter_secao_doutrina, mcp__plugin_vadefocus-juris_iajus__obter_secao_doutrina, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa, mcp__iajus__consultar_ontologia_juridica, mcp__plugin_vadefocus-juris_iajus__consultar_ontologia_juridica
---

# Redigir peça jurídica e ementa (VadeFocus)

Produz **texto jurídico** com a técnica, a estrutura e a **linguagem jurídica** corretas —
e **ancorado em fonte real**. Toda regra de direito, dispositivo, súmula, tese, precedente
ou lição doutrinária que entrar no texto **veio de uma chamada ao MCP VadeFocus nesta
sessão** (lei vigente, jurisprudência, doutrina) — **nunca de memória**. A skill orquestra
as skills de busca (`pesquisar-jurisprudencia`, `consultar-legislacao`,
`consultar-legislacao-estadual`, `consultar-doutrina`); não inventa fonte.

## O que esta skill redige

| Gênero | Quem assina / contexto | Estrutura-âncora |
|---|---|---|
| **Ementa de acórdão (padrão CNJ)** | redator do acórdão / estudo de precedente | cabeçalho + I-IV + dispositivos/jurisprudência citados (Rec. CNJ 154/2024) |
| **Petição inicial** | advogado (autor) | endereçamento, qualificação, fatos, fundamentos (direito), pedido, valor da causa, provas |
| **Contestação / resposta** | advogado (réu) | preliminares, mérito (impugnação especificada dos fatos), pedidos |
| **Recurso** (apelação, RE, REsp, agravo) | advogado / parte | cabimento + tempestividade, razões, prequestionamento, pedido de reforma |
| **Parecer / memorial** | parecerista, MP, amicus | relatório, fundamentação, conclusão (parecer); síntese de teses (memorial) |
| **Voto / minuta de decisão** | magistrado / estudo | relatório, fundamentação (art. 489, II, CPC), dispositivo |

Para QUALQUER gênero, a base é a mesma: **pesquisar a fonte real → estruturar no gênero →
escrever em linguagem jurídica → citar corretamente → revisar**. Detalhe de cada gênero
(seções obrigatórias, fórmulas de abertura, requisitos legais) em
`references/GENEROS_REDACAO_JURIDICA.md`. A estrutura completa da **ementa-padrão CNJ** e os
**princípios de linguagem jurídica** estão em `references/EMENTA_CNJ_E_LINGUAGEM.md`.

## Linguagem jurídica (diretrizes — sempre aplicar)

A norma de clareza vem da **LC 95/1998, art. 11** (técnica legislativa) e da **Recomendação
CNJ 154/2024** (ementas), e vale para todo texto forense:

- **Frases curtas, ordem direta** (sujeito - verbo - complemento). Uma ideia por frase.
- **Registro formal e impessoal**, sem coloquialismo, sem 1ª pessoa do singular em peça
  (use "esta defesa", "o autor", "o parecerista"). Pronomes de tratamento corretos:
  **Excelentíssimo Senhor Doutor Juiz** (1º grau), **Egrégio Tribunal / Colenda Câmara /
  Eminente Relator** (2º grau e superiores).
- **Termo técnico padronizado, não floreio.** Diga "Constituição" (não "Carta Magna"/"Lei
  Maior"), "mandado de segurança" (não "mandamus"). Evite adjetivos, advérbios, hipérbole,
  superlativo, estrangeirismo e sinônimo meramente estilístico. A mesma ideia = a mesma
  palavra ao longo do texto.
- **Precisão**: nada de duplo sentido; defina o que for técnico; tempo verbal uniforme
  (presente ou futuro do presente em norma; pretérito nos fatos).
- **Abreviaturas CNJ**: `CF/1988`, `CPC`/`CPC/2015`, `CC`, `CP`, `CPP`, `CDC`, `CLT`, `art.`,
  `inc.`, `§`, `p.u.`; ano com 4 dígitos (`Lei nº 9.099/1995`).
- **Estrutura silogística** na fundamentação: premissa maior (a norma/tese) - premissa menor
  (o fato provado) - conclusão. É o que o art. 489, §1º, do CPC exige de uma decisão.

## Ementa no padrão CNJ (Recomendação 154/2024) — resumo operacional

Estrutura obrigatória (detalhe e exemplos em `references/EMENTA_CNJ_E_LINGUAGEM.md`):

1. **Cabeçalho/indexação** (versalete, até 3-4 linhas): `RAMO DO DIREITO. CLASSE PROCESSUAL.
   ASSUNTO PRINCIPAL. CONCLUSÃO.` Ex.: `DIREITO PROCESSUAL PENAL. HABEAS CORPUS.
   RECONHECIMENTO DE PESSOAS. ORDEM CONCEDIDA.`
2. **I. CASO EM EXAME** — a ação/recurso + fato relevante + pedido (sem "trata-se de"),
   numerado por cardinais.
3. **II. QUESTÃO EM DISCUSSÃO** — "A questão em discussão consiste em saber se (...)" / "Há
   duas questões em discussão: (i) ...; e (ii) ...".
4. **III. RAZÕES DE DECIDIR** — um fundamento por item.
5. **IV. DISPOSITIVO E TESE** — conclusão (provido/desprovido; procedente/improcedente) +
   `Tese de julgamento` entre aspas quando houver.
6. **Dispositivos relevantes citados** e **Jurisprudência relevante citada** ao final (formato
   CNJ: `STF, ADPF 130, Rel. Min. Ayres Britto, Plenário, j. 30.04.2009`).

Regras de redação da ementa: frases curtas, ordem direta, sem citação doutrinária no corpo,
sem adjetivos/estrangeirismos, siglas padronizadas.

## Metodologia relator / revisor / redator (3 passagens)

Adote o ciclo do julgamento colegiado como **método de escrita** — três passagens, mesmo
quando você é o único autor (detalhe em `references/GENEROS_REDACAO_JURIDICA.md`):

1. **Relator (relatar + fundamentar)**: levante os fatos e a fonte real, monte o **relatório**
   (o que se discute) e construa a **fundamentação** (a tese, com lei + precedente + doutrina).
   É a passagem que produz o primeiro texto.
2. **Revisor (revisar)**: confira o texto contra a fonte — cada citação casa com um registro
   do MCP? Vigência conferida? Silogismo fechado? Linguagem dentro das diretrizes? Corrija.
3. **Redator (formalizar o resultado)**: dê a forma final ao gênero (ementa/peça/voto),
   garantindo dispositivo/pedido coerente com a fundamentação. No acórdão real o redator é o
   relator quando vence, ou o autor do **primeiro voto vencedor** quando o relator é vencido —
   por isso o "dispositivo" sempre reflete a **posição que prevaleceu**, não o voto vencido.

## Fluxo (pesquisa real → redação → revisão)

1. **Enquadrar.** Identifique o gênero, o ramo OJBU (`consultar_ontologia_juridica`), o polo
   (autor/réu/julgador/parecerista) e o pedido. Confirme só se ambíguo.
2. **Levantar as fontes — em PARALELO, 1 subagente por perna** (ver "Pesquisa em profundidade"
   em **pesquisar-jurisprudencia**): LEGISLAÇÃO vigente (artigos-âncora + `link_completo`,
   conferindo vigência via `consultar_grafo_legislacao`), JURISPRUDÊNCIA (qualificada por número
   via `consultar_qualificada`; precedentes via busca densa/ontologia; informativos), DOUTRINA
   (conceito + referência ABNT). Cada subagente devolve um mini-dossiê citável; **nada de
   memória**. Mantenha um **ledger de procedência** `{tool, consulta, id, link_completo, trecho}`.
3. **Estruturar** no gênero (ver referência): monte o esqueleto de seções obrigatórias.
4. **Escrever** aplicando as diretrizes de linguagem jurídica e o silogismo na fundamentação.
   Cada afirmação jurídica aponta para uma entrada do ledger.
5. **Revisar** (passagem do revisor) com o checklist abaixo. Só então entregue.

## Anti-alucinação + procedência (regra dura)

- **Nada de memória.** Todo artigo, número de processo, ementa, súmula, tese ou trecho
  doutrinário citado TEM de vir de uma chamada ao MCP nesta sessão. Um parágrafo cuja
  afirmação jurídica não casa com um registro do ledger é **reescrito ou descartado**.
- **Vigência**: lei = redação vigente (sinalize dispositivo revogado/alterado); súmula/tese =
  confira `status_vigencia` (cancelada/superada → avise, nunca cite como vigente).
- **Sem fonte para um ponto?** Diga que **não há cobertura no acervo** — não preencha a lacuna.
  Um texto com lacuna honesta vale mais que um com fonte inventada.
- **Cobertura do acervo**: jurisprudência de STF, STJ, TSE, TRF1-6, os 27 TREs, TJ-RJ (cível) e
  TJ-MG; legislação federal + estadual/municipal RJ e MG; doutrina dos autores ingeridos. Fora
  disso, nomeie a lacuna.

## Citação (obrigatório)

- **Jurisprudência e legislação**: cite com o `link_completo` oficial. Jurisprudência no formato
  CNJ (tribunal, classe, nº, Rel., órgão, data). Legislação por diploma + dispositivo
  (`art. 5º, LVII, da CF/1988`). **Nunca invente** número, redação ou URL.
- **Doutrina**: referência **ABNT (NBR 6023)** ao final — `AUTOR. *Título*. seção, p. X.` —
  **sem hyperlink** (a doutrina do acervo não tem URL pública). Use `[S. l.]`, `[s. n.]`,
  `[20--?]` quando a fonte não trouxer local/editora/ano; não invente esses dados.
- Em peça/voto, a fonte aparece **inline** junto da tese que sustenta; a ementa concentra as
  remissões nas seções finais "Dispositivos/Jurisprudência relevante citada".
- Preserve diacríticos e UTF-8 exatamente como na fonte.

## Checklist de revisão (rode antes de entregar)

- [ ] Gênero correto, com todas as seções obrigatórias (ver referência).
- [ ] Toda afirmação jurídica casa com uma entrada do ledger (fonte real desta sessão).
- [ ] Vigência conferida (lei não revogada; súmula/tese não cancelada/superada).
- [ ] Fundamentação em silogismo (norma - fato - conclusão); art. 489, §1º, CPC, atendido em voto/decisão.
- [ ] Linguagem jurídica: frases curtas, ordem direta, registro formal, siglas CNJ, sem floreio.
- [ ] Ementa (se for o caso): cabeçalho em versalete + I-IV + tese + remissões, no padrão CNJ.
- [ ] Citação: lei/jurisprudência com `link_completo` (formato CNJ); doutrina em ABNT ao final.
- [ ] Pedido/dispositivo coerente com a fundamentação; pronomes de tratamento corretos.
- [ ] Diacríticos/UTF-8 corretos; sem mojibake. Lacunas de cobertura declaradas.

## Referências

- `references/EMENTA_CNJ_E_LINGUAGEM.md` — ementa-padrão CNJ (Recomendação 154/2024) completa
  com exemplos reais, regras de formatação/indexação e as diretrizes de linguagem jurídica
  (LC 95/1998, art. 11). **Consulte ao redigir ou revisar uma ementa.**
- `references/GENEROS_REDACAO_JURIDICA.md` — estrutura por gênero (petição, contestação,
  recurso, parecer, memorial, voto/sentença), fórmulas de endereçamento e tratamento,
  requisitos legais (CPC), e a metodologia relator/revisor/redator detalhada.

> A chave `ik_*` é injetada pelo cliente no header `Authorization: Bearer` — **em 401, repita
> UMA vez** (cold-start); nunca exiba a chave. Respostas grandes: use o `userConfig`
> `max_output_kb` e, no Claude Code, `MAX_MCP_OUTPUT_TOKENS`.
