---
name: montar-apostila-didatica
description: Monta APOSTILAS e material de estudo didático sobre um tema jurídico, consolidando MÚLTIPLAS FONTES reais do MCP VadeFocus (jurisprudência + legislação + doutrina) em HTML e PDF prontos para entregar. Acione quando o usuário pedir "apostila", "material de estudo", "resumo didático", "handout" ou "guia de estudo" sobre um tema/ramo, e quiser um documento estruturado. Orquestra as skills de busca (com subagentes) e NUNCA inventa fonte. Não use para gerar questões nem busca pontual.
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_qualificada, mcp__plugin_vadefocus-juris_iajus__buscar_qualificada, mcp__iajus__buscar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stf, mcp__iajus__buscar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stj, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa, mcp__iajus__obter_secao_doutrina, mcp__plugin_vadefocus-juris_iajus__obter_secao_doutrina, mcp__iajus__obter_codigo, mcp__plugin_vadefocus-juris_iajus__obter_codigo, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__buscar_dispositivos, mcp__plugin_vadefocus-juris_iajus__buscar_dispositivos, mcp__iajus__obter_dispositivo_legal, mcp__plugin_vadefocus-juris_iajus__obter_dispositivo_legal, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__obter_ontologia_juridica, mcp__plugin_vadefocus-juris_iajus__obter_ontologia_juridica
---

# Montar apostila didática (VadeFocus)

Produz uma **apostila de estudo** sobre um tema jurídico, consolidando fontes REAIS do
acervo VadeFocus, e entrega o material em **HTML** (autocontido) e **PDF** (A4, pronto para
imprimir). Esta skill ORQUESTRA as skills de busca - não inventa nada: cada afirmação
substantiva vem de uma chamada ao MCP, com a fonte rastreável.

## Quando usar / não usar

- **Use** para: "monte uma apostila sobre X", "material de estudo de Y", "resumo didático
  de Z", "consolidado de jurisprudência + lei + doutrina sobre W", em formato de documento.
- **Não use** para: gerar questões/simulado de concurso → **gerar-questoes-concurso**; uma
  consulta pontual ("o que diz a Súmula 7 do STJ") → a skill de busca direta correspondente.

## Fluxo (orquestração com SUBAGENTES)

Trate a apostila como um **levantamento** seguido de redação. Paralelize a pesquisa.

1. **Definir o escopo + sumário.** Com o tema e o público (concurso? graduação? prática?),
   esboce o sumário (5-9 seções): conceito → fundamento constitucional/legal → requisitos →
   jurisprudência consolidada → divergências → aplicação prática → súmulas/teses. Confirme o
   recorte com o usuário só se for ambíguo.
2. **Pesquisar as fontes - em PARALELO, 1 subagente por eixo** (ver "Pesquisa em
   profundidade" em **pesquisar-jurisprudencia**). Lance numa só leva:
   - **Legislação** (via **consultar-legislacao** / **consultar-legislacao-estadual**): o
     fundamento normativo vigente (artigos âncora), com `link_completo`.
   - **Jurisprudência** (via **pesquisar-jurisprudencia**): precedentes e qualificadas
     (`buscar_qualificada` para súmula/SV/tema), com `link_completo` e ementa.
   - **Doutrina** (via **consultar-doutrina**): o enquadramento conceitual + a `referencia_abnt`.
   Cada subagente devolve um mini-dossiê com um **schema fixo** por achado - `{eixo,
   consultas_feitas, registros_usados:[{tool, id, link_completo, trecho}], conflitos,
   lacunas}` - para você consolidar sem perder procedência. **Dedup** por `link_completo` >
   número CNJ/acórdão > identidade lógica; em **conflito** (precedentes divergentes, súmula
   superada), não escolha em silêncio - registre a divergência na seção própria. Agrupe por
   seção do sumário.
3. **Redigir o conteúdo didático.** Para cada seção: explique o conceito em linguagem clara →
   ancore no dispositivo legal (citado inline com link) → ilustre com o precedente/súmula
   (citado inline com link) → traga o ensinamento doutrinário (sem link; referência ABNT no
   final) → feche com um exemplo/quadro-resumo. Preserve diacríticos/UTF-8. Para a **linguagem
   jurídica** (frases curtas, ordem direta, registro formal, siglas CNJ) e para qualquer **modelo
   de peça/ementa** que a apostila precise exemplificar, aplique a skill **redigir-peca-juridica**.
4. **Gerar HTML + PDF.** Monte o documento autocontido e renderize o PDF - ver
   **`references/PRODUCAO_HTML_PDF.md`** (template, CSS de marca VadeFocus, WeasyPrint, A4).
5. **Validar** com o checklist abaixo antes de entregar; informe os caminhos dos arquivos
   (`.html` e `.pdf`) e as lacunas (o que não foi achado no acervo).

## Qualidade didática (não ser genérico)

Uma apostila SOTA não é um amontoado de ementas. Inclua, conforme o público:

- **Objetivos de aprendizagem** no início de cada seção (o que o leitor deve dominar).
- **Pré-requisitos / conceitos-base** quando a seção pressupõe outro instituto.
- **Quadros comparativos** para institutos que se confundem (ex.: prescrição × decadência;
  IRDR × IAC) e **boxes "não confundir"** com a pegadinha clássica.
- **Progressão** do simples ao complexo; um **exemplo/caso** por seção.
- **Divergências e evolução**: aponte súmula superada, tese pendente, controvérsia viva -
  com as fontes dos dois lados.
- Para público de **concurso**, feche seções com a **súmula/tese** que costuma cair e, se o
  usuário quiser treinar, ofereça acionar **gerar-questoes-concurso** sobre o mesmo tema.

## Contrato de citação (obrigatório - preferência do usuário)

A apostila CITA tudo o que afirma, com esta regra exata por tipo de fonte:

- **Jurisprudência** → **citação inline com HYPERLINK** para o `link_completo` (URL oficial
  do acórdão/súmula). Ex.: `… firmou-se que <a href="LINK">STJ, REsp 1.234.567/SP, Rel.
  Min. Fulano, j. 12/3/2023</a> …`.
- **Legislação** → **citação inline com HYPERLINK** para o `link_completo` (Planalto / fonte
  oficial). Ex.: `… nos termos do <a href="LINK">art. 5º, LVII, da CF</a> …`.
- **Doutrina** → **SEM hyperlink inline** (a doutrina do acervo é protegida por direito
  autoral e não tem URL pública). No corpo, mencione autor/obra em texto comum; **liste a
  `referencia_abnt` completa apenas no final**, na seção "Referências" (ABNT NBR 6023).
- **Só crie hyperlink quando houver link real retornado pela busca.** Sem `link_completo`,
  cite em texto e sinalize a ausência - nunca fabrique URL.

Detalhe da marcação (notas de rodapé numeradas para legislação/jurisprudência + lista de
referências ABNT para doutrina): ver `references/PRODUCAO_HTML_PDF.md` › "Citações".

## Anti-alucinação + procedência (regra dura)

- **Nada de memória.** Todo artigo, número de processo, ementa, súmula, tese ou trecho
  doutrinário citado TEM de ter vindo de uma chamada ao MCP nesta sessão.
- **Ledger de procedência (FONTE-ÂNCORA).** Mantenha, durante a montagem, uma tabela que
  amarra cada afirmação substantiva à sua origem: `{tool, consulta, id_do_registro
  (_id_key/unit_id/numero), link_completo, trecho}`. **Um parágrafo cuja afirmação jurídica
  não casa com um registro do ledger desta sessão é descartado ou reescrito** - não vai para
  a apostila. (O ledger é interno; no documento final aparecem as citações + a seção de links.)
- Se a busca não trouxer fonte para um ponto, **diga que não há cobertura** em vez de
  preencher - uma apostila com lacuna honesta vale mais que uma com fonte inventada.
- Texto de lei = redação **vigente** (confirme; sinalize dispositivo revogado/alterado).
- Súmula/tese: confira `status_vigencia` (cancelada/superada → avise).

## Validação (antes de entregar)

- [ ] Cada seção do sumário tem ao menos uma fonte real citada (lei, precedente ou doutrina).
- [ ] Toda citação de **lei/jurisprudência** é um **hyperlink inline** para o `link_completo`.
- [ ] Toda **doutrina** aparece como referência **ABNT no final** (sem link inline).
- [ ] Nenhum número de lei/súmula/tese/processo foi inventado; vigências conferidas.
- [ ] HTML autocontido (abre sem internet) **e** PDF A4 gerados; caminhos informados.
- [ ] Diacríticos/UTF-8 corretos; sem mojibake.
- [ ] Lacunas de cobertura declaradas ao usuário.

## Referências

- `references/PRODUCAO_HTML_PDF.md` - template HTML autocontido + CSS de marca VadeFocus
  (tokens OKLCH, serifa, A4), conversão para PDF via WeasyPrint, e a marcação exata das
  citações (hyperlink inline p/ lei/jurisprudência; ABNT no final p/ doutrina).

> A chave `ik_*` é injetada pelo cliente no header `Authorization: Bearer` (keychain) -
> **em 401, repita UMA vez** (cold-start); nunca exiba a chave. Respostas muito grandes:
> use o `userConfig` `max_output_kb` e, no Claude Code, `MAX_MCP_OUTPUT_TOKENS`.
