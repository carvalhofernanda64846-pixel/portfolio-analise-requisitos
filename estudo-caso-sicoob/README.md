# 🏦 Estudo de Caso 01: Engenharia de Requisitos e Especificação Funcional — Landing Page Sicoob

## 📈 1. Visão Geral e Contexto de Negócio
Este documento detalha a engenharia de requisitos e a especificação funcional da landing page da campanha Poupança Premiada Sicoob. O objetivo principal foi mapear e desenhar a jornada de conversão e engajamento do cliente, garantindo que as regras dos sorteios, a tabela de períodos e os tutoriais de participação estivessem transparentes, mitigando falhas de fluxo ou links órfãos que pudessem prejudicar as metas comerciais da campanha. 

Como padrão de excelência e governança de software, a entrega também assegurou a compatibilidade com padrões universais de qualidade e acessibilidade (WCAG 2.1), ampliando o alcance digital da campanha para 100% dos potenciais poupadores.

---

## 🗺️ 2. Mapeamento da Jornada do Usuário e Regras de Negócio
Durante a análise funcional e a engenharia reversa da interface, foram identificados três gargalos de produto que impactavam diretamente a taxa de conversão, a retenção de usuários e a clareza das informações no portal:

*   **Transparência de Premiação e Regras:** Banners contendo informações cruciais de negócio (como a regra "A cada R$ 200 depositados = 1 cupom" e as descrições dos veículos sorteados) estavam atrelados exclusivamente a elementos visuais estáticos, gerando perda total de contexto e quebra no fluxo de conversão caso a camada gráfica falhasse na renderização.
*   **Integridade dos Tutoriais de Conversão:** Os blocos de instrução estruturados em passos ordenados ("Como Participar" e "Como Abrir Conta") careciam de marcação semântica nativa, gerando o risco de fragmentação na ordem sequencial de leitura em dispositivos móveis ou tecnologias assistivas.
*   **Saúde dos Links e CTAs (Call to Action):** Presença de elementos interativos sem rótulos de destino claros ou com âncoras vazias, criando pontos de atrito na jornada e aumentando a taxa de rejeição por cliques perdidos.

---

## 📝 3. Histórias de Usuário (User Stories) & Critérios de Aceite (BDD / Gherkin)

### 📊 História de Usuário 01: Robustez e Transparência nos Banners da Campanha
*   **COMO** um investidor ou poupador acessando o portal da campanha de prêmios,
*   **QUERO** que todas as regras de conversão de cupons e descrições das premiações estejam presentes tanto no elemento visual quanto nos metadados estruturais do sistema,
*   **PARA** que a proposta de valor e o regulamento da campanha sejam compreendidos de forma imediata por qualquer perfil de cliente.

#### 📐 Critérios de Aceite (Padrão Gherkin / BDD):
*   **Cenário:** Validação de metadados em elementos visuais de impacto
    *   **Dado que** o cliente acessa a landing page da campanha Poupança Premiada Sicoob;
    *   **Quando** o sistema renderizar os banners de premiação na tela;
    *   **Então** o front-end deve carregar descrições textuais estruturadas (atributos `alt` e metadados) detalhando os prêmios (3 Fiat Strada, 1 Hilux e motos Honda) e a regra de conversão de valores (R$ 200 = 1 cupom).

---

### ⏳ História de Usuário 02: Sequenciamento Lógico dos Tutoriais de Conversão
*   **COMO** um potencial cliente interessado em aderir à promoção,
*   **QUERO** visualizar os passos de participação e o fluxo de abertura de conta em uma sequência lógica, intuitiva e contínua,
*   **PARA** que eu consiga concluir o processo de cadastro sem perder nenhuma etapa regulamentar ou aviso de elegibilidade.

#### 📐 Critérios de Aceite (Padrão Gherkin / BDD):
*   **Cenário:** Estruturação semântica e hierarquia de listas de instruções
    *   **Dado que** a interface exibe o tutorial dinâmico "Como Participar";
    *   **Quando** a arquitetura do código da página for compilada no navegador;
    *   **Então** a seção deve utilizar obrigatoriamente marcação semântica ordenada (tags `<ol>` e `<li>`), garantindo que o DOM mantenha a sequência cronológica correta e proíba saltos inesperados de blocos informativos.

---

### 🔗 História de Usuário 03: Governança de Links e Botões de Ação (CTAs)
*   **COMO** um cliente interagindo com os botões de ação e conversão do portal,
*   **QUERO** que todos os links da página apontem para rotas válidas, íntegras e textualmente identificadas,
*   **PARA** mitigar pontos de frustração e eliminar cliques em "links vazios" ou botões sem resposta.

#### 📐 Critérios de Aceite (Padrão Gherkin / BDD):
*   **Cenário:** Auditoria de integridade de elementos âncora interativos
    *   **Dado que** o sistema executa uma varredura de validação estática de código na árvore do DOM;
    *   **Quando** o motor analisar os componentes globais do tipo link (tag `<a>`) e botões de ação;
    *   **Então** 0% dos elementos interativos podem estar desprovidos de texto descritivo interno ou de rótulos explícitos de destino (atributos `aria-label`).

---

## 💡 4. Diretrizes Técnicas Adicionais para a Engenharia
*   **Saneamento de Tags Semânticas (HTML5):** Toda a arquitetura do portal deve seguir a árvore do DOM de forma limpa, garantindo a ordenação e a correta aplicação de cabeçalhos (`<h1>` a `<h6>`) para evitar quebras de contexto informacional.
*   **Blindagem de Metadados:** Elementos de mídia e layouts de carrossel móvel devem conter marcações textuais internas que garantam redundância de leitura caso o carregamento de mídia externa sofra delay ou restrição de servidor.
