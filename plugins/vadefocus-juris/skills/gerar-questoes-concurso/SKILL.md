---
name: gerar-questoes-concurso
description: Gera QUESTÕES de concurso jurídico (foco em Ministério Público e Magistratura) ancoradas em fontes REAIS do MCP VadeFocus — objetivas (Cebraspe, FGV/FCC), discursivas, peça/sentença e prova oral — com GABARITO COMENTADO e saída em HTML e PDF. Acione quando o usuário pedir "questões", "simulado", "prova", "caderno", "treino para concurso/MP/magistratura" ou "peça/sentença". Modela a BANCA como parâmetro e NUNCA inventa súmula, tese ou jurisprudência. Não use para apostila nem busca.
allowed-tools: mcp__iajus__buscar_semantica, mcp__plugin_vadefocus-juris_iajus__buscar_semantica, mcp__iajus__buscar_hibrida, mcp__plugin_vadefocus-juris_iajus__buscar_hibrida, mcp__iajus__buscar_fts, mcp__plugin_vadefocus-juris_iajus__buscar_fts, mcp__iajus__buscar_regex, mcp__plugin_vadefocus-juris_iajus__buscar_regex, mcp__iajus__buscar_por_ontologia, mcp__plugin_vadefocus-juris_iajus__buscar_por_ontologia, mcp__iajus__buscar_por_cnj, mcp__plugin_vadefocus-juris_iajus__buscar_por_cnj, mcp__iajus__buscar_qualificada, mcp__plugin_vadefocus-juris_iajus__buscar_qualificada, mcp__iajus__buscar_informativos_stf, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stf, mcp__iajus__buscar_informativos_stj, mcp__plugin_vadefocus-juris_iajus__buscar_informativos_stj, mcp__iajus__obter_unidade_completa, mcp__plugin_vadefocus-juris_iajus__obter_unidade_completa, mcp__iajus__obter_secao_doutrina, mcp__plugin_vadefocus-juris_iajus__obter_secao_doutrina, mcp__iajus__obter_codigo, mcp__plugin_vadefocus-juris_iajus__obter_codigo, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__buscar_dispositivos, mcp__plugin_vadefocus-juris_iajus__buscar_dispositivos, mcp__iajus__obter_dispositivo_legal, mcp__plugin_vadefocus-juris_iajus__obter_dispositivo_legal, mcp__iajus__buscar_norma_fonte_oficial, mcp__plugin_vadefocus-juris_iajus__buscar_norma_fonte_oficial, mcp__iajus__obter_texto_norma, mcp__plugin_vadefocus-juris_iajus__obter_texto_norma, mcp__iajus__obter_ontologia_juridica, mcp__plugin_vadefocus-juris_iajus__obter_ontologia_juridica
---

# Gerar questões de concurso jurídico (VadeFocus)

Gera questões de prática para **Ministério Público** e **Magistratura** (e correlatas),
**ancoradas em fontes reais** do acervo VadeFocus, com **gabarito comentado**, e entrega o
**caderno em HTML + PDF**. Toda questão nasce de uma fonte verificável (lei vigente, súmula,
tese de RG/repetitivo, precedente, doutrina) — **nunca de memória**.

## Quando usar / não usar

- **Use** para: "gere N questões de [disciplina] estilo [banca] para [MP/magistratura]",
  "simulado", "questões discursivas/peça/sentença", "roteiro de prova oral".
- **Não use** para: apostila/material expositivo → **montar-apostila-didatica**; uma consulta
  pontual → a skill de busca direta.

## Parâmetros (pergunte só o que muda a saída)

A **banca é parâmetro de primeira classe** — ela define formato, tipo de âncora e pegadinha:

| Parâmetro | Valores típicos |
|---|---|
| **carreira** | Magistratura (CNJ Res. 75) · Ministério Público (MPE/MPF/MPT/MPM/MPDFT) |
| **banca** | Cebraspe (C/E) · FGV · FCC · VUNESP · CONSULPLAN · IBFC · AOCP |
| **fase / tipo** | objetiva C/E · objetiva múltipla escolha · discursiva · peça/sentença · oral |
| **disciplina / tema** | ramo + sub-tema (use o ramo OJBU p/ recortar a fonte) |
| **UF / órgão** | p/ MPE e magistratura estadual: a UF/tribunal (define lei estadual, LOMP, Constituição estadual aplicáveis) |
| **nível** | fácil (memória) · médio (interpretação) · difícil (caso concreto) |
| **quantidade** | nº de itens |

**Prova-alvo (normalize antes de gerar).** Uma simulação **fiel** depende de:
edital/cargo, **carreira**, **banca**, **UF/tribunal**, fase, programa/conteúdo e materiais
permitidos. Quando o usuário fornecer esses dados, fixe a "prova-alvo" e gere fiel a ela
(inclusive a **legislação estadual/LOMP/Constituição estadual** via **consultar-legislacao-estadual**
para MPE/TJ do estado — hoje o acervo cobre RJ e MG). **Quando faltar edital/prova-alvo, rotule
honestamente a saída como "treino genérico"** (não "simulado do concurso X") e use defaults:
carreira = a citada (senão MP **e** Magistratura); banca = Cebraspe p/ C/E e FGV p/ múltipla;
nível = médio; 10 itens.

A taxonomia completa (estilos por banca, fases, espelho de sentença/peça, formato oral) está
em **`references/tipos-questoes-concursos-juridicos.md`** — consulte-a ao montar.

## Fluxo (orquestração com SUBAGENTES)

1. **Definir o escopo**: carreira + banca + disciplinas + nível + tipos + quantidade.
   Decomponha em disciplinas/temas.
2. **Levantar as ÂNCORAS — em PARALELO, 1 subagente por disciplina/tema** (ver "Pesquisa em
   profundidade" em **pesquisar-jurisprudencia**). Cada item PRECISA de uma âncora real:
   - **lei vigente** (via consultar-legislacao / `obter_codigo` / `obter_texto_norma`),
   - **súmula / SV / tema de RG / repetitivo** (via `buscar_qualificada`),
   - **precedente / informativo** (via pesquisar-jurisprudencia / `consultar_informativos_*`),
   - **doutrina** (via consultar-doutrina) — só para conceito pacífico que *explica*, nunca
     como a âncora que *decide* o gabarito de um item objetivo (ver passo 4).
   Cada subagente devolve no schema fixo `{eixo, consultas, registros:[{tool, id, link_completo,
   teor}], conflitos, lacunas}`. Mantenha um **ledger FONTE-ÂNCORA** (consulta · id ·
   link_completo · teor) e **cada item gerado referencia a entrada do ledger que o sustenta** —
   item sem âncora rastreável nesta sessão **não é gerado**.
3. **Elaborar os itens por tipo** (ver `references/tipos-questoes-concursos-juridicos.md`):
   - **C/E (Cebraspe)**: assertiva binária; o erro mora num deslocamento sutil (troca de
     instituto, termo absoluto, requisito omitido). Tie a regra "uma errada anula uma certa"
     ao edital — **não assuma 1:1**.
   - **Múltipla escolha (FGV/FCC/VUNESP)**: 5 alternativas, 1 correta; distratores plausíveis
     (erro conceitual real, exceção esquecida). Respeite "incompleto ≠ incorreto" da FGV;
     entre duas plausíveis, a "mais completa/correta" vence.
   - **Discursiva**: pergunta aberta + **padrão de resposta** (tese + base legal + jurisprudência).
     Para o **espelho/modelo de resposta** redigido em linguagem jurídica correta, apoie-se na
     skill **redigir-peca-juridica** (registro formal, silogismo, citação CNJ/ABNT).
   - **Peça/Sentença**: enunciado com caso + autos resumidos + **espelho de correção** por
     quesito (relatório / fundamentação ≈55-65% / dispositivo; penal: dosimetria trifásica).
     Peça do MP = candidato atua COMO órgão do MP (denúncia art. 41 CPP, ACP, parecer).
   - **Oral**: ponto + bateria de perguntas de arguição (lei seca + súmulas + teses + caso
     hipotético) com a resposta esperada.
4. **Verificar** cada item: âncora real, vigência, resposta única, distratores legítimos,
   **gabarito comentado completo** — rode o checklist anti-anulação/anti-alucinação abaixo.
   **Item objetivo (C/E ou múltipla escolha): a âncora que DECIDE o gabarito é lei vigente
   ou jurisprudência** (súmula/tese/precedente). Doutrina entra só para contextualizar — não
   sustenta sozinha o gabarito de um objetivo, salvo se o edital/banca pedir expressamente um
   conceito doutrinário (aí explicite que a base é doutrinária e use posição pacífica).
5. **Montar o caderno + gabarito → HTML + PDF** (ver `references/PRODUCAO_HTML_PDF.md`).
   Separe **caderno de questões** (sem respostas) do **gabarito comentado** (com fontes),
   ou um documento com gabarito ao final. Informe os caminhos `.html`/`.pdf`.

## Gabarito comentado (entregar SEMPRE com o item)

Cada questão vem com: **(1)** gabarito; **(2)** fundamento da correta citando a fonte;
**(3)** por que cada distrator erra (qual o erro); **(4)** metadados (banca-estilo,
disciplina, nível, tipo, âncora-fonte). Template em
`references/tipos-questoes-concursos-juridicos.md` › (D.3).

## Contrato de citação (HTML/PDF — preferência do usuário)

No gabarito/fundamento e em qualquer enunciado que cite a fonte:

- **Lei e jurisprudência** → **hyperlink inline** para o `link_completo` (ex.: a súmula/tese
  ou o artigo que ancora o item linkam para a URL oficial).
- **Doutrina** → **sem link inline**; **referência ABNT só no final** (seção Referências).
- Sem `link_completo` real, cite em texto — nunca fabrique URL. Detalhe da marcação:
  `references/PRODUCAO_HTML_PDF.md` › "Citações".

## Validação — anti-anulação + anti-alucinação (rode em TODO item)

- [ ] **Âncora real e citável** (artigo/súmula/tema/precedente conferido — número + diploma/tribunal).
- [ ] **Súmula/Tema/tese EXISTE** de fato — nada inventado (o erro fatal de IA).
- [ ] **Vigência** confirmada (lei não revogada; súmula não cancelada; tese não superada).
- [ ] **Resposta única** (1 correta, ou julgamento C/E inequívoco); nenhuma outra defensável.
- [ ] **Comando claro e fechado**; enunciado traz tudo o que é preciso.
- [ ] **Distratores plausíveis e errados de verdade** (erro identificável, não absurdo).
- [ ] **Sem pegadinha indevida** (incompleto só é errado com termo absoluto ou inversão de sentido).
- [ ] **Estilo da banca** respeitado (C/E vs A–E; idioma de comando; âncora lei seca vs jurisprudência).
- [ ] **Nível** declarado bate com o conteúdo (fácil=memória; médio=interpretação; difícil=caso).
- [ ] **Gabarito comentado completo** (correta fundamentada + cada distrator + fonte).
- [ ] **Peça/sentença**: espelho com quesitos pontuados e pesos coerentes (fundamentação ≈55-65%).
- [ ] **Citação**: lei/jurisprudência com hyperlink; doutrina em ABNT no final; sem URL fabricada.
- [ ] HTML autocontido **e** PDF A4 gerados; diacríticos/UTF-8 corretos.

## Referências

- `references/tipos-questoes-concursos-juridicos.md` — taxonomia SOTA: estilos por banca
  (Cebraspe C/E, FGV/FCC/VUNESP/…), fases × tipo de questão, formato por carreira (Magistratura
  CNJ Res. 75; Ministério Público), regras de qualidade + template de gabarito comentado,
  checklist anti-alucinação. **Consulte ANTES de elaborar.**
- `references/PRODUCAO_HTML_PDF.md` — template HTML autocontido + CSS de marca VadeFocus + PDF
  via WeasyPrint (A4) + a marcação exata das citações (mesmo padrão de montar-apostila-didatica).

> Em 401, repita UMA vez (cold-start); nunca exiba a chave `ik_*`. Respostas grandes: use
> `max_output_kb` e, no Claude Code, `MAX_MCP_OUTPUT_TOKENS`.
