---
name: pesquisar-direito-integrado
description: "Pesquisa jurídica integrada no MCP VadeFocus: cruza legislação vigente, jurisprudência e doutrina real para responder questão de direito. Use para pesquisa completa, parecer, tese, fundamentação de petição ou decisão, ou base legal e jurisprudencial. Entrega cadeia lei, precedente e doutrina. Não use para uma fonte só, apostila ou questões."
allowed-tools: mcp__iajus__obter_ontologia_juridica, mcp__plugin_vadefocus-juris_iajus__obter_ontologia_juridica, mcp__iajus__buscar_dispositivos, mcp__plugin_vadefocus-juris_iajus__buscar_dispositivos, mcp__iajus__obter_dispositivo_legal, mcp__plugin_vadefocus-juris_iajus__obter_dispositivo_legal, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__obter_grafo_norma, mcp__plugin_vadefocus-juris_iajus__obter_grafo_norma, mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_qualificada, mcp__plugin_vadefocus-juris_iajus__buscar_qualificada, mcp__iajus__buscar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stf, mcp__iajus__buscar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stj, mcp__iajus__obter_secao_doutrina, mcp__plugin_vadefocus-juris_iajus__obter_secao_doutrina, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa
---

# Pesquisa jurídica integrada (lei + jurisprudência + doutrina)

Esta skill responde uma questão de direito amarrando as **três fontes** numa só análise. O valor
não está em consultar cada fonte — as skills `consultar-legislacao`, `pesquisar-jurisprudencia` e
`consultar-doutrina` já fazem isso — está em **cruzá-las**: o dispositivo legal vigente, os
precedentes que o interpretam e a doutrina que o explica/critica, na mesma resposta e com a cadeia
de fundamentação explícita.

## Escopo e honestidade

O acervo VadeFocus cobre legislação federal + estadual/municipal (UFs e municípios ingeridos),
jurisprudência de **STF, STJ, TSE, TRF1-6, os 27 TREs, TJ-RJ e TJ-MG** (2000-2026 — STF controle concentrado desde 1988, TRF6 desde 2022;
TJ-RJ só câmaras cíveis por ora, OJBU/CNJ) e doutrina real (livros e manuais de autores ingeridos). Quando uma das três fontes não
cobre o ponto pedido (ex.: doutrina de um nicho não ingerido, tribunal fora do acervo), **diga
explicitamente qual perna ficou descoberta** — uma pesquisa integrada honesta nomeia as lacunas,
nunca preenche com texto de memória.

## Fluxo de pesquisa integrada

Siga as etapas; pule uma fonte só quando a pergunta de fato não a envolver (e diga que pulou).

1. **Enquadrar o tema.** Identifique o ramo/sub-área OJBU com `obter_ontologia_juridica` (dá o
   `l1_code`/`l2_code` que você reusa para focar as buscas de jurisprudência por ontologia e para
   conferir que está no ramo certo).
2. **LEI primeiro (a base positiva).** `buscar_dispositivos` para achar a(s) norma(s) e o
   dispositivo aplicável; `obter_dispositivo_legal` para o texto do artigo. **Confira a vigência**:
   `obter_grafo_norma` revela se o dispositivo foi alterado/revogado (cite o texto VIGENTE e
   sinalize alteração quando houver). Para a íntegra, `obter_texto_norma`. Anote o
   `link_completo` oficial de cada norma.
3. **JURISPRUDÊNCIA (como os tribunais leem a lei).** Comece pela tese vinculante: se houver
   súmula/SV/tema/RG sobre o ponto, `buscar_qualificada` pelo número, ou descubra-a em
   `buscar_hibrida`. Para o entendimento corrente, `buscar_semantica`/`buscar_hibrida` (focando o
   ramo via `buscar_por_ontologia` com o `l1_code` da etapa 1) e `buscar_informativos_stf`/`_stj`
   para o que saiu recente. Capture, por decisão, órgão, nº, relator, data, trecho da ementa e
   `link_completo`.
4. **DOUTRINA (o porquê e a crítica).** Busque a doutrina com `buscar_semantica`/`buscar_hibrida`
   (a busca cobre a família doutrina) e traga a seção com `obter_secao_doutrina` (use
   `obter_unidade_completa` com o `unit_id` de um hit antes de transcrever trecho longo). A doutrina
   sustenta o conceito, explica a ratio e expõe divergência teórica.
5. **CRUZAR (a etapa que define esta skill).** Amarre as três pernas no MESMO ponto: "o art. X da
   Lei Y [base legal] é interpretado pelo STJ no Tema Z / REsp ... [precedente] no sentido de ...,
   alinhado a (ou criticado por) a doutrina de [autor] [doutrina]". Marque onde há **convergência** e
   onde a doutrina/algum tribunal **diverge** da regra ou de outro tribunal.
6. **SINTETIZAR.** Estruture a resposta nesta ordem:
   - **O que a lei dispõe** — dispositivo(s) vigente(s), citados.
   - **Como os tribunais interpretam** — tese vinculante (se houver) + precedentes, com divergências.
   - **O que a doutrina explica** — conceito, fundamento e crítica.
   - **Síntese** — a posição integrada (lei + jurisprudência + doutrina) e, ao final, as **lacunas**
     (fonte que o acervo não cobriu, ponto sem precedente, divergência ainda aberta).

> Quando o produto final desta pesquisa for uma **peça, parecer, memorial, voto ou ementa**
> (não só a resposta integrada), continue na skill **redigir-peca-juridica**: ela transforma a
> cadeia lei-precedente-doutrina já levantada em texto forense com a estrutura, a linguagem
> jurídica e a citação corretas (padrão CNJ / ABNT).

## Paralelize as três pernas com SUBAGENTES

Uma pesquisa integrada robusta toca muitas buscas. NÃO faça tudo numa thread só: **delegue uma
perna por subagente** (um varre a LEGISLAÇÃO + vigência, outro a JURISPRUDÊNCIA — qualificadas e
busca densa do ramo —, outro a DOUTRINA), lançados **na mesma leva** (em paralelo), e só então
cruze e sintetize você. Dê a cada subagente: o recorte exato, as tools desta família que pode usar
(as deste plugin) e o formato de retorno (dossiê citável: fonte, identificação, trecho e
`link_completo`/referência — **nada de memória**).

- **Claude / Claude Code:** subagentes via a ferramenta de tarefas (Task/Agent), lançados na mesma
  mensagem para correrem em paralelo.
- **ChatGPT / Codex e afins:** dispare as buscas independentes em lote (várias tool-calls na mesma
  rodada) ou sub-tarefas paralelas, se o cliente suportar.

## Regras de citação (obrigatório)

- **Jurisprudência e legislação:** cada uma com o seu **`link_completo`** oficial ao lado da fonte
  (lado a lado, não num bloco solto no fim). Inclua, na jurisprudência, tribunal, nº do processo,
  relator e data; na legislação, a norma, o artigo e a **vigência** (sinalize se alterado/revogado).
- **Doutrina:** referência no padrão **ABNT** (autor, obra, edição, página) ao final, **nunca**
  hyperlink inline — doutrina não tem URL pública oficial.
- **Cadeia explícita:** ao afirmar a tese integrada, deixe visível qual lei, qual precedente e qual
  doutrina a sustentam (os marcadores [L], [J], [D] ou citação direta servem).
- Preserve diacríticos e UTF-8 exatamente como na fonte; nunca invente norma, acórdão, ementa,
  link ou página de doutrina. Se uma perna não tiver resultado, diga — não compense com memória.

## Tratamento de erros

- **Timeout numa busca** (`{"erro": "...tempo limite..."}`): restrinja escopo (ramo, ano, `k`
  menor) e repita uma vez; não troque a fonte por memória.
- **`total: 0` numa das pernas:** registre a perna como lacuna e siga com as outras duas; relate a
  lacuna na síntese.
- **401 na primeira chamada:** repita UMA vez (validação de chave no cold-start). Persistindo,
  oriente a revisar a chave configurada — nunca exiba a chave.
- **Vigência ambígua** (`obter_grafo_norma` sem dado): cite o texto como está e avise que a
  vigência do dispositivo não pôde ser confirmada pelo grafo — não afirme vigência que não verificou.
