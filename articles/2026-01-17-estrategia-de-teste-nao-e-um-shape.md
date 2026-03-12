---
layout: post
title: "Estratégia de Teste não é um Shape"
description: "Reflexões sobre por que estratégia de teste é um sistema de decisões e contexto, não um modelo geométrico fixo."
date: 2026-01-17
tags: [qa, test-strategy, qualidade, arquitetura, engenharia-de-qualidade]
canonical_url: "https://medium.com/@pri-campos/estrat%C3%A9gia-de-teste-n%C3%A3o-%C3%A9-um-shape-9c5cd1e306a8"
reading_time: "6 min"
---

![Capa do artigo](/assets/images/posts/estrategia-shape/cover.png)

> Publicado originalmente no Medium: {{ page.canonical_url }}

---

## Contexto

> O shape é um resultado possível de uma estratégia, não a estratégia em si.

Há quase quatro anos escrevi o artigo Estratégia de Teste para APIs, com ênfase no uso da Pirâmide e dos Quadrantes Ágeis como balizadores para organizar níveis e abordagens de teste. Embora eu ainda considere a Pirâmide uma forma eficaz de comunicar custo e velocidade, e os Quadrantes úteis para discutir o valor gerado pelos testes, esse artigo já não representa como penso sobre testes hoje.

A simplificação de “escolher um shape” — seja Pyramid, Trophy, Honeycomb, Diamond, ou qualquer outro — é sedutora porque cria uma sensação rápida de decisão. Você escolhe um modelo, encaixa seus testes nele, e parece que o trabalho estratégico foi feito.

Na prática, porém, o shape raramente responde às perguntas que realmente determinam se uma estratégia funciona:

 - *Quais riscos são relevantes para este sistema?*
 - *Onde esses riscos se manifestam, em termos de fronteiras técnicas?*
 - *Que tipo de validação é capaz de capturar esses riscos com custo e feedback adequados?*

Embora os shapes sejam atalhos cognitivos úteis, eles escondem as decisões reais que estão acontecendo na cabeça das pessoas. Com isso, o debate tende a se concentrar em formatos, em vez de riscos e fronteiras, o que dificulta a construção de modelos mentais sobre como decidir estratégia de teste — especialmente em contextos de onboarding e formação técnica.

---
## Antes do shape, o problema

Antes de pensar em shape, vale organizar o entendimento do contexto onde a qualidade precisa ser validada — seja por meio de dimensões, perguntas-guia, mapas ou qualquer outra estrutura que ajude a tornar explícitos os riscos e as fronteiras do sistema. A seguir, um exemplo de como esse contexto pode ser estruturado:

![Capa do artigo](/assets/images/posts/estrategia-shape/context-map.png)

Após o assessment do contexto, a saída esperada não é um desenho de testes, mas a definição dos eixos de decisão: o que precisa ser validado, onde, em que momento e sob quais heurísticas e critérios essas escolhas deverão ser feitas para mitigar riscos e alcançar os objetivos de qualidade.

> Estratégia nasce de decisões. Decisões nascem de entendimento.

Dependendo do volume de riscos mapeados, vale combinar essa análise com a abordagem de [Risk-Based Testing (RBT)](https://www.testmuai.com/learning-hub/risk-based-testing/), considerando probabilidade e impacto para definir prioridades que orientem a organização do backlog e, como consequência, a alocação mais eficaz do esforço de teste.

---
## Aos poucos, um padrão começa a surgir

Com esse mapa de contexto em mãos, o próximo passo é sair do nível de abstração e começar a transformar entendimento em escolhas técnicas progressivas. Para isso, ajuda muito organizar as decisões de teste em uma sequência.

⚠️ risco → 🧭 fronteira → 🛠️ mecanismo de validação

(o que importa → onde validar → como validar)

Essa sequência ajuda a transformar uma visão estratégica em escolhas técnicas progressivas, evitando tanto a seleção prematura de tipos de teste quanto decisões técnicas desconectadas de onde o risco realmente se manifesta no sistema.

A seguir, um exemplo fictício de mapeamento para uma IDP cujo portal de desenvolvedor é baseado em Backstage:

<div class="csv-preview">
  <div id="risk-boundary-table">Carregando tabela…</div>
</div>

<style>
  .csv-preview { overflow-x: auto; }
  .csv-preview table { border-collapse: collapse; width: 100%; min-width: 720px; }
  .csv-preview th, .csv-preview td {
    border-bottom: 1px solid #eee;
    padding: 8px;
    text-align: left;
    vertical-align: top;
    white-space: normal;
    word-break: break-word;
  }
  .csv-preview th { border-bottom: 1px solid #ccc; }
</style>

<script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
<script>
  const rawCsvUrl =
    "https://gist.githubusercontent.com/pri-campos/e71c9a9ed1848efa08313f5069655b0a/raw/4500e7d54872e9f40cf5ac8d7aed5ec9eed18f4c/risk_boundary_validation_map.csv";

  function escapeHtml(str) {
    return String(str ?? "")
      .replaceAll("&", "&amp;")
      .replaceAll("<", "&lt;")
      .replaceAll(">", "&gt;")
      .replaceAll('"', "&quot;")
      .replaceAll("'", "&#039;");
  }

  function renderTable(headers, rows) {
    let html = "<table><thead><tr>";
    for (const h of headers) html += `<th>${escapeHtml(h)}</th>`;
    html += "</tr></thead><tbody>";

    for (const row of rows) {
      html += "<tr>";
      for (const h of headers) html += `<td>${escapeHtml(row[h])}</td>`;
      html += "</tr>";
    }

    html += "</tbody></table>";
    return html;
  }

  (async function () {
    const mount = document.getElementById("risk-boundary-table");

    try {
      const res = await fetch(rawCsvUrl, { cache: "no-store" });
      if (!res.ok) throw new Error("Falha ao baixar CSV: " + res.status);
      const csvText = await res.text();

      const parsed = Papa.parse(csvText, {
        header: true,
        skipEmptyLines: true,
        quotes: true
      });

      const headers = parsed.meta.fields || [];
      const rows = (parsed.data || []).filter(r =>
        headers.some(h => (r[h] ?? "").toString().trim() !== "")
      );

      mount.innerHTML = renderTable(headers, rows);
    } catch (e) {
      mount.textContent = "Não foi possível carregar a tabela.";
      console.error(e);
    }
  })();
</script>

Depois dessas decisões, o shape começa a emergir naturalmente — não porque alguém “escolheu” um modelo visual, mas porque certos riscos pedem validações mais próximas de contratos, outros exigem integração controlada, e alguns só fazem sentido quando observados em runtime real.

---
## Quase lá — mas ainda há armadilhas no caminho

Mesmo quando o shape já começa a se tornar visível, ainda existe um risco comum: assumir que todo mundo está usando as mesmas palavras para falar das mesmas coisas.

 - *Teste de unidade é quando só uma função roda ou quando um comportamento pequeno é validado?*
 - *Teste de integração é quando módulos conversam ou quando serviços conversam?*
 - *Teste de sistema é quando tudo está no ar ou quando o fluxo completo faz sentido?*
 - *Teste end-to-end é quando existe UI ou quando a jornada está coberta, mesmo sem tela?*
 - *O nível vem do tamanho do teste ou da fronteira que ele cruza?*
*…e a lista continua.*

Depois de muitos debates sobre “a melhor estratégia de teste”, aprendi duas coisas: **1)** às vezes as pessoas discordam apenas porque usam nomes diferentes para práticas muito parecidas ou **2)** em outros casos, todo mundo parece concordar… até você perceber que cada pessoa está chamando de “teste de integração” algo completamente diferente.

Para sair desse jogo de “ou isso ou aquilo”, vale observar como grupos técnicos inseridos em contextos distintos de produto e arquitetura buscaram reduzir a ambiguidade de taxonomia na prática, adaptando as classificações ao tipo de sistema que precisavam sustentar.

### Classificação por interação

#### 📌Martin Fowler — Unit vs Integration
 - **Testes de Unidade (Unit):** Podem ser feitos de duas formas: no estilo *solitary*, quando a unidade é testada isoladamente com todas as dependências substituídas por mocks ou outros tipos de substitutos; e no estilo *sociable*, quando a unidade é testada junto com seus colaboradores reais, mantendo fora apenas dependências externas ao escopo do componente.
 - **Testes de Integração (Integration):** Também aparecem em dois níveis. Na forma *narrow*, o serviço real é exercitado, mas as fronteiras externas são controladas com infraestrutura de teste ou substitutos, focando principalmente no código de integração (por exemplo, usando test doubles, harness ou contract tests). Já na forma *broad*, a validação ocorre com múltiplos serviços reais interagindo entre si, em configurações equivalentes às de produção, para observar o comportamento do sistema distribuído.

🔗 https://martinfowler.com/bliki/UnitTest.html

🔗 https://martinfowler.com/bliki/IntegrationTest.html

### Classificação por tamanho e custo operacional

#### 📌 Google — Small / Medium / Large
Aqui o foco não é o escopo funcional validado, mas o impacto operacional do teste: recursos envolvidos, tempo de execução e custo de manutenção.

🔗 https://testing.googleblog.com/2010/12/test-sizes.html

### Classificação por escopo e acoplamento do sistema

#### 📌 Spotify — Detail / Integration / Integrated (Honeycomb Strategy)
A distinção está no grau de dependência entre sistemas e no foco em interações entre serviços versus detalhes internos de componentes.

🔗 https://engineering.atspotify.com/2018/01/testing-of-microservices/

#### 📌 Kent C. Dodds — Static / Unit / Integration / E2E (Testing Trophy)
Embora utilize rótulos familiares, a classificação é feita pelo escopo da validação e pelo grau de dependência do sistema real, redefinindo o que cada nível cobre na prática.

🔗 https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications

> Independentemente dos rótulos adotados para compor os níveis da estratégia, o ponto central é o mesmo: o time precisa compartilhar um vocabulário comum para transformar decisões estratégicas em ações técnicas consistentes.

---
## Habemus Shape

Depois de mapear riscos, fronteiras e mecanismos de validação, a pergunta deixa de ser “qual modelo seguir?” e passa a ser “como tornar visível as decisões que já tomamos?”.

Nesse ponto, o shape não define a estratégia — ele representa a distribuição de esforço de teste que faz sentido para aquele contexto. Ou seja: o desenho não vem primeiro. Ele surge como síntese de decisões sobre risco, custo e tipo de verificação.

*No exemplo fictício anterior, eu adotaria um desenho híbrido: Honeycomb para evidenciar integrações e contratos entre serviços, e Testing Trophy para priorizar validações próximas ao comportamento de uso.*

---
## Conclusão

Usar o shape como ponto de partida é tentar condensar, em um único rótulo, decisões que vivem em dimensões diferentes — intenção, fronteira técnica, custo operacional e tipo de evidência necessária. Na prática, ele emerge da arquitetura, dos riscos e do tipo de validação que o sistema exige naquele momento.

E como esses fatores mudam — seja por evolução arquitetural, novos fluxos críticos, pelo impacto crescente do uso de IA ou pela própria natureza de sistemas modernos, cada vez mais distribuídos, orientados a contratos e dependentes de observabilidade como parâmetro de qualidade — o shape também muda.

> **Lei de Goodhart:** “Quando uma medida se torna uma meta, ela deixa de ser uma boa medida.”

Porque estratégia não é fazer o problema caber no formato — é entender o problema antes de escolher a forma.


![Imagem final](/assets/images/posts/estrategia-shape/illustration.gif)
