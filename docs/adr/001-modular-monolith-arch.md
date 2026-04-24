# ADR 001 — Arquitetura de Monólito Modular

**Status:** Aceito
**Data:** 22/04/2026

## Contexto

O projeto é desenvolvido por um time de estudantes universitários com nível iniciante a intermediário, com prazo de entrega até a segunda semana de junho de 2026. O sistema é uma REST API de gerenciamento de biblioteca física com domínios bem definidos: livro, membro, empréstimo e multa.

Precisávamos de uma arquitetura que equilibrasse organização, facilidade de ensino e divisão clara de tarefas entre o time.

## Decisão

Adotamos o **Monólito Modular** — organização por domínio com camadas internas em cada domínio.

Estrutura de pacotes:

```
br.unibh/
  book/
    api/
    service/
    repository/
  member/
    api/
    service/
    repository/
  loan/
    api/
    service/
    repository/
  fine/
    api/
    service/
    repository/
```

Responsabilidades de cada camada:
- `api/` — controladores de rota (controllers) e dados da requisição/resposta (DTOs), recebe e responde requisições HTTP
- `service/` — lógica de negócio e regras do domínio
- `repository/` — acesso ao banco de dados

## Consequências

**🟢 Positivo:**
- Cada membro do time pode ser responsável por um domínio inteiro
- A separação por camadas impede que lógica de negócio vaze para os controllers
- Reflete o que o mercado usa em projetos reais
- Fácil de validar com ArchUnit

**🔴 Negativo:**
- Mais arquivos e pacotes do que uma arquitetura em camadas simples
- Curva de aprendizado ligeiramente maior para quem nunca viu essa organização
- Risco de acoplamento entre domínios se o time não seguir as regras definidas