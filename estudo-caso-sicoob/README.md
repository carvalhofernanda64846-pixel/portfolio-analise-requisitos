# 📊 Estudo de Caso 01: Especificação de Requisitos e Regras de Negócio — Landing Page Sicoob

## 1. Visão Geral e Contexto de Negócio
Este documento detalha a engenharia de requisitos e a especificação funcional da landing page da campanha **Poupança Premiada Sicoob 2026**. O objetivo foi redesenhar e auditar a **jornada de conversão e engajamento do cliente**, garantindo que as regras dos sorteios, a tabela de períodos e os tutoriais de participação estivessem 100% claros, sem falhas de fluxo ou links órfãos. Como padrão de excelência de qualidade, a entrega também assegurou compatibilidade com padrões universais de acessibilidade (WCAG 2.1), ampliando o alcance da campanha para 100% do público.

---

## 2. Mapeamento da Jornada do Usuário e Regras de Negócio
Durante a análise funcional da landing page, identificamos três gargalos principais que impactavam diretamente a conversão e a clareza das informações:

* **Transparência de Premiação e Regras:** Banners com informações cruciais (como "A cada R$ 200 = 1 cupom" e os modelos dos veículos sorteados) estavam atrelados exclusivamente a elementos visuais estáticos, gerando perda de contexto caso a camada gráfica falhasse na renderização.
* **Integridade dos Tutoriais ("Como Participar" e "Como Abrir Conta"):** Blocos de instrução em passos ordenados careciam de marcação semântica estrutural, o que poderia fragmentar a leitura sequencial em dispositivos móveis ou de renderização alternativa.
* **Saúde dos Links e CTAs (Call to Action):** Foram mapeados elementos interativos sem rótulos de destino ou com âncoras vazias, gerando risco de cliques perdidos na jornada do usuário.

---

## 3. Histórias de Usuário (User Stories) e Critérios de Aceite (BDD / Gherkin)

### História de Usuário 01: Clareza e Robustez nos Banners de Campanha
> **Como** usuário acessando a campanha de prêmios,  
> **Quero** que todas as informações de regras e premiações estejam presentes tanto no elemento visual quanto nos metadados textuais do sistema,  
> **Para que** a proposta de valor da campanha seja compreendida de forma imediata por qualquer canal de acesso.

* **Critérios de Aceite (Gherkin):**
  * **Cenário:** Validação de metadados em elementos visuais de impacto
    * **Dado** que o usuário acessa a landing page da campanha Sicoob 2026,
    * **Quando** o sistema renderiza os banners de premiação,
    * **Então** o front-end deve associar descrições textuais estruturadas (`alt` / metadados) detalhando os prêmios (3 Fiat Strada, 1 Hilux e motos Honda) e a regra de conversão (R$ 200 = 1 cupom).

---

### História de Usuário 02: Sequenciamento Lógico dos Tutoriais de Conversão
> **Como** potencial cliente interessado em participar da promoção,  
> **Quero** visualizar os passos de participação e abertura de conta em uma sequência lógica e contínua,  
> **Para que** eu não perca nenhuma etapa regulamentar ou aviso importante sobre elegibilidade.

* **Critérios de Aceite (Gherkin):**
  * **Cenário:** Estruturação semântica de listas de instruções
    * **Dado** que a página exibe o tutorial "Como Participar",
    * **Quando** a arquitetura de código for compilada,
    * **Então** a seção deve utilizar marcação semântica nativa (`<ol>` e `<li>`), garantindo que o DOM mantenha a ordem correta de leitura sem saltar blocos informativos.

---

### História de Usuário 03: Governança de Links e Redirecionamentos
> **Como** usuário interagindo com os botões de ação (CTA),  
> **Quero** que todos os links da página apontem para rotas válidas e rotuladas,  
> **Para que** o sistema elimine pontos de atrito ou cliques em "links vazios".

* **Critérios de Aceite (Gherkin):**
  * **Cenário:** Auditoria de integridade de elementos âncora
    * **Dado** que o sistema passa por uma varredura de validação estática de código,
    * **Quando** o motor analisar os componentes do tipo link (`<a>`),
    * **Então** 0% dos elementos interativos podem estar desprovidos de texto descritivo ou rótulo de destino (`aria-label`).
