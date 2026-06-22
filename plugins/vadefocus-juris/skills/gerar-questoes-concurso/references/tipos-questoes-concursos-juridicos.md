# Tipos de questões em concursos jurídicos BR (referência SOTA)

> Referência densa para gerar questões de prática de alto nível para concursos jurídicos brasileiros — Magistratura (juiz estadual/federal, CNJ Res. 75/2009) e Ministério Público (MPE/MPF/MPT/MPM/MPDFT). Cobre estilos das bancas, fases/tipos de questão, formatos por carreira, regras de qualidade e checklist anti-alucinação. Termos em PT-BR autêntico. Fontes inline.

> ⚠️ **FIREWALL — este arquivo é METODOLOGIA/ESTILO, NÃO é fonte jurídica.** Use-o para
> moldar o FORMATO da questão (estilo da banca, tipo, espelho, calibração). **NUNCA** trate
> os fatos jurídicos, números de súmula/tese/artigo ou as URLs aqui contidos como âncora de
> uma questão. **Toda âncora jurídica de um item gerado tem de vir de uma chamada ao MCP
> VadeFocus NESTA sessão** (lei vigente, súmula, tese, precedente, doutrina retornados pelas
> tools). As URLs abaixo são da pesquisa de elaboração desta referência — não as cite nas
> questões nem as use como prova de vigência.

---

## (A) BANCAS × ESTILO OBJETIVO

Cada banca tem uma "assinatura" de elaboração. Para gerar itens fiéis, replique o *idioma de comando* e o *padrão de pegadinha* da banca-alvo.

### A.1 CEBRASPE / CESPE — formato "Certo/Errado" (C/E) com fator de correção

- **Formato**: cada **item** é uma assertiva isolada que o candidato julga **C (Certo)** ou **E (Errado)**. Não há alternativas A–E; o item *é* a afirmação. Uma "questão" do edital costuma se desdobrar em vários itens numerados.
- **Regra "uma errada anula uma certa"**: o Cebraspe é pioneiro no padrão C/E **com fator de correção** — cada item errado **subtrai** um item certo (nota = acertos − erros), para desencorajar o "chute". **Atenção (variação recente)**: o critério **não é universal** — o edital define se haverá apenação negativa, qual o fator (1:1 ou variações em que só se perde ponto ao errar dois itens) e o peso por disciplina. Alguns certames recentes **descartaram** a apenação. Sempre ancore a regra ao edital, não assuma 1:1 cegamente. ([Estratégia — método Cebraspe](https://www.estrategiaconcursos.com.br/blog/metodo-cebraspe-cespe-avaliacao/); [Correio Braziliense](https://blogs.correiobraziliense.com.br/papodeconcurseiro/so-o-cebraspe-aplica-o-criterio-uma-errada-anula-uma-certa-em-concursos/); [Jusbrasil](https://www.jusbrasil.com.br/noticias/voce-sabia-que-o-cebraspe-nem-sempre-aplica-o-temido-criterio-uma-errada-anula-uma-certa/629399991))
- **Como o item é escrito**: afirmação fechada, derivada de **letra da lei**, **súmula** ou **jurisprudência** consolidada, frequentemente com um *deslocamento* sutil que a torna falsa. Um item C/E é binário: basta **um erro** em qualquer parte da assertiva para o todo ser **E**.
- **Pegadinhas clássicas (replicar para gerar distrações boas)** ([Estratégia — pegadinhas](https://www.estrategiaconcursos.com.br/blog/pegadinhas-de-concurso-saiba-quais-sao-as-10-maiores/); [Academia do Raciocínio](https://concursos.academiadoraciocinio.com/2025/10/22/o-algoritmo-secreto-do-cespe-padroes-que-se-repetem-ha-10-anos-e-ninguem-percebeu/)):
  1. **Generalização/restrição indevida**: termos totalizantes — *sempre, nunca, todos, nenhum, somente, exclusivamente, em qualquer hipótese*. Como o Direito é cheio de exceções, esses termos quase sempre tornam o item **E**.
  2. **Troca de conceitos parecidos**: *prescrição × decadência*, *dolo × culpa*, *eficácia contida × eficácia limitada*, *deportar × extraditar × expulsar*, *distinguishing × overruling*.
  3. **Supressão de requisito/exceção**: descreve a regra corretamente mas **omite** elemento essencial ou exceção — tornando o item incompleto ao ponto de ficar errado.
  4. **Inversão de competência/prazo/sujeito**: troca o órgão competente, o prazo, o legitimado.
  5. **Termos polissêmicos** e reescritura que altera o sentido jurídico.
- **Gabarito + justificativa**: gabarito **preliminar → definitivo** (após recursos). Itens podem ser **anulados** (todos pontuam) por ambiguidade, conteúdo fora do edital ou dupla resposta defensável. O Cebraspe não publica justificativa item-a-item por padrão, mas o STJ vem exigindo **espelho/padrão de resposta** em discursivas. ([STJ — intervenção em concursos](https://www.stj.jus.br/sites/portalp/Paginas/Comunicacao/Noticias/13022022-Questao-de-prova-ate-onde-a-Justica-pode-intervir-nos-criterios-da-banca-de-concurso-publico.aspx))

### A.2 FGV — múltipla escolha (A–E), doutrina "incompleta × incorreta", caso concreto

- **Formato**: múltipla escolha, **cinco alternativas (A–E)**, **uma correta**. Enunciados **longos, contextualizados, caso-baseados** (descrevem uma situação fática e pedem a solução jurídica), com forte uso de **jurisprudência, súmulas e teses** atuais, além da lei. ([Estratégia — FGV](https://www.estrategiaconcursos.com.br/blog/fgv/); [Nova Concursos — FGV](https://www.novaconcursos.com.br/portal/artigos/como-gabaritar-provas-banca-fgv-analise-completa/))
- **Doutrina "incompleto não é incorreto"**: princípio interpretativo recorrente da FGV — uma alternativa que enuncia uma **regra geral correta sem esgotar todas as exceções** *não fica automaticamente errada* só por ser incompleta. Marcar como errada uma assertiva incompleta (mas verdadeira) é tratada como "pegadinha" indevida em sede de recurso; a própria banca, no padrão de resposta, costuma reconhecer que incompleto ≠ incorreto. ([análise de recursos FGV](https://olivaesouza.com.br/concurso-do-mpu-soberba-da-fgv-da-as-caras-mais-uma-vez-e-questoes-mal-formuladas-prejudicam-candidatos-mesmo-apos-gabarito-definitivo/))
- **"Assertiva mais correta / mais completa"**: quando **duas** alternativas são plausíveis, a resposta é a **mais completa e precisa** — a que melhor responde ao **comando**. Entre uma verdade parcial e uma verdade que abrange mais elementos pertinentes, vence a mais abrangente. ([Próximos Concursos — múltipla escolha](https://www.proximosconcursos.com/questoes-de-multipla-escolha/))
- **Sinais de pegadinha FGV**: termos *nunca, apenas, exceto, salvo*; enunciados que escondem o comando real no meio do texto; alternativas que misturam dois institutos. ([Próximos Concursos — FGV](https://www.proximosconcursos.com/banca-fgv/))
- **Itens combinatórios**: também usa o estilo "I, II, III" (avalie quais afirmativas estão corretas) e "assinale a (in)correta".

### A.3 FCC, VUNESP, CONSULPLAN, IADES, IBFC, AOCP — idiomas de múltipla escolha

Características por banca ([Gran — perfis de bancas](https://blog.grancursosonline.com.br/perfil-bancas-de-concursos-publicos/); [Gran — FCC e VUNESP](https://blog.grancursosonline.com.br/conhecendo-as-principais-bancas-examinadoras-fcc-e-vunesp/); [Decorando a Lei Seca — Consulplan](https://blog.decorandoaleiseca.com.br/perfil-banca-consulplan/); [Próximos Concursos — IBFC](https://www.proximosconcursos.com/banca-ibfc-saiba-tudo/); [Direção — AOCP](https://www.direcaoconcursos.com.br/noticias/concurso-instituto-aocp)):

| Banca | Formato típico | Ênfase de conteúdo | Idioma de comando frequente |
|---|---|---|---|
| **FCC** | Múltipla escolha, 5 alternativas, 1 correta | Historicamente **lei seca**; passou a cobrar **jurisprudência** com mais frequência | "Assinale a alternativa correta/incorreta"; troca cirúrgica de palavra no texto legal |
| **VUNESP** | Múltipla escolha, enunciados longos com textos/casos | Forte **lei seca**, pouca doutrina | Pegadinha por **enunciado truncado** e troca de palavra |
| **CONSULPLAN** | Múltipla escolha | **Lei seca** quase pura, pouca doutrina/jurisprudência | "Assinale a alternativa correta"; itens diretos |
| **IADES** | Múltipla escolha, **5 opções**, textos longos/cansativos | Comparável a FGV/Cebraspe em complexidade | Enunciados extensos, exige concentração |
| **IBFC** | Múltipla escolha, **4 ou 5 alternativas**, mais direta | **Lei seca**, poucas pegadinhas | Pede com frequência a **alternativa "INCORRETA"** (até ~22% da prova) |
| **AOCP / Instituto AOCP** | Múltipla escolha (4–5 alt.); às vezes **C/E estilo Cebraspe** com apenação | **Lei seca**, enunciados diretos | "Assinale a correta/incorreta"; baixa ambiguidade |

**Idiomas combinatórios comuns a essas bancas** (replicar literalmente):
- *"Assinale a alternativa correta."* / *"Assinale a alternativa INCORRETA."*
- *"Considere as afirmativas I, II e III. Está(ão) correta(s):"* → alternativas combinam (apenas I; I e II; II e III; todas; nenhuma).
- *"Analise as assertivas e marque V (verdadeiro) ou F (falso). A sequência correta é:"*
- *"Complete a lacuna conforme a [lei / súmula / CF]."* (preenchimento literal de dispositivo).
- *"Relacione a coluna I com a coluna II."* (associação instituto ↔ definição).

### A.4 Qualidade na construção do item (todas as bancas)

Estrutura canônica de um item de múltipla escolha ([UFMG — Guia de elaboração de itens](https://www.nescon.medicina.ufmg.br/biblioteca/imagem/Guia-elaboracao-itens-avaliacao.pdf)):
- **Enunciado (suporte/texto-base)**: contexto + comando claro. Deve conter **tudo** o que é preciso para responder; o erro nunca pode vir de o candidato *não entender o que se pede*.
- **Comando**: instrução precisa e única ("assinale a correta", "é INCORRETO afirmar", "está de acordo com a súmula").
- **Gabarito (chave)**: a alternativa correta, inequívoca.
- **Distratores**: alternativas erradas **plausíveis** — refletem **raciocínios possíveis** de quem não domina o conteúdo (erro conceitual real, troca de instituto, exceção esquecida). Distrator bom = "armadilha cognitiva legítima", não absurdo descartável de cara.
- **Letra da lei × jurisprudência × doutrina**:
  - *Lei seca* → ancore em **artigo vigente** (cite número e diploma).
  - *Jurisprudência* → ancore em **súmula** (STF/STJ/TST), **tese de repercussão geral** (Tema X do STF), **tema de recurso repetitivo** (Tema Y do STJ), **súmula vinculante**, **IRDR/IAC/IRR**, ou **informativo** recente.
  - *Doutrina* → use para classificações e conceitos consolidados, evitando posição minoritária como gabarito.

---

## (B) FASES × TIPO DE QUESTÃO (ciclo do certame)

### B.1 1ª fase — OBJETIVA (eliminatória/classificatória)

- **C/E (Cebraspe)**: julgamento item a item; possível apenação negativa (ver A.1).
- **Múltipla escolha (FGV/FCC/VUNESP/…)**: A–E (ou A–D), uma correta; combinatórias I/II/III; V/F.
- **Na Magistratura (CNJ Res. 75/2009)**: a **prova objetiva seletiva** é organizada em **blocos por disciplina**. Corte: **acerto mínimo de 30% das questões em cada bloco e média final de 60%** (Art. 43). Blocos típicos (Justiça Estadual, Anexo): Bloco I — Civil, Processo Civil, Consumidor, ECA; Bloco II — Penal, Processo Penal, Constitucional, Eleitoral; Bloco III — Empresarial, Tributário, Ambiental, Administrativo, Direitos Humanos e noções gerais. ([CNJ Res. 75/2009 — íntegra](https://atos.cnj.jus.br/atos/detalhar/100); [PDF oficial](https://www.cnj.jus.br/wp-content/uploads/2009/07/rescnj_75.pdf))
- **No MP (ex.: padrão CESPE/FGV)**: prova objetiva costuma ter ~**120 questões**, ~5h, 4–5 alternativas, uma correta, eliminatória e classificatória. ([CESPE MP-CE](https://cdn.cebraspe.org.br/concursos/mp_ce_19_promotor/arquivos/MP_CE_19_PROMOTOR_PROVAS_DISCURSIVAS.PDF); [MPDFT Res. 271](https://www.mpdft.mp.br/portal/pdf/unidades/conselho_superior/resolucoes_vigor/Resolucao_271_Concurso_MPDFT.pdf))

### B.2 2ª fase — DISCURSIVA / ESCRITA (eliminatória)

Tipos de questão discursiva:
- **Questão discursiva / resposta fundamentada**: pergunta aberta sobre ponto do programa; exige **resposta jurídica fundamentada** (tese + base legal + jurisprudência), em espaço limitado de linhas. Na Magistratura, é a **prova escrita discursiva** sobre "quaisquer pontos do programa específico" (Art. 47, Res. 75). ([CNJ Res. 75](https://atos.cnj.jus.br/atos/detalhar/100))
- **Dissertação jurídica**: texto dissertativo-argumentativo sobre tema jurídico, com tese, desenvolvimento e conclusão.
- **Parecer**: peça opinativa (no MP, **parecer ministerial** como *custos legis* — ex.: parecer em mandado de segurança, em apelação).
- **No MP — provas discursivas**: tipicamente **3 provas discursivas**; consulta a **legislação seca** geralmente permitida (vedada doutrina/súmula/anotação). Uma parte vale ~40 pontos e exige **peça** (instauração de ação cível ou penal, parecer, recurso ou peça aplicável a procedimento). ([CESPE MP-CE](https://cdn.cebraspe.org.br/concursos/mp_ce_19_promotor/arquivos/MP_CE_19_PROMOTOR_PROVAS_DISCURSIVAS.PDF))

### B.3 PEÇA PROCESSUAL / PRÁTICA

- **Magistratura — SENTENÇA**: ver B.4.
- **MP — peças típicas** (2ª fase prática) ([G7 Jurídico — Prática MPE](https://www.g7juridico.com.br/curso-pratica-mpe); [MPSP 96º concurso](https://arquivos.qconcursos.com/prova/arquivo_prova/129489/mpe-sp-2025-mpe-sp-promotor-de-justica-prova.pdf)):
  - **Denúncia** (peça acusatória penal — crimes patrimoniais, violência doméstica, etc.): requisitos do art. 41 do CPP — qualificação, exposição do fato criminoso com todas as circunstâncias, classificação e rol de testemunhas.
  - **Ação Civil Pública (ACP)**: petição inicial em ACP ambiental, de improbidade administrativa, consumerista — legitimidade do MP, objeto, pedido, tutela coletiva.
  - **Parecer ministerial** (MP como *custos legis* / fiscal da ordem jurídica): ex. parecer em mandado de segurança, em recurso.
  - **Recurso / contrarrazões**, **peças de procedimento** (TAC, recomendação, inquérito civil).

### B.4 SENTENÇA (Magistratura) — caso + autos resumidos + espelho

- **Estrutura da prova**: apresenta-se um **"caso"** com **autos resumidos** (relatório dos fatos, pedidos das partes, provas) e o candidato redige a **sentença completa**: relatório → fundamentação → dispositivo.
- **Quantidade (Res. 75, Art. 49)**: Justiça Federal/Estadual = **duas sentenças** (uma **cível** e uma **criminal**, em dias sucessivos); Justiça do Trabalho = **uma** sentença trabalhista; Justiça Militar = **uma** sentença criminal. **Nota mínima 6** em cada (Art. 54, parágrafo único). ([CNJ Res. 75](https://atos.cnj.jus.br/atos/detalhar/100))
- **Espelho de correção — o que premia** (pesos típicos) ([Decorando a Lei Seca — sentença](https://blog.decorandoaleiseca.com.br/prova-de-sentenca-concurso/); [FGV — espelho prova de sentença](https://conhecimento.fgv.br/sites/default/files/concursos/espelho-de-correcao-provas-praticas-de-sentenca_v.f.pdf); [FGV — espelho sentença penal](https://conhecimento.fgv.br/sites/default/files/concursos/espelho_sentenca_penal.pdf)):

  | Bloco | Peso típico | O que o examinador avalia |
  |---|---|---|
  | **Relatório** | ~5–10% | Síntese fiel das fases, pedidos e provas; primeiro "filtro de credibilidade" |
  | **Fundamentação** | **~55–65%** (bloco mais pesado) | Enfrentamento de **todas** as questões relevantes (prejudiciais, mérito, teses das partes); pertinência, clareza e lógica do raciocínio; correta aplicação de lei + jurisprudência |
  | **Dispositivo** | ~15–20% | Conclusão coerente com a fundamentação; procedência/improcedência; condenação, custas, honorários; (penal) **dosimetria da pena** trifásica |
  | **Técnica / redação / português** | restante | Estrutura, vernáculo, clareza; desconto por erros de português |

  - **Cada quesito recebe pontuação independente** — o gabarito da sentença é **analítico** (lista de pontos que *devem* ser enfrentados, cada um valendo X). Na **sentença penal**, a fundamentação se decompõe em **materialidade, autoria, qualificadoras/causas, dosimetria (trifásica: pena-base → agravantes/atenuantes → causas de aumento/diminuição), regime, substituição/sursis** e o dispositivo.
  - **STJ**: vem exigindo **divulgação de espelho/padrão de resposta** em provas escritas (dissertativas e de sentença) para garantir transparência e direito a recurso; já decidiu que **prova oral pode dispensar espelho**. ([STJ](https://www.stj.jus.br/sites/portalp/Paginas/Comunicacao/Noticias/13022022-Questao-de-prova-ate-onde-a-Justica-pode-intervir-nos-criterios-da-banca-de-concurso-publico.aspx); [Magistrar — espelho prova oral](https://magistrarcursos.com.br/blog/stj-decide-que-ausencia-de-espelho-de-correcao-em-prova-oral-de-concurso-para-magistratura-nao-e-ilegal/))

### B.5 PROVA ORAL / arguição (eliminatória)

- **Formato (Res. 75)**: sessão **pública**, perante toda a Comissão Examinadora (Art. 64). **Ponto sorteado** por candidato com **24h de antecedência** (Art. 65, §2º); o **programa específico** é divulgado até **5 dias** antes. A arguição cobre as **disciplinas da prova escrita**, agrupadas em pontos pela Comissão. Cada examinador dá nota **0–10**, média aritmética; **mínimo 6** para aprovar (Art. 65, §§). Tempo de arguição até ~15 min por examinador. ([CNJ Res. 75](https://atos.cnj.jus.br/atos/detalhar/100); [TRT2 — pontos prova oral](https://www.trt2.jus.br/images/Institucional/concursos/magistrados/XL/pontos_concurso_publicacao_procedimentos.pdf))
- **Estilo "ponto a ponto" / disciplina a disciplina**: o examinador percorre o ponto sorteado e formula perguntas sobre **conhecimento técnico** — domínio do **conhecimento jurídico**, adequação da linguagem, articulação do raciocínio, capacidade de argumentação e uso correto do vernáculo.
- **O que o examinador sonda** (gerar perguntas orais nesse molde): **lei seca + súmulas + teses + informativos recentes**; conceitos e classificações; divergências doutrinárias e jurisprudenciais; aplicação a hipótese hipotética que o examinador inventa na hora.
- **No MP**: formato análogo (ponto sorteado por disciplina, arguição perante banca). ([Estratégia — oral MP-SP](https://cj.estrategia.com/portal/como-funciona-a-prova-oral-do-mp-sp-promotor/))

### B.6 TÍTULOS (classificatória, pontuação reduzida)

- Etapa **apenas classificatória**, pontuação-teto baixa. Na Magistratura (Res. 75, Art. 67), pontuam: exercício de **magistratura/MP/cargos jurídicos**, **magistério jurídico**, **advocacia**, **pós-graduação** (especialização, mestrado, doutorado), aprovação em outros concursos jurídicos, publicações. Cada título tem teto objetivo (0 a X pontos). **Não compõe nota de corte eliminatória** — só desempata/classifica. ([CNJ Res. 75](https://atos.cnj.jus.br/atos/detalhar/100))

---

## (C) FORMATO POR CARREIRA

### C.1 MAGISTRATURA — CNJ Resolução nº 75/2009

Concurso público de **provas e títulos**, cargo inicial **juiz substituto** (CF arts. 93, I e 96, I, "c"). Etapas (Res. 75) ([íntegra](https://atos.cnj.jus.br/atos/detalhar/100); [PDF](https://www.cnj.jus.br/wp-content/uploads/2009/07/rescnj_75.pdf)):

1. **1ª etapa — Prova Objetiva Seletiva**: múltipla escolha por **blocos**; corte 30%/bloco + 60% média (Art. 43); eliminatória/classificatória.
2. **2ª etapa — Provas Escritas**: (a) **discursiva** sobre pontos do programa (Art. 47); (b) **prática de sentença** — 2 sentenças (cível + criminal) na Justiça Comum (Art. 49); mínimo 6 em cada (Art. 54).
3. **3ª etapa — Sindicância da vida pregressa e investigação social** + exames de sanidade física/mental + documentação (Art. 58).
4. **4ª etapa — Prova Oral** (Art. 64–65): ponto sorteado 24h antes; mínimo 6.
5. **5ª etapa — Avaliação de Títulos** (Art. 66–69): classificatória.

**Pesos finais (Art. 7º)**: objetiva = 1; cada prova escrita = 3; oral = 2; títulos = 1.

- **ENAM (atualização)**: o CNJ passou a admitir que o **Exame Nacional da Magistratura (ENAM)** possa substituir/anteceder a 1ª fase em certames dos tribunais. Verificar edital atual. ([CNJ — ENAM](https://www.cnj.jus.br/cnj-aprova-regras-para-instituicao-do-exame-nacional-para-a-magistratura/); [Estratégia — ENAM](https://cj.estrategia.com/portal/cnj-aprova-enam-substituir-primeira-fase-dos-concursos-magistratura/))
- **Juiz Federal**: regido pela Res. 75 + normativo do CJF (atualizado 2024). ([CJF](https://www.cjf.jus.br/cjf/noticias/2024/fevereiro/cjf-aprova-atualizacao-do-normativo-que-dispoe-sobre-concurso-publico-de-juiz-federal-substituto))
- **Disciplinas-núcleo**: Constitucional, Administrativo, Civil, Processo Civil, Penal, Processo Penal, Empresarial, Tributário, Ambiental, Consumidor, ECA, Direitos Humanos, Filosofia/Sociologia do Direito; **Trabalho/Processo do Trabalho** (TRTs), **Eleitoral** (Justiça Eleitoral), **Militar** (Justiça Militar).

### C.2 MINISTÉRIO PÚBLICO — MPE / MPF / MPT / MPM / MPDFT

Cada ramo tem regulamento próprio (resolução do CNMP/Conselho Superior), mas o **arco de etapas é uniforme**: objetiva → discursiva (com peças) → oral → títulos, todas eliminatórias e classificatórias. ([MPDFT Res. 271](https://www.mpdft.mp.br/portal/pdf/unidades/conselho_superior/resolucoes_vigor/Resolucao_271_Concurso_MPDFT.pdf); [MPF — provas anteriores](https://www.mpf.mp.br/regiao3/estagie-conosco/provas-e-gabaritos-anteriores))

- **MPE (Promotor de Justiça)**: 1ª fase objetiva (~120 q.); 2ª fase **discursiva/prática** com **peças** (denúncia, ACP, parecer, recurso) + questões dissertativas; **prova oral**; títulos. Inclui disciplina específica **Legislação Institucional do MP** (LONMP — Lei 8.625/93, LC 75/93 no MPU). ([MPSP](https://arquivos.qconcursos.com/prova/arquivo_prova/129489/mpe-sp-2025-mpe-sp-promotor-de-justica-prova.pdf))
- **MPF (Procurador da República)**: objetiva + **subjetiva/discursiva** por grupos de disciplinas + **oral** + títulos. Ênfase em Constitucional, Penal, Processo, Coletivo, Direitos Humanos, Internacional. ([MPF/PRR3](https://www.mpf.mp.br/regiao3/estagie-conosco/provas-e-gabaritos-anteriores))
- **MPT (Procurador do Trabalho)**: foco em Direito do Trabalho, Processo do Trabalho, Coletivo, Constitucional; peças trabalhistas (ACP trabalhista, parecer).
- **MPM (Procurador da Justiça Militar)** e **MPDFT**: estrutura análoga; MPM com Direito Penal/Processual Militar.
- **Peça do MP** = o equivalente da "sentença" da magistratura: o candidato **atua como órgão do MP** (acusação penal, autor de ação coletiva, ou *custos legis*), não como julgador. O espelho premia: **endereçamento correto, legitimidade/cabimento, fatos e fundamentação jurídica, pedido/promoção adequados, técnica e português.**

---

## (D) REGRAS DE QUALIDADE — ITENS ANCORADOS EM FONTE VERIFICÁVEL + GABARITO COMENTADO

### D.1 Âncora obrigatória (anti-alucinação por design)

**Todo item deve nascer de uma fonte verificável e citável.** Defina a âncora ANTES de escrever a assertiva:

| Tipo de âncora | Como citar | Verificação |
|---|---|---|
| **Lei vigente** | "CF, art. 5º, LVII"; "CPC/2015, art. 489, §1º"; "CP, art. 121, §2º" | Conferir número, diploma e **vigência atual** (norma não revogada) |
| **Súmula** | "Súmula 7/STJ"; "Súmula Vinculante 11"; "Súmula 331/TST" | Conferir número, tribunal, **teor literal** e se não foi **cancelada/superada** |
| **Tese de Repercussão Geral** | "STF, Tema 1234, tese fixada: '…'" | Conferir número do tema + tese transitada |
| **Recurso Repetitivo** | "STJ, Tema 999 (repetitivo), tese: '…'" | Idem |
| **IRDR / IAC / IRR** | "TJ/TRF, IRDR nº …, tese: '…'" | Conferir vinculação e vigência |
| **Doutrina** | autor + obra (apenas para conceitos pacíficos) | **Nunca** usar posição minoritária como gabarito |

- Prefira **letra da lei e jurisprudência consolidada** ao gabarito; reserve doutrina para classificações pacíficas.
- Para banca **lei-seca-cêntrica** (FCC/VUNESP/CONSULPLAN/IBFC), ancore preferencialmente em **dispositivo literal**.
- Para banca **jurisprudência-cêntrica** (FGV; Cebraspe avançado; MP/Magistratura), ancore em **súmula/tese/informativo**.

### D.2 Calibração de dificuldade (Bloom adaptado ao Direito)

| Nível | Cognição (Bloom jurídico) | Como o item se apresenta |
|---|---|---|
| **Fácil** | **Memória da lei/súmula** | Reprodução literal de dispositivo ou súmula; troca óbvia |
| **Médio** | **Compreensão / interpretação** | Reformulação do dispositivo; distinguir institutos; aplicar exceção; combinatória I/II/III |
| **Difícil** | **Aplicação a caso concreto / síntese** | Caso fático longo (estilo FGV); conflito de teses; aplicar jurisprudência a fato novo; pegar a exceção da exceção; "assertiva mais correta" entre duas plausíveis |

Distribua a dificuldade pelos distratores: itens difíceis têm distratores que são **erros sofisticados** (jurisprudência superada, tese minoritária travestida de majoritária, requisito sutilmente omitido).

### D.3 Padrão "gabarito comentado" (entregar SEMPRE junto com o item)

Cada item gerado deve vir acompanhado de:

1. **Gabarito** (letra correta, ou C/E).
2. **Fundamentação da correta**: por que está certa, **citando a fonte** (artigo/súmula/tese) e, se útil, o teor literal.
3. **Por que cada distrator está errado**: justificativa item a item — *qual* o erro (conceito trocado, exceção omitida, prazo/competência incorretos, jurisprudência superada). É o que faz o estudante "registrar o padrão do erro", não só a resposta certa. ([Decorando a Lei Seca — questões comentadas](https://blog.decorandoaleiseca.com.br/questoes-comentadas-erros-lei-seca/))
4. **Metadados**: banca-estilo, disciplina/sub-área, nível (fácil/médio/difícil), tipo (C/E, A–E, I/II/III, V/F), âncora-fonte (ID do dispositivo/súmula/tese), data da verificação.

**Template de gabarito comentado:**
```
GABARITO: [letra/C/E]
FUNDAMENTO DA CORRETA: [tese] — base: [CF art. X / Súmula Y/STJ / Tema Z/STF], teor: "[...]".
DISTRATORES:
  (A) ERRADA — [erro específico]; o correto seria [...] (base: ...).
  (B) ERRADA — troca de [instituto1] por [instituto2].
  (C) CORRETA.
  (D) ERRADA — generalização indevida ("sempre"); há exceção em [...].
  (E) ERRADA — jurisprudência superada por [...].
FONTE-ÂNCORA: [ID verificável]  |  NÍVEL: [médio]  |  BANCA-ESTILO: [FGV]
```

### D.4 Armadilhas a EVITAR ao gerar (qualidade negativa)

- **Ambiguidade**: comando que admite mais de uma leitura. Comando deve ser único e fechado.
- **Lei desatualizada/revogada**: dispositivo alterado, súmula cancelada, tese superada (overruling).
- **Duas respostas defensáveis**: causa de anulação. Garanta **uma e só uma** correta (ou, em C/E, julgamento inequívoco).
- **Súmula/tese inexistente**: nunca inventar número de súmula, Tema ou tese — fonte fantasma é o erro fatal de IA.
- **Pegadinha indevida (incompleto = incorreto sem termo absoluto)**: tratar regra geral correta como falsa só por não esgotar exceções é abusivo (a própria FGV reconhece "incompleto ≠ incorreto"). Só marque incompleto como **errado** quando o enunciado usa **termo totalizante** (*sempre/somente/em qualquer caso*) ou quando a omissão **inverte o sentido**.
- **Distrator implausível**: alternativa absurda, descartável de imediato, não testa conhecimento.
- **Mistura de fato e direito mal calibrada**: caso fático que esconde dados essenciais ou contém contradição interna.

---

## (E) CHECKLIST ANTI-ALUCINAÇÃO (rodar em TODO item gerado)

Marque **todos** antes de liberar o item. Reprovou em um → corrigir ou descartar.

- [ ] **Âncora real e citável**: existe artigo/súmula/tese/informativo concreto que sustenta o gabarito (número + diploma/tribunal conferidos).
- [ ] **Vigência confirmada**: a lei não está revogada; a súmula não foi cancelada; a tese não foi superada (verificar data).
- [ ] **Teor fiel**: a assertiva correta reflete o **teor literal/atual** da fonte, sem distorção.
- [ ] **Súmula/Tema existe de fato**: nenhum número de Súmula, Súmula Vinculante, Tema de RG ou Repetitivo foi inventado.
- [ ] **Resposta única**: há exatamente **uma** alternativa correta (ou julgamento C/E inequívoco); nenhuma outra é defensável.
- [ ] **Comando claro e fechado**: enunciado contém tudo o que é necessário; o comando ("correta/incorreta", "conforme a súmula") é único.
- [ ] **Distratores plausíveis e errados de verdade**: cada distrator tem um erro **identificável e explicável** (não absurdo, não correto por acaso).
- [ ] **Sem pegadinha indevida**: item incompleto só é "errado" se houver termo absoluto ou inversão de sentido (regra "incompleto ≠ incorreto").
- [ ] **Estilo da banca-alvo respeitado**: formato (C/E vs A–E vs I/II/III), idioma de comando e tipo de âncora (lei seca vs jurisprudência) coerentes com a banca escolhida.
- [ ] **Nível declarado bate com o conteúdo**: fácil = memória; médio = interpretação; difícil = aplicação a caso.
- [ ] **Gabarito comentado completo**: correta fundamentada + cada distrator justificado + fonte-âncora + metadados.
- [ ] **Português correto** e diacríticos preservados (UTF-8).
- [ ] **Para peça/sentença**: o espelho gerado lista quesitos pontuados (relatório/fundamentação/dispositivo/técnica) com pesos coerentes (fundamentação ≈ 55–65%).

---

### Fontes (URLs)

- CNJ Res. 75/2009 (íntegra): https://atos.cnj.jus.br/atos/detalhar/100 — PDF oficial: https://www.cnj.jus.br/wp-content/uploads/2009/07/rescnj_75.pdf
- CNJ — ENAM: https://www.cnj.jus.br/cnj-aprova-regras-para-instituicao-do-exame-nacional-para-a-magistratura/ — https://cj.estrategia.com/portal/cnj-aprova-enam-substituir-primeira-fase-dos-concursos-magistratura/
- CJF — concurso juiz federal (atualização 2024): https://www.cjf.jus.br/cjf/noticias/2024/fevereiro/cjf-aprova-atualizacao-do-normativo-que-dispoe-sobre-concurso-publico-de-juiz-federal-substituto
- Cebraspe / C-E e "uma errada anula uma certa": https://www.estrategiaconcursos.com.br/blog/metodo-cebraspe-cespe-avaliacao/ — https://blogs.correiobraziliense.com.br/papodeconcurseiro/so-o-cebraspe-aplica-o-criterio-uma-errada-anula-uma-certa-em-concursos/ — https://www.jusbrasil.com.br/noticias/voce-sabia-que-o-cebraspe-nem-sempre-aplica-o-temido-criterio-uma-errada-anula-uma-certa/629399991
- Pegadinhas Cebraspe: https://www.estrategiaconcursos.com.br/blog/pegadinhas-de-concurso-saiba-quais-sao-as-10-maiores/ — https://concursos.academiadoraciocinio.com/2025/10/22/o-algoritmo-secreto-do-cespe-padroes-que-se-repetem-ha-10-anos-e-ninguem-percebeu/
- FGV (estilo, incompleta×incorreta, assertiva mais correta): https://www.estrategiaconcursos.com.br/blog/fgv/ — https://www.novaconcursos.com.br/portal/artigos/como-gabaritar-provas-banca-fgv-analise-completa/ — https://www.proximosconcursos.com/banca-fgv/ — https://www.proximosconcursos.com/questoes-de-multipla-escolha/ — https://olivaesouza.com.br/concurso-do-mpu-soberba-da-fgv-da-as-caras-mais-uma-vez-e-questoes-mal-formuladas-prejudicam-candidatos-mesmo-apos-gabarito-definitivo/
- Perfis FCC/VUNESP/Consulplan/IADES/IBFC/AOCP: https://blog.grancursosonline.com.br/perfil-bancas-de-concursos-publicos/ — https://blog.grancursosonline.com.br/conhecendo-as-principais-bancas-examinadoras-fcc-e-vunesp/ — https://blog.decorandoaleiseca.com.br/perfil-banca-consulplan/ — https://www.proximosconcursos.com/banca-ibfc-saiba-tudo/ — https://www.direcaoconcursos.com.br/noticias/concurso-instituto-aocp
- Espelho de sentença (estrutura/pesos): https://blog.decorandoaleiseca.com.br/prova-de-sentenca-concurso/ — https://conhecimento.fgv.br/sites/default/files/concursos/espelho-de-correcao-provas-praticas-de-sentenca_v.f.pdf — https://conhecimento.fgv.br/sites/default/files/concursos/espelho_sentenca_penal.pdf
- STJ — controle de provas/espelho: https://www.stj.jus.br/sites/portalp/Paginas/Comunicacao/Noticias/13022022-Questao-de-prova-ate-onde-a-Justica-pode-intervir-nos-criterios-da-banca-de-concurso-publico.aspx — https://magistrarcursos.com.br/blog/stj-decide-que-ausencia-de-espelho-de-correcao-em-prova-oral-de-concurso-para-magistratura-nao-e-ilegal/
- Prova oral (pontos sorteados, arguição): https://www.trt2.jus.br/images/Institucional/concursos/magistrados/XL/pontos_concurso_publicacao_procedimentos.pdf — https://cj.estrategia.com/portal/como-funciona-a-prova-oral-do-mp-sp-promotor/
- MP — peças, discursivas, regulamento: https://www.g7juridico.com.br/curso-pratica-mpe — https://arquivos.qconcursos.com/prova/arquivo_prova/129489/mpe-sp-2025-mpe-sp-promotor-de-justica-prova.pdf — https://cdn.cebraspe.org.br/concursos/mp_ce_19_promotor/arquivos/MP_CE_19_PROMOTOR_PROVAS_DISCURSIVAS.PDF — https://www.mpdft.mp.br/portal/pdf/unidades/conselho_superior/resolucoes_vigor/Resolucao_271_Concurso_MPDFT.pdf — https://www.mpf.mp.br/regiao3/estagie-conosco/provas-e-gabaritos-anteriores
- Construção de itens/distratores (teoria): https://www.nescon.medicina.ufmg.br/biblioteca/imagem/Guia-elaboracao-itens-avaliacao.pdf
- Gabarito comentado (pedagogia): https://blog.decorandoaleiseca.com.br/questoes-comentadas-erros-lei-seca/
