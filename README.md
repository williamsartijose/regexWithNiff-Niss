
# Documentação de Validação de NIF e NISS com Expressões Regulares

Este documento fornece uma visão detalhada sobre as expressões regulares utilizadas para validar dois documentos importantes: o NIF (Número de Identificação Fiscal) em Portugal e o NISS (Número de Identificação da Segurança Social) em Portugal

## Índice
1. [Introdução](#introdução)
2. [O que é NIF?](#o-que-é-nif)
3. [O que é NISS?](#o-que-é-niss)
4. [Expressões Regulares](#expressões-regulares)
   - [Regex para NIF](#regex-para-nif)
   - [Regex para NISS](#regex-para-niss)
5. [Exemplos de Uso](#exemplos-de-uso)
6. [Considerações Finais](#considerações-finais)

## Introdução

As expressões regulares (regex) são ferramentas poderosas para a validação de formatos específicos em strings, como números de documentos. Este documento explora a criação de regex para dois documentos usados em Portugal e no Brasil: o NIF e o NIS, respectivamente.

## O que é NIF?

**NIF (Número de Identificação Fiscal)** é um número de 9 dígitos utilizado em Portugal para identificar contribuintes, sejam pessoas físicas ou jurídicas. Este número é essencial para a administração de impostos e para a realização de operações financeiras e fiscais no país.

### Estrutura do NIF:
- 9 dígitos numéricos (exemplo: `314549625`).
- Pode ser formatado com pontos como separadores opcionais (exemplo: `314.549.625`).

## O que é NISS?

**NISS (Número de Identificação da Segurança Social)** é um número utilizado em Portugal para identificar trabalhadores e beneficiários de serviços sociais. O NISS é gerado pela Segurança Social e é essencial para a administração de benefícios como pensões e subsídios.

### Estrutura do NISS:
- 11 dígitos numéricos (exemplo: `12345678901`).
- Pode ser formatado com pontos e um hífen como separadores opcionais (exemplo: `123.456.789-01`).

## Expressões Regulares

### Regex para NIF

A regex abaixo é usada para validar o formato do NIF, considerando que ele pode ser escrito com ou sem pontos como separadores.

```regex
\d{3}.?\d{3}.?\d{3}
```

#### Explicação:
- `\d{3}`: Corresponde a três dígitos numéricos.
- `.?`: Permite um ponto opcional após cada grupo de três dígitos.
  
#### Exemplos de Correspondências:
- `314549625`
- `314.549.625`
  
#### Exemplos de Não Correspondências:
- `31454.9625`
- `31.4549.625`

### Regex para NISS

A regex abaixo é usada para validar o formato do NISS, permitindo a inclusão opcional de pontos e um hífen como separadores.

```regex
\d{3}.?\d{5}.?\d{2}-?\d{1}
```

#### Explicação:
- `\d{3}`: Corresponde aos três primeiros dígitos do NISS.
- `.?`: Permite um ponto opcional após os três primeiros dígitos.
- `\d{5}`: Corresponde aos próximos cinco dígitos do NISS.
- `.?`: Permite um ponto opcional após os cinco dígitos centrais.
- `\d{2}`: Corresponde aos dois próximos dígitos do NISS.
- `-?`: Permite um hífen opcional antes do último dígito.
- `\d{1}`: Corresponde ao último dígito do NISS.
  
#### Exemplos de Correspondências:
- `12345678901`
- `123.45678901`
- `123.456.789-01`
- `1234567-8901`
  
#### Exemplos de Não Correspondências:
- `123.45.678901`
- `12.345678901`
- `123456789-012`

## Exemplos de Uso

Para usar expressões regulares para validação de NIF e NISS em uma aplicação Spring Boot com Bean Validation, você pode criar anotações personalizadas que utilizam regex para validar os formatos dos números. Abaixo está um exemplo completo de como fazer isso em Java.

### Passo 1: Criar Anotações Personalizadas

Primeiro, você precisa criar anotações personalizadas para validar NIF e NISS.

#### Anotação para NIF
Crie uma anotação `@ValidNIF`:

```java
package com.example.validation;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Constraint(validatedBy = NIFValidator.class)
@Target({ ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE })
@Retention(RetentionPolicy.RUNTIME)
public @interface ValidNIF {
    String message() default "Invalid NIF format";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

#### Anotação para NISS
Crie uma anotação `@ValidNIS`:

```java
package com.example.validation;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Constraint(validatedBy = NISValidator.class)
@Target({ ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE })
@Retention(RetentionPolicy.RUNTIME)
public @interface ValidNIS {
    String message() default "Invalid NIS format";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

### Passo 2: Criar Validadores

Crie classes de validador que implementam `ConstraintValidator`.

#### Validador para NIF
Crie a classe `NIFValidator`:

```java
package com.example.validation;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

public class NIFValidator implements ConstraintValidator<ValidNIF, String> {

    private static final String NIF_REGEX = "\\d{3}.?\\d{3}.?\\d{3}";

    @Override
    public void initialize(ValidNIF constraintAnnotation) {
    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if (value == null) {
            return true; // Não faz validação se for nulo, você pode ajustar conforme sua necessidade
        }
        return value.matches(NIF_REGEX);
    }
}
```

#### Validador para NISS
Crie a classe `NISValidator`:

```java
package com.example.validation;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

public class NISValidator implements ConstraintValidator<ValidNIS, String> {

    private static final String NIS_REGEX = "\\d{3}.?\\d{5}.?\\d{2}-?\\d{1}";

    @Override
    public void initialize(ValidNIS constraintAnnotation) {
    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if (value == null) {
            return true; // Não faz validação se for nulo, você pode ajustar conforme sua necessidade
        }
        return value.matches(NIS_REGEX);
    }
}
```

### Passo 3: Usar as Anotações em sua Entidade

Agora você pode usar essas anotações em suas classes de modelo.

#### Exemplo de Modelo com NIF e NISS

```java
package com.example.model;

import com.example.validation.ValidNIF;
import com.example.validation.ValidNIS;

import javax.validation.constraints.NotNull;

public class User {

    @NotNull
    @ValidNIF
    private String nif;

    @NotNull
    @ValidNIS
    private String nis;

    // Getters e Setters
    public String getNif() {
        return nif;
    }

    public void setNif(String nif) {
        this.nif = nif;
    }

    public String getNis() {
        return nis;
    }

    public void setNis(String nis) {
        this.nis = nis;
    }
}
```

### Passo 4: Testar a Validação

Você pode testar a validação ao submeter um formulário em sua aplicação Spring Boot. A validação será realizada automaticamente se você usar o Bean Validation (Hibernate Validator) com Spring Boot.

### Exemplo de Controller

Aqui está um exemplo de controlador que usa o modelo com validação:

```java
package com.example.controller;

import com.example.model.User;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import javax.validation.Valid;
import org.springframework.http.ResponseEntity;

@RestController
public class UserController {

    @PostMapping("/users")
    public ResponseEntity<String> createUser(@Valid @RequestBody User user) {
        // Se a validação falhar, uma exceção será lançada e a resposta retornará um erro
        return ResponseEntity.ok("User is valid!");
    }
}
```

Este exemplo demonstra como usar anotações personalizadas e validações em uma aplicação Spring Boot para garantir que os números de NIF e NIS estejam no formato correto.

## Considerações Finais

- **Flexibilidade**: As regex fornecidas são flexíveis e permitem diferentes formatos de entrada, aumentando a usabilidade para os usuários.
- **Precisão**: Essas regex são projetadas para corresponder apenas aos formatos válidos de NIF e NISS, evitando erros comuns de formatação.
- **Uso**: Essas expressões podem ser usadas em qualquer linguagem que suporte regex para validar a entrada de dados de usuários em formulários ou sistemas.

Com essas expressões regulares, você pode assegurar que os números de NIF e NISS sejam inseridos corretamente, facilitando a validação automática em aplicações web e de software.

---

# William Sarti José

Bem-vindo ao meu perfil!

Aqui estão alguns links importantes para o meu trabalho e perfil profissional:

- **[LinkedIn](https://www.linkedin.com/in/william-analistadesistema/)**: Conecte-se comigo no LinkedIn para ver minhas experiências e atualizações profissionais.
- **[Artigo Publicado](https://www.linkedin.com/pulse/como-criar-e-validar-express%25C3%25B5es-regulares-para-nif-nis-jos%25C3%25A9-odref/)**: Leia meu artigo sobre como criar e validar expressões regulares para NIF/NIS.
- **[GitHub](https://github.com/williamsartijose)**: Confira meus projetos e contribuições no GitHub.

Obrigado por visitar!
