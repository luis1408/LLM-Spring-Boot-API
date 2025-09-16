# LLM Spring Boot API

Este projeto é uma API REST desenvolvida em Spring Boot para integração com modelos de linguagem natural (LLMs), utilizando a biblioteca [LangChain4j](https://github.com/langchain4j/langchain4j). O objetivo é fornecer uma interface HTTP simples para enviar perguntas e receber respostas de um modelo como o Llama3, rodando localmente via [Ollama](https://ollama.com/) ou outro endpoint compatível com OpenAI.

---

## Estrutura do Projeto

```
src/
  main/
    java/
      com.example.llm/
        LlmApplication.java         # Classe principal da aplicação Spring Boot
        controller/
          ControllerApi.java        # Controller REST que expõe o endpoint de chat
    resources/
      application.properties        # Configurações da aplicação
      static/
      templates/
  test/
    java/
      com.example.llm/
        LlmApplicationTests.java    # Testes automatizados
```

---

## Funcionamento

- **Endpoint principal:**  
  `GET /chat?mensagem=...`  
  Você envia uma mensagem via parâmetro `mensagem` e recebe a resposta gerada pelo modelo LLM configurado.

- **Integração LLM:**  
  O projeto utiliza o LangChain4j para se conectar a um endpoint compatível com OpenAI (por exemplo, Ollama rodando localmente).  
  O modelo e o endpoint são configuráveis via `application.properties`.

- **Controller:**  
  O arquivo `ControllerApi.java` define o endpoint `/chat`, injeta o `ChatModel` do LangChain4j e faz a ponte entre a requisição HTTP e o modelo de linguagem:

  ```java
  @RestController
  @RequestMapping("/chat")
  public class ControllerApi {

      ChatModel chatModel;

      public ControllerApi(ChatModel chatModel) {
          this.chatModel = chatModel;
      }

      @GetMapping
      public String chat(@RequestParam String mensagem) {
          return chatModel.chat(mensagem);
      }
  }
  ```

---

## Configuração

Edite o arquivo `src/main/resources/application.properties` conforme necessário:

```properties
langchain4j.open-ai.chat-model.api-key=SEU_API_KEY
langchain4j.open-ai.chat-model.model-name=llama3
langchain4j.open-ai.chat-model.base-url=http://localhost:11434/v1
langchain4j.open-ai.chat-model.temperature=0.2
spring.rest.template.read-timeout=60s
spring.rest.template.connect-timeout=30s
```

- **api-key:** Pode ser fictício se usar Ollama local.
- **base-url:** URL do seu servidor LLM (ex: Ollama).
- **model-name:** Nome do modelo carregado (ex: `llama3`).
- **Timeouts:** Ajuste conforme a performance do seu modelo/servidor.

---

## Como rodar

1. **Inicie o servidor LLM** (ex: Ollama) e carregue o modelo desejado:
   ```sh
   ollama run llama3
   ```
2. **Compile e execute a aplicação:**
   ```sh
   ./mvnw spring-boot:run
   ```
3. **Faça uma requisição para o endpoint:**
   ```sh
   curl "http://localhost:8080/chat?mensagem=Olá, tudo bem?"
   ```

---

## Requisitos

- Java 17 ou superior
- Maven
- Servidor LLM (ex: [Ollama](https://ollama.com/))

---

## Observações

- Os timeouts podem ser ajustados em `application.properties` para lidar com respostas lentas do modelo.
- O projeto serve como exemplo básico de integração entre Spring Boot e LLMs via LangChain4j.
- O endpoint `/chat` aceita apenas requisições GET com o parâmetro `mensagem`.

---



