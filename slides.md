---
theme: default
background: /EV-Image-2x.png
# some information about your slides, markdown enabled
title: MOOV.OLT - Transformando a Mobilidade Elétrica no Brasil
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
fonts:
  sans: Montserrat
---

<h1 class="scale-150 font-black">

  # MOOV.OLT

</h1>


Transformando a Mobilidade Elétrica no Brasil

<div class="pt-12 animate-pulse">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Pressione para o lado <kbd><carbon:arrow-right class="inline"/></kbd>
  </span>
</div>

---
transition: slide-up
layout: image-right
image: /App-workflow.svg
backgroundSize: contain
---

<h1 class="b rounded-xl p8">

  # Breve Especificação da Aplicação

</h1>


## Arquitetura do Servidor
A aplicação adere ao protocolo [**OCPP**](https://en.wikipedia.org/wiki/Open_Charge_Point_Protocol). Ela consistirá num esquema cliente-servidor com dois componentes principais: <br>
1. [**Serviço de Ponto de Recarga (SPR)**]()
  - API de interação direta com estações de carregamento físicas
2. [**Sistema de Gerenciamento (SG)**]()
  - Responsável por permissões, pagamentos, etc. O [**SG**]() consiste em um modelo [**cliente-servidor**]() com o [**Servidor**]() se comunicando via protocolo [**AMQP**](https://pt.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) com os [**SPRs**]()


<style>
h1 {
  background-color: #2B90B6;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: two-cols
layoutClass: gap-16
class: text-xl
---

# 1. Serviço de Ponto de Recarga (SPR)
## ![image](/SPR-API.svg)

::right::

- Executa tarefas fornecidas pelo [**Servidor**]().
- Responsável pela interação direta com as estações de carregamento físicas.
- Estabelece e gerencia conexões [Websocket](https://pt.wikipedia.org/wiki/WebSocket).
- Recebe e envia dados de/para as estações de carregamento.
- Não toma decisões sobre permissões ou capacidade de carregamento.

<style>
h1 {
  background-color: #2B90B6;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: image-right
backgroundSize: contain
image: /SG.svg
---

# 2. Sistema de Gerenciamento (SG)

- Gerencia a lógica de negócios, incluindo permissões, controle do processo de carregamento e pagamentos.
- Não tem conhecimento sobre o funcionamento interno do [**SPR**]().
- Aceita dados dos serviços de [**SPR**](), toma decisões e envia tarefas de volta para execução baseda no tipo de mensagem (serviço solicitado).
- Utiliza o protocolo [**AMQP**]() para comunicação com os [**SPRs**]().

<style>
h1 {
  background-color: #2B90B6;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: center
transition: slide-up
---

# TECNOLOGIAS DO BACKEND

---
layout: image-right
backgroundSize: contain
image: /Rust-cases.png
---

# **Rust**
Linguagem de Programação

  - **Alta performance e vasto ecossistema Web**
  - **Uso eficiente de recursos do sistema**
  - **Segurança verificável e garantida**

<style>
h1 {
  color: white;
  background-color: #a5300f;
  text-align: center;
  border-radius: 1rem;
  padding: 0.5rem 1rem;
}
</style>

#### Exemplo - Servidor Axum
```rust twoslash
use axum::routing::{get, post};
use serde::{Deserialize, Serialize};
use tokio::net::TcpListener;

#[tokio::main]
async fn main() {
    let app = Router::new().route("/", get(root));
    let sock = TcpListener::bind("0.0.0.0:3000")
      .await.unwrap(); // run async with `hyper`
    axum::serve(sock, app).await.unwrap();
}
// responds with a static string
async fn root() -> &'static str {
    "Hello, World!"
}
```

---
layout: image-right
image: /API-flow.svg
backgroundSize: contain
---

# OpenAPI
Linguagem de descrição de API

- **[OpenAPI](https://www.openapis.org/)** é compatível com diversos ferramentas de desenvolvimento oferecendo flexibilidade na seleção de fornecedores.
- O conhecimento comum do OpenAPI entre desenvolvedores e engenheiros proporciona flexibilidade na contratação de pessoal.
- A <span v-mark.circle.green>abstração multi-linguagem</span> facilita a adoção de inovações nos comportamentos da API, evitando a necessidade de reescritas totais.

<style>
  h1 {
    color: black;
    background-color: #93d500;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---
layout: image-right
image: /RabbitMQ-process.svg
backgroundSize: contain
---

# RabbitMQ
Sistema de comunicação Middleware

- O [RabbitMQ](https://www.rabbitmq.com/) suporta vários protocolos padrão abertos, incluindo [**AMQP**](https://pt.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) e [**MQTT**](https://pt.wikipedia.org/wiki/MQTT). Existem várias bibliotecas de cliente disponíveis, que podem ser usadas com a linguagem de programação de sua escolha. Sem bloqueio de fornecedor!

- Oferece muitas opções para definir como suas mensagens vão do publicador (aplicação) para um ou muitos consumidores (roteamento, filtragem, streaming, etc).

- Garantia que a troca de mensagens não será interceptada, fornecendo segurança ao consumidores da aplicação.

<style>
  h1 {
    color: white;
    background-color: #ff6600;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---
layout: image-right
image: /postgresql.svg
backgroundSize: contain
---

# PostgreSQL
Gerenciador de Banco de Dados SQL

- Conformidade com SQL
- Variedade rica de tipos de dados proporcionando flexibilidade na criação de diversas estruturas de dados
- [**Multi Processamento**](): recursos de indexação, transações e particionamento de tabelas favorecem operações concorrentes e processamento de alta performance.
- [**Segurança**](): possui um framework de segurança robusto com suporte para vários métodos de autenticação garantindo integridade e confidencialidade dos dados.

<style>
  h1 {
    color: white;
    background-color: #ff6600;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---
layout: image-right
image: /Docker.svg
backgroundSize: contain
---

# Docker
- Oferece ambientes isolados ([**Máquinas Virtuais Linux**](https://pt.wikipedia.org/wiki/Virtualiza%C3%A7%C3%A3o)) para aplicativos, garantindo que cada aplicativo execute sem interferência de outros.
- Os contêineres compartilham o mesmo kernel do sistema operacional em uso, resultando em uma utilização mais eficiente de recursos em comparação com máquinas virtuais tradicionais.
- <span v-mark.circle.cyan>[**Portabilidade garantida**]()</span>: os contêineres podem ser executados em qualquer lugar (`localhost` ou nuvem) mantendo o mesmo comportamento.

<style>
  h1 {
    color: white;
    background-color: #0091e2;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---
preload: false
layout: center
---

# TECNOLOGIAS DO FRONTEND

---
layout: image-right
backgroundSize: contain
image: /TypeScript.svg
---

# **TypeScript:**
- [**Integração com JavaScript**](): sendo apenas um superconjunto do [**JavaScript**](), oferece compatibilidade total com o vasto ecossistema [**JavaScript**]().
- [**Adição de Tipos**](): com tipagem forte e um conceito de interfaces, o TypeScript torna mais fácil trabalhar em projetos grandes e complexos, fornecendo maior clareza e segurança.
- <span v-mark.circle.blue>[**Tipagem Estática**]()</span>: oferece verificação de tipos estáticos durante o desenvolvimento, detectando erros antes mesmo da execução do código.

<style>
  h1 {
    color: white;
    background-color: #3178c6;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---
layout: image-right
backgroundSize: contain
image: UI-UX.svg
---

# React
- [**Componentização**](): permite dividir a interface do usuário em componentes reutilizáveis, facilitando o desenvolvimento e manutenção de aplicações.
- [**Fluxo unidirecional**](): simplifica o gerenciamento de estado, tornando-o mais previsível e fácil de depurar. Extensível através do [**Redux**](https://redux-toolkit.js.org/).
- <span v-mark.circle.cyan>[**Virtual DOM**]():</span> oferece uma atualização de página apenas nas partes necessárias da interface, resultando em um melhor desempenho e experiência do usuário.


<style>
  h1 {
    color: white;
    background-color: #00D8FF;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---
layout: image-right
backgroundSize: contain
image: TailwindCSS.svg
---
# TailWindCSS

- [**Produtividade**](): oferece classes pré-definidas para estilos comuns, acelerando o processo de desenvolvimento e permitindo prototipagem rápida.
- [**Customização Flexível**](): com base em classes utilitárias, facilita a personalização de estilos sem a obrigatoriedade de escrever [**CSS**]() personalizado, proporcionando flexibilidade total.
- [**Manutenção Simplificada**](): A abordagem baseada em utilitários torna a manutenção do código mais simples, pois as alterações de estilo são centralizadas e facilmente identificáveis.


<style>
  h1 {
    color: white;
    background-color: #2b85d6;
    text-align: center;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
</style>

---

# Recursos Principais

### **Escalabilidade**
- A arquitetura permite fácil escalabilidade adicionando [**SPRs**]() adicionais, tornando-a adequada para gerenciar um grande número de estações de carregamento físicas sem depender de um único [**Servidor**]().
### **Flexibilidade e Extensibilidade**
- A separação de funções entre o [**SPR**]() e o [**SG**]() permite a fácil adição de novos recursos sem alterações significativas na arquitetura geral do sistema.
### **Gerenciamento de Desempenho**
- O sistema baseado em fila de mensagens, aliado ao backend escrito em [**Rust**](), permite multi-processamento, controle e prioridade de processamento, garantindo uma resposta rápida às solicitações dos clientes.
### **Abertura e Extensibilidade**
- Utilizando padrões abertos e tecnologias open-source populares, permite fácil integração com outros sistemas e serviços, como sistemas de pagamento, plataformas de controle e aplicações de terceiros.

---

# Plano de Desenvolvimento

## **Configuração do Backend**
- Desenvolvimento dos componentes do backend, incluindo o [**SPR**]() e o [**SG**](), com as seguintes prioridades:
    1. Primeiramente offline (conexão de internet apenas para sincronia de dados)
    2. Monitoramento das funções do hardware
    3. Gestão financeira
    4. Cadastro de clientes
## **Desenvolvimento do Frontend**
- Integração da [UI]() com o backend logo após a finalização do mesmo.
## **Modelagem do Banco de Dados**
- Design e implementação do esquema do banco de dados após o cadastro de clientes e todos os testes de integridade de dados passarem como válidos.
## **Melhorias e Novas Funcionalidades**
- Após todos os testes passarem como válidos, pensaremos em novas ideias e melhorias para o futuro da aplicação.


---
class: text-center
---

# MOOV.OLT
Saiba mais em *[moovolt.netlify.app](https://moovolt.netlify.app/)*
 e visite o projeto no GitHub:
  <a href="https://github.com/FlipSoftware/moov.olt-mvp/tree/main?tab=readme-ov-file" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>

 <img border="rounded" src="/Moovolt-concept.png" alt="">
