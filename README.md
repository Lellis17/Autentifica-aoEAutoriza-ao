# JWT Rest API

Este projeto é uma implementação de uma API REST utilizando Spring Boot com segurança baseada em JWT (JSON Web Token).

## Índice

- [Pré-requisitos](#pré-requisitos)
- [Instalação](#instalação)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Configuração de Segurança](#configuração-de-segurança)
- [Utilização](#utilização)
- [Endpoints](#endpoints)
- [Contribuição](#contribuição)
- [Licença](#licença)

## Pré-requisitos

Antes de começar, certifique-se de ter o seguinte instalado:

- JDK 8 ou superior
- Maven
- IDE (recomendado: IntelliJ, Eclipse, VS Code)

## Instalação

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/jwt-rest-api.git
   ```
2. Navegue até o diretório do projeto:
   ```bash
   cd jwt-rest-api
   ```
3. Compile o projeto usando Maven:
   ```bash
   mvn clean install
   ```
4. Execute a aplicação:
   ```bash
   mvn spring-boot:run
   ```

## Estrutura do Projeto

```plaintext
jwt-rest-api
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── example
│   │   │           ├── JWT_RestAPI
│   │   │           │   ├── application
│   │   │           │   │   └── JwtRestApiApplication.java
│   │   │           │   ├── config
│   │   │           │   │   └── SecurityConfig.java
│   │   │           │   ├── controller
│   │   │           │   │   └── AuthController.java
│   │   │           │   ├── model
│   │   │           │   │   └── LoginRequest.java
│   │   │           │   ├── security
│   │   │           │   │   └── JwtUtil.java
│   │   │           │   └── service
│   │   │           │       └── AuthService.java
│   │   └── resources
│   │       └── application.properties
│   └── test
│       └── java
│           └── com
│               └── example
│                   └── JWT_RestAPI
│                       └── JwtRestApiApplicationTests.java
└── pom.xml
```

### Classe Principal

**JwtRestApiApplication.java**

```java
package com.example.JWT_RestAPI.application;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication(scanBasePackages = {"com.example"})
public class JwtRestApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(JwtRestApiApplication.class, args);
    }
}
```

### Configuração de Segurança

**SecurityConfig.java**

Configura a segurança da aplicação, definindo permissões de acesso e configurando autenticação básica.

```java
package com.example.JWT_RestAPI.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                .csrf(AbstractHttpConfigurer::disable)
                .authorizeHttpRequests(request -> request
                        .requestMatchers(HttpMethod.POST, "/login/**").permitAll()
                        .requestMatchers(HttpMethod.GET, "/username/**").permitAll()
                        .requestMatchers(HttpMethod.GET, "/user/**").permitAll()
                        .requestMatchers(HttpMethod.GET, "/admin/**").permitAll()
                        .requestMatchers(HttpMethod.GET, "/moderado/**").permitAll()
                        .requestMatchers(HttpMethod.GET, "/comum/**").permitAll()
                        .requestMatchers("/admin/**").hasRole("ADMIN")
                        .requestMatchers("/moderado/**").hasRole("MODERADO")
                        .requestMatchers("/comum/**").hasRole("COMUM")
                        .anyRequest()
                        .authenticated()
                ).httpBasic(Customizer.withDefaults());
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
                .username("Larissa")
                .password(passwordEncoder().encode("4321"))
                .roles("ADMIN")
                .build();
        UserDetails moderado = User.builder()
                .username("Chandler")
                .password(passwordEncoder().encode("123"))
                .roles("MODERADO")
                .build();
        UserDetails comum = User.builder()
                .username("Rachel")
                .password(passwordEncoder().encode("456"))
                .roles("COMUM")
                .build();
        return new InMemoryUserDetailsManager(user, moderado, comum);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

## Utilização

### Autenticação

Para autenticar um usuário, envie uma requisição POST para o endpoint `/login` com um payload JSON contendo `username` e `password`.

**Exemplo de Requisição:**

```json
POST /login
Content-Type: application/json

{
  "username": "Larissa",
  "password": "4321"
}
```

**Exemplo de Resposta:**

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJMYXJpc3NhIiwiaWF0IjoxNjQ0MjkxOTUyfQ.wTzIwo9R5doB9gH8BpLw3MPBZ-3dGw7ibuhFl7qOUoQ"
}
```

## Endpoints

### `POST /login`

Autentica o usuário e retorna um token JWT.

### `GET /username/{token}`

Extrai o nome de usuário a partir de um token JWT.

### `GET /user`

Retorna o nome do usuário autenticado.

### `GET /admin`

Acesso restrito para administradores.

### `GET /moderado`

Acesso restrito para usuários moderados.

### `GET /comum`

Acesso restrito para usuários comuns.

## Contribuição

1. Fork o projeto.
2. Crie uma nova branch:
   ```bash
   git checkout -b minha-feature
   ```
3. Faça as alterações necessárias.
4. Commit suas mudanças:
   ```bash
   git commit -m 'Adicionando minha feature'
   ```
5. Push para a branch:
   ```bash
   git push origin minha-feature
   ```
6. Abra um Pull Request.

## Licença

Distribuído sob a licença MIT. Veja `LICENSE` para mais informações.

---

Esta documentação deve fornecer uma visão geral suficiente para que outros desenvolvedores possam entender, instalar, configurar e contribuir com o projeto.
