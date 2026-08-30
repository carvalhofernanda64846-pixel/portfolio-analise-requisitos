# 📊 Estudo de Caso 01: Análise de Requisitos e Regras de Negócio — Landing Page Sicoob

## 1. Visão Geral e Contexto de Negócio
Este documento apresenta a engenharia reversa e a especificação de requisitos funcionais e estruturais da landing page da campanha **Poupança Premiada Sicoob 2026**. O objetivo principal é estruturar a jornada de conversão, mapear regras de negócio dos sorteios e garantir a integridade dos fluxos de instrução ao usuário. Como padrão de excelência de engenharia, o projeto incorpora diretrizes rigorosas de usabilidade universal e conformidade legal (**WCAG 2.1** e **Artigo 63 da Lei Brasileira de Inclusão**), assegurando uma experiência sem barreiras para 100% do público-alvo.

---

## 2. Diagnóstico Inicial e Baseline de Qualidade
* **Pontuação Técnica Inicial (WAVE WebAIM):** 2,7 / 10 (Identificação de gargalos estruturais no código).
* **Erros de Integridade e Layout:** 11 falhas estruturais e 52 inconsistências de contraste de design.
* **Violações Automatizadas (Axe-core):** 72 ocorrências mapeando falhas de rotulagem de elementos, links órfãos e restrições de navegação em componentes de interface.

---

## 3. Histórias de Usuário (User Stories) e Critérios de Aceite (BDD / Gherkin)

### História de Usuário 01: Clareza e Rastreabilidade em Banners Promocionais
> **Como** usuário acessando a campanha de prêmios,  
> **Quero** que os banners informativos e visuais contem com metadados e descrições textuais completas,  
> **Para que** as regras da promoção (valores de participação e premiações) fiquem transparentes independentemente do canal de renderização ou tecnologia utilizada.

* **Critérios de Aceite (Gherkin):**
  * **Cenário:** Validação de metadados no Banner Principal de Prêmios
    * **Dado** que o usuário carrega a landing page da campanha Sicoob 2026,
    * **Quando** o sistema renderiza o banner principal de prêmios,
    * **Então** o elemento gráfico deve possuir um atributo descritivo associado (`alt` ou equivalente estrutural) detalhando os prêmios (3 Fiat Strada, 1 Hilux e motos Honda) e a regra de conversão (R$ 200 = 1 cupom).

---

### História de Usuário 02: Integridade na Ordem de Leitura e Tutoriais Passo a Passo
> **Como** usuário navegando pelos tutoriais da campanha,  
> **Quero** que as instruções "Como Participar" e "Como Abrir Conta" sigam uma ordem lógica e estruturada de exibição,  
> **Para que** nenhuma etapa do regulamento ou aviso de elegibilidade seja omitido ou pulado pelo sistema.

* **Critérios de Aceite (Gherkin):**
  * **Cenário:** Estruturação semântica de tutoriais passo a passo
    * **Dado** que a página exibe o passo a passo de participação,
    * **Quando** a arquitetura front-end estruturar o componente,
    * **Então** os passos devem utilizar tags HTML5 semânticas ordenadas (`<ol>` e `<li>`), garantindo que a árvore de elementos do DOM mantenha a sequência lógica de leitura sem saltar blocos de avisos legais ou regras de menores de idade.

---

### História de Usuário 03: Tratamento de Links e Redirecionamentos Órfãos
> **Como** usuário interagindo com os elementos de chamada para ação (CTA),  
> **Quero** que todos os links e botões da página possuam rotulagem clara e destinos definidos,  
> **Para que** não ocorram redirecionamentos falhos ou elementos interativos vazios na interface.

* **Critérios de Aceite (Gherkin):**
  * **Cenário:** Eliminação de elementos âncora vazios detectados em validações estáticas
    * **Dado** que o código passa por uma varredura de qualidade estática (`cypress-axe` / WAVE),
    * **Quando** o motor analisar os componentes do tipo link (`<a>`),
    * **Então** 0% dos elementos interativos podem estar sem texto descritivo ou rótulo de destino (`aria-label`), assegurando clareza total na navegação.
