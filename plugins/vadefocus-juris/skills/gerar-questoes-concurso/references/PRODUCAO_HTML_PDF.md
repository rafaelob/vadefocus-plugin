# Produção de HTML + PDF (material VadeFocus)

Como transformar o conteúdo consolidado em um **HTML autocontido** + um **PDF A4**, com a
identidade visual VadeFocus e o contrato de citação do usuário. Mesmo padrão para apostila
(montar-apostila-didatica) e caderno de questões (gerar-questoes-concurso).

## Princípios

- **HTML autocontido**: CSS embutido em `<style>` (sem CDN, sem arquivo externo); o `.html`
  abre offline. Imagens, se houver, embutidas como `data:` URI.
- **PDF determinístico via WeasyPrint** (não usar headless browser): renderiza o MESMO HTML
  para PDF respeitando `@page`/`@media print`. Reexecutar sobre o mesmo conteúdo → PDF estável.
- **A4 com numeração de página** no rodapé; serifa para leitura longa; tokens de cor OKLCH.

## Esqueleto HTML

```html
<!doctype html>
<html lang="pt-BR" data-tema="claro">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{TÍTULO DO MATERIAL} — VadeFocus</title>
<style>/* ↓ ver "CSS de marca" */</style>
</head>
<body>
  <header class="capa">
    <p class="marca">VadeFocus</p>
    <h1>{TÍTULO}</h1>
    <p class="subtitulo">{tema · público · data}</p>
  </header>
  <nav class="sumario"><!-- TOC com âncoras estáveis (#sec-1, #sec-2…) --></nav>
  <main>
    <section id="sec-1"><h2>1. {Seção}</h2> … </section>
    <!-- … demais seções … -->
  </main>
  <section id="links-oficiais" class="links-oficiais">
    <h2>Links oficiais</h2>
    <!-- Lei + jurisprudência: cada nota repete a referência curta + a URL POR EXTENSO
         (clicável no HTML, legível no PDF impresso). Uma nota por citação inline. -->
    <ol>
      <li id="n1">{ref curta da lei/acórdão} — {URL oficial (link_completo) por extenso}</li>
    </ol>
  </section>
  <section id="referencias" class="referencias">
    <h2>Referências</h2>
    <!-- SÓ doutrina entra aqui, em ABNT (NBR 6023). Lei/jurisprudência ficam nos hyperlinks inline. -->
    <ol><li id="ref-1">{referência ABNT da obra: AUTOR. *Título*. seção, p. X.}</li></ol>
  </section>
</body>
</html>
```

## CSS de marca (tokens OKLCH VadeFocus + A4)

```css
:root{
  --bg: oklch(0.985 0.002 75); --fg: oklch(0.145 0.015 75);
  --accent: oklch(0.475 0.12 45);      /* azul VadeFocus */
  --muted: oklch(0.935 0.008 75); --link: oklch(0.475 0.12 45);
}
@media (prefers-color-scheme: dark){
  :root{ --bg: oklch(0.145 0.015 75); --fg: oklch(0.965 0.004 75);
         --accent: oklch(0.65 0.14 45); --muted: oklch(0.27 0.02 75); --link: oklch(0.72 0.12 45);} }
html{ background:var(--bg); color:var(--fg); }
body{ font-family: Georgia, "Times New Roman", serif; line-height:1.6;
      max-width:48rem; margin:0 auto; padding:2rem; }
h1,h2,h3{ color:var(--accent); font-weight:700; }
.marca{ letter-spacing:.18em; text-transform:uppercase; color:var(--accent); font-weight:700; }
a{ color:var(--link); text-decoration:underline; text-underline-offset:2px; }
blockquote{ border-left:3px solid var(--accent); margin:1rem 0; padding:.4rem 1rem;
            background:var(--muted); }
.fonte{ font-size:.85em; color:var(--accent); }           /* rótulo "(STJ, REsp…)" */
sup.nota{ color:var(--accent); font-weight:600; }
table{ border-collapse:collapse; width:100%; } th,td{ border:1px solid var(--accent); padding:.4rem .6rem; }
.referencias li{ margin:.4rem 0; }
/* Impressão / PDF: A4, margens, numeração, sem quebra feia */
@page{ size:A4; margin:25mm 20mm 25mm 30mm;
       @bottom-center{ content: counter(page); color:#666; font-size:9pt; } }
@media print{
  body{ max-width:none; padding:0; }
  h2,h3{ break-after:avoid; } blockquote,table,.qa{ break-inside:avoid; }
  a{ color:inherit; }                 /* link continua visível pelo texto âncora */
  .sumario{ break-after:page; }
}
```

## Citações (o contrato do usuário, exato)

- **Lei e jurisprudência = hyperlink inline** no texto âncora, apontando para `link_completo`,
  **+ nota numerada OBRIGATÓRIA** com a URL por extenso (o PDF impresso perde o clique — a URL
  visível é o que torna a citação verificável no papel):
  ```html
  Conforme o <a href="{link_lei}">art. 927 do Código Civil</a><sup class="nota"><a href="#n1">1</a></sup>, …
  O STJ assentou (<a href="{link_acordao}">REsp 1.234.567/SP, Rel. Min. Fulano, j. 2023</a><sup class="nota"><a href="#n2">2</a></sup>) que …
  ```
  As notas vão para uma seção `#links-oficiais` ("Links oficiais") **antes** das Referências:
  cada `<li id="n1">` repete a referência curta + a **URL por extenso** (clicável no HTML,
  legível no PDF). Toda citação inline de lei/jurisprudência TEM a sua nota correspondente.
- **Doutrina = SEM link inline e SEM nota de URL.** No corpo, cite autor/obra em texto comum
  ("como ensina Fulano, …"); a **referência ABNT** entra **só** na seção `#referencias` no final,
  **montada a partir dos campos que a tool devolve** (`autor`, `obra_titulo`, `section_title`,
  `page_start`) — nenhuma tool do perfil retorna uma string ABNT pronta. A fonte (PDF) não traz
  local/editora/ano: mantenha os marcadores ABNT (`[S. l.]`, `[s. n.]`, `[20--?]`) — não invente.
- **Nunca crie hyperlink sem `link_completo` real.** Sem link, cite em texto e siga. Antes de
  fechar, **verifique**: todo `href` aponta para uma URL `http(s)` real retornada por uma busca
  (zero `#`/placeholder/`example.com`); nenhuma `<a href>` envolve doutrina.

## Gerar o PDF (WeasyPrint)

Depois de escrever `material.html`, renderize o PDF determinístico:

```bash
uv run --with weasyprint python -c "from weasyprint import HTML; HTML('material.html').write_pdf('material.pdf')"
```

- Se `uv` não estiver disponível: `python -m pip install --quiet weasyprint && python -c "from weasyprint import HTML; HTML('material.html').write_pdf('material.pdf')"`.
- **Fallbacks** (se WeasyPrint não instalar no ambiente): a skill `pdf` do host, `pandoc
  material.html -o material.pdf`, ou print-to-PDF via Playwright/Chromium. O HTML acima já
  carrega `@page`/`@media print`, então qualquer um respeita A4 + numeração.
- Entregue SEMPRE os dois artefatos (`.html` e `.pdf`) e diga os caminhos ao usuário.

## Ferramentas do host (não-MCP) + fail-closed

Escrever o `.html` e renderizar o `.pdf` usa as ferramentas do **próprio cliente** (escrita de
arquivo + shell/renderizador) — elas **não** entram no `allowed-tools` da skill (que pré-aprova
só as tools MCP de busca; o `allowed-tools` não bloqueia as ferramentas nativas do host). Se o
host estiver com permissões restritas e **não** puder escrever arquivo nem renderizar PDF,
**não falhe em silêncio**: entregue o conteúdo como HTML inline na resposta, gere o PDF assim
que possível, e diga ao usuário o que faltou para produzir os dois artefatos.

## Acessibilidade e durabilidade do PDF/HTML (checklist)

- [ ] `<html lang="pt-BR">` + `<title>` significativo (vira o título/idioma do PDF).
- [ ] Hierarquia semântica de headings (`h1>h2>h3`) → vira o **outline/bookmarks** do PDF.
- [ ] Imagens (se houver) com `alt`; `<svg>` com `<title>`/`<desc>`.
- [ ] Contraste suficiente (os tokens OKLCH já passam AA no claro e no escuro).
- [ ] **Seção "Links oficiais"** presente com as URLs por extenso (legíveis no papel).
- [ ] Sem overflow horizontal / tabelas estouradas na largura A4 útil (160 mm).
- [ ] Verificação final: extrair os `href` do HTML e conferir que todos resolvem para URLs
      reais retornadas pelas buscas (nenhum placeholder), e que nenhum envolve doutrina.

## Determinismo / qualidade

- Âncoras de seção estáveis (`#sec-N`), TOC gerado a partir dos `<h2>`.
- Sem timestamps voláteis no corpo (a não ser a data pedida na capa).
- Quebras controladas (`break-inside:avoid` em quadros/Q&A); títulos não ficam órfãos no pé.
