# ADR 002 — Stack Tecnológica: Java + Javalin + PostgreSQL + Docker

**Status:** Aceito  
**Data:** 22/04/2026

## Contexto

O time precisa de uma stack que seja ensinável em pouco tempo, reflita o mercado real de desenvolvimento backend e seja viável de configurar em máquinas Windows e macOS dentro do prazo do projeto.

## Decisão

Adotamos a seguinte stack:

| Camada                       | Tecnologia              | Versão |
|------------------------------|-------------------------|--------|
| Linguagem                    | Java                    | 25     |
| Framework web                | Javalin                 | 7.2.0  |
| Banco de dados               | PostgreSQL              | 18     |
| Infraestrutura               | Docker                  | 4.66.1 |
| Compilação                   | Gradle Wrapper          | 9.4.0  |
| Migrações SQL                | Liquibase               | 5.0.2  |
| Explorador de banco de dados | DBeaver                 | última |
| Testes de API                | Postman                 | última |
| Editor de código             | IntelliJ IDEA Community | última |

## Consequências

**🟢 Positivo:**
- Java é a linguagem ensinada no curso — sem curva de aprendizado de linguagem
- Javalin é minimalista e explícito, sem mágica — o time entende o que está acontecendo
- PostgreSQL é o banco relacional mais adotado no mercado
- Docker garante ambiente idêntico entre Windows e macOS, eliminando o clássico problema de "funciona na minha máquina"
- Gradle Wrapper elimina a necessidade de instalar o Gradle manualmente
- Liquibase gerencia as migrações do banco, criando e versionando as tabelas sem necessidade de execução individual de scripts SQL
- DBeaver e Postman são gratuitos e amplamente usados no mercado

**🔴 Negativo:**
- Javalin exige que o time configure manualmente o que frameworks como Spring Boot entregam automaticamente
- Docker Desktop exige que a máquina tenha virtualização habilitada — pode ser um problema em máquinas antigas
- Java 25 é recente e pode ter menos respostas disponíveis em fóruns para problemas específicos
- Postman exige cadastro para uso completo