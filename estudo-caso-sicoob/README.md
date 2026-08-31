# 🏦 Estudo de Caso 01: Engenharia de Requisitos e Especificação Funcional — Landing Page Sicoob

## 📈 1. Visão Geral e Contexto de Negócio
Este documento detalha a engenharia de requisitos e a especificação funcional da landing page da campanha Poupança Premiada Sicoob. O objetivo principal foi mapear e desenho a jornada de conversão e engajamento do cliente, garantindo que as regras dos sorteios, a tabela de períodos e os tutoriais de participação estivessem transparentes, mitigando falhas de fluxo ou links órfãos que pudessem prejudicar as metas comerciais da campanha. 

Como padrão de excelência e governança de software, a entrega também assegurou a compatibilidade com padrões universais de qualidade e acessibilidade (WCAG 2.1), ampliando o alcance digital da campanha para todos os potenciais poupadores.

---

## 🗺️ 2. Mapeamento da Jornada do Usuário e Regras de Negócio
Durante a análise funcional e a engenharia reversa da interface, foram identificados três gargalos de produto que impactavam diretamente a taxa de conversão, a retenção de usuários e a clareza das informações no portal:

* **Transparência de Premiação e Regras:** Banners contendo informações cruciais de negócio (como a regra "A cada R$ 200 depositados = 1 cupom" e as descrições dos veículos sorteados) estavam atrelados exclusivamente a elementos visuais estáticos, gerando perda total de contexto caso a camada gráfica falhasse na renderização.
* **Integridade dos Tutoriais de Conversão:** Os blocos de instrução estruturados em passos ordenados ("Como Participar" e "Como Abrir Conta") careciam de marcação semântica nativa, gerando o risco de fragmentação na ordem sequencial de leitura em dispositivos móveis ou tecnologias assistivas.
* **Saúde dos Links e CTAs (Call to Action):** Presença de elementos interativos sem rótulos de destino claros ou com âncoras vazias, criando pontos de atrito na jornada e aumentando a taxa de rejeição por cliques perdidos.

---

## 🎨 3. Fluxograma de Processo e Transição de Telas (User Flow — Miro)
Abaixo está o mapeamento visual que modela a lógica de navegação do cliente e as tomadas de decisão do sistema desde a entrada na Landing Page até a geração final do cupom:

<img width="1970" height="792" alt="Meu primeiro board" src="https://github.com/user-attachments/assets/6f7a534d-d057-4afc-a2e0-960fc212d39c" />

### 📐 Estrutura e Lógica das Caixinhas do Fluxo:
1. **[Início] (Retângulo Arredondado):** Investidor acessa a Landing Page da Campanha Poupança Premiada.
2. **[Ação] (Retângulo):** Usuário visualiza o banner e clica no CTA "Participar / Abrir Conta".
3. **[Decisão de Sistema] (Losango):** O usuário já possui conta poupança activa no Sicoob?
    * *Fluxo Sim:* Sistema direciona para a tela de Login / Digitação de Agência e Conta.
    * *Fluxo Não:* Sistema direciona para a tela de captura de dados (Abertura de Conta via app/web).
4. **[Regra de Negócio de API] (Losango):** Valor depositado é maior ou igual a R$ 200,00?
    * *Fluxo Sim:* Envia requisição para a API de sorteios -> Gera o cupom na tela [💡].
    * *Fluxo Não:* Sistema barra a transação e exibe modal com mensagem de erro tratada.

---

## 📝 4. Histórias de Usuário (User Stories) & Critérios de Aceite (BDD / Gherkin)

### 📊 História de Usuário 01: Robustez nos Banners da Campanha
* **COMO** um investidor acessando o portal da campanha de prêmios,
* **QUERO** que todas as regras de conversão de cupons e descrições das premiações estejam presentes tanto no elemento visual quanto nos metadados estruturais do sistema,
* **PARA** que o regulamento da campanha seja compreendido de forma imediata por qualquer perfil de cliente.

#### 📐 Critérios de Aceite (Padrão Gherkin / BDD):
* **Cenário: Validação de metadados em elementos visuais de impacto**
    * **Dado que** o cliente acessa a landing page da campanha Poupança Premiada Sicoob;
    * **Quando** o sistema renderizar os banners de premiação na tela;
    * **Então** o front-end deve carregar descrições textuais estruturadas (atributos `alt` e metadados) detalhando os prêmios (carros e motos) e a regra de conversão de valores (R$ 200 = 1 cupom).

---

### ⏳ História de Usuário 02: Sequenciamento Lógico dos Tutoriais
* **COMO** um potencial cliente interessado em aderir à promoção,
* **QUERO** visualizar os passos de participação em uma sequência lógica e contínua,
* **PARA** que eu consiga concluir o processo de cadastro sem perder nenhuma etapa regulamentar.

#### 📐 Critérios de Aceite (Padrão Gherkin / BDD):
* **Cenário: Estruturação semântica e hierarquia da árvore do DOM**
    * **Dado que** a interface exibe o tutorial dinâmico "Como Participar";
    * **Quando** a estrutura do código da página for processada no navegador;
    * **Então** a seção deve utilizar obrigatoriamente marcação semântica ordenada na árvore do DOM (tags `<ol>` e `<li>`), garantindo que a sequência cronológica correta seja mantida para tecnologias assistivas.

---

### 🔗 História de Usuário 03: Governança de Links e Botões de Ação (CTAs)
* **COMO** um cliente interagindo com os botões de ação e conversão do portal,
* **QUERO** que todos os links da página apontem para rotas válidas e textualmente identificadas,
* **PARA** eliminar cliques em links vazios ou botões sem resposta.

#### 📐 Critérios de Aceite (Padrão Gherkin / BDD):
* **Cenário: Auditoria de integridade de elementos interativos**
    * **Dado que** o sistema executa a validação de elementos na árvore do DOM;
    * **Quando** forem analisados os componentes do tipo link (tag `<a>`) e botões de ação;
    * **Então** 100% dos elementos devem possuir texto descritivo interno ou rótulos explícitos de destino (atributos `aria-label`).

---

## ⚙️ 5. Requisitos de Integração e Comportamento da API (Back-End)

### 🔹 Cenário A: Geração de Cupons com Sucesso (Status HTTP 201 Created)
* **Fluxo de Envio:** Ao acionar o botão de confirmação, o front-end envia para a API as variáveis informadas: `agencia`, `conta_poupanca`, `cpf_titular` e `valor_deposito`.
* **Retorno do Servidor:** A API processa a validação dos dados com sucesso, retorna o código do cupom gerado (Ex: `SIC-2026-X98F`) e o front-end renderiza a confirmação para o usuário.

### 🔹 Cenário B: Bloqueio por Elegibilidade de Valor Mínimo (Status HTTP 400 ou 422)
* **Regra de Negócio:** Caso o valor enviado seja inferior a R$ 200,00, a API recusa a transação e retorna um código de erro com uma mensagem tratada.
* **🟢 Regra Especial de Acessibilidade Digital:** Sempre que a API retornar uma falha de negócio (`400` ou `422`), a interface web deve capturar dinamicamente a string da mensagem de erro e renderizá-la em um componente visual com foco automático e marcação semântica `aria-live="assertive"`. Essa regra de integração garante que o leitor de tela anuncie o aviso de forma imediata, oferecendo autonomia para o usuário corrigir a ação [💡].
*outs de carrossel móvel devem conter marcações textuais internas que garantam redundância de leitura caso o carregamento de mídia externa sofra delay ou restrição de servidor.
