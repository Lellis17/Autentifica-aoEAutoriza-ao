# Documentação da API REST JWT

## Visão Geral

Este projeto demonstra uma implementação simples de uma API REST com autenticação JWT (JSON Web Token) usando Spring Boot. A API inclui papéis de usuários e controle de acesso para diferentes endpoints.

## Pré-requisitos

- Java 8 ou superior
- Maven ou Gradle
- IDE (IntelliJ IDEA, Eclipse, etc.)

## Estrutura do Projeto

O projeto está dividido em vários pacotes, cada um com responsabilidades específicas:

1. **application**: Contém a classe principal para executar a aplicação Spring Boot.
2. **config**: Contém classes de configuração para as configurações de segurança.
3. **controller**: Contém controladores REST para manipulação de requisições HTTP.
4. **model**: Contém modelos de dados para requisições e respostas.
5. **security**: Contém classes utilitárias para operações JWT.
6. **service**: Contém classes de serviço para lógica de negócios.

## Componentes Principais

### Ponto de Entrada da Aplicação

- `JwtRestApiApplication`: A classe principal que inicializa a aplicação Spring Boot.

### Configuração de Segurança

- `SecurityConfig`: Configura as definições de segurança HTTP, detalhes do usuário e codificação de senhas.

### Controladores

- `AuthController`: Manipula requisições relacionadas à autenticação.

### Modelos

- `LoginRequest`: Representa a carga útil da requisição de login.

### Utilitário de Segurança

- `JwtUtil`: Contém métodos para gerar e extrair informações de JWTs.

### Serviços

- `AuthService`: Fornece métodos para gerar tokens e extrair nomes de usuário.

## Descrição Detalhada

### Ponto de Entrada da Aplicação

O ponto de entrada da aplicação é a classe `JwtRestApiApplication`. Ela utiliza a anotação `@SpringBootApplication` para habilitar a configuração automática e a varredura de componentes.

### Configuração de Segurança

A classe `SecurityConfig` configura o seguinte:

- **Proteção CSRF**: Desativada para simplicidade.
- **Segurança dos Endpoints**: Configura quais endpoints são acessíveis para quais papéis:
  - `/login/**`, `/username/**`, `/user/**`, `/admin/**`, `/moderado/**` e `/comum/**` são todos acessíveis sem autenticação.
  - `/admin/**` é restrito a usuários com o papel "ADMIN".
  - `/moderado/**` é restrito a usuários com o papel "MODERADO".
  - `/comum/**` é restrito a usuários com o papel "COMUM".
  - Todas as outras requisições requerem autenticação.

A classe também configura um serviço de detalhes do usuário em memória com três usuários tendo diferentes papéis e um codificador de senhas.

### Controladores

O `AuthController` fornece os seguintes endpoints:

- **POST /login**: Autentica um usuário e retorna um token JWT.
- **GET /username/{token}**: Extrai o nome de usuário de um token JWT fornecido.
- **GET /user**: Retorna as informações do usuário autenticado.
- **GET /admin**: Restrito a usuários administradores; retorna informações específicas do administrador.
- **GET /moderado**: Restrito a usuários moderados; retorna informações específicas do moderador.
- **GET /comum**: Acessível a usuários comuns; retorna informações específicas do usuário comum.

### Modelos

A classe `LoginRequest` representa a carga útil para a requisição de login, contendo um nome de usuário e uma senha.

### Utilitário de Segurança

A classe `JwtUtil` fornece métodos para:

- Gerar um token JWT para um determinado nome de usuário.
- Extrair o nome de usuário de um token JWT fornecido.
- Extrair o ID de um token JWT fornecido (método não utilizado na implementação atual).

### Serviços

A classe `AuthService` atua como intermediária entre o controlador e a classe utilitária. Ela utiliza o `JwtUtil` para gerar tokens e extrair nomes de usuário de tokens.

## Uso

1. **Executar a Aplicação**: Use sua IDE ou linha de comando para executar a classe `JwtRestApiApplication`.
2. **Autenticar**: Envie uma requisição POST para `/login` com uma carga JSON contendo `username` e `password`. Exemplo:
    ```json
    {
      "username": "Larissa",
      "password": "4321"
    }
    ```
3. **Acessar Endpoints Seguros**: Use o token JWT retornado para acessar outros endpoints, incluindo-o no cabeçalho `Authorization` como um token Bearer.

## Exemplos de Requisições

1. **Requisição de Login**:
    ```sh
    curl -X POST http://localhost:8080/login -H "Content-Type: application/json" -d '{"username": "Larissa", "password": "4321"}'
    ```
2. **Extrair Nome de Usuário**:
    ```sh
    curl -X GET http://localhost:8080/username/{token}
    ```
3. **Acessar Endpoint de Administrador**:
    ```sh
    curl -X GET http://localhost:8080/admin -H "Authorization: Bearer {token}"
    ```

## Conclusão

Este projeto fornece uma compreensão fundamental de como implementar autenticação JWT em uma aplicação Spring Boot. Inclui controle de acesso baseado em papéis e demonstra o manuseio seguro da autenticação e autorização de usuários.

Para mais detalhes, consulte o código e os comentários dentro de cada classe.

## Diagrama 

![AUTENTICACAO-DIAGRAMA](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/d71b3e0c-f674-4edc-887a-a4584864e6f4)


## Imagens do Insomnia em execução

![Captura de tela 2024-06-14 221713](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/ee6834a6-8645-4aeb-8a31-d827d0ca3ec8)

![Captura de tela 2024-06-14 221657](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/bc617ba7-bd5d-4df0-92fa-c41b409eeb7e)

![Captura de tela 2024-06-14 221635](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/cb79bbd1-06fa-4ad4-8e53-2433e99b4af5)

![Captura de tela 2024-06-14 221610](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/b755d5dd-56e2-4eaa-b63d-4194f5b43089)

![Captura de tela 2024-06-14 221420](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/7825e7f4-5a90-4169-82b6-81dfed03f166)

![Captura de tela 2024-06-14 221730](https://github.com/Lellis17/Autentifica-aoEAutoriza-ao/assets/111644936/40e27dbc-f7f6-4724-8c2f-d5970c1f09e9)






