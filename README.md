# Security by Design no Desenvolvimento Assistido por IA

## Sobre a POC

Esta POC estuda a aplicação de **boas práticas de Security by Design ao desenvolvimento assistido por Inteligência Artificial**, com atenção aos pequenos detalhes de configuração, acesso, código e infraestrutura que podem ampliar silenciosamente a superfície de ataque.

A proposta não é tratar a IA como necessariamente insegura nem adicionar ferramentas de segurança sem justificativa. O foco é demonstrar como boas práticas podem acompanhar o desenvolvimento desde a geração ou modificação de artefatos com IA até sua validação e execução na infraestrutura.

> **Funcionar não significa estar seguro.**

> **A segurança também está nos pequenos detalhes.**

> **IA auxilia. Boas práticas orientam. Segurança valida.**

## Problema

Ferramentas de IA podem auxiliar na geração de código, scripts, Dockerfiles, pipelines, manifests Kubernetes e outras configurações. Mesmo quando esses artefatos funcionam corretamente, pequenos detalhes podem introduzir riscos, como:

- secrets incorporados ao código;
- permissões excessivas;
- containers executados como root;
- dependências vulneráveis ou desnecessárias;
- serviços e portas expostos sem necessidade;
- configurações Kubernetes excessivamente permissivas;
- informações sensíveis em logs;
- credenciais mal gerenciadas;
- configurações padrão inseguras;
- envio indevido de dados sensíveis para uma IA.

A pergunta central da POC é:

**Como aplicar boas práticas de Security by Design ao desenvolvimento assistido por Inteligência Artificial, identificando e reduzindo pequenos riscos de configuração, código, acesso e infraestrutura antes que eles alcancem o ambiente de produção?**

## Objetivo

Demonstrar como boas práticas de Security by Design podem ser incorporadas ao desenvolvimento assistido por IA para identificar, prevenir e reduzir riscos em código, configuração, acesso e infraestrutura, utilizando prioritariamente tecnologias e ferramentas open source.

## Fluxo inicial

```text
Desenvolvedor
      |
      v
      IA
      |
      v
Código / Configuração / Infraestrutura
      |
      v
Validações de segurança
      |
      v
     Git
      |
      v
    CI/CD
      |
      v
   Docker
      |
      v
 Kubernetes
      |
      v
 Aplicação
```

Secrets e credenciais serão tratados com controles específicos de gestão de segredos, incluindo **OpenBao** quando aplicável.

## Documentação

- [Passo 1 — Definição da POC](docs/01-definicao-da-poc.md)
- [Passo 2 — Matriz de Boas Práticas](docs/02-matriz-boas-praticas.md)

## Status

**Etapa atual:** matriz de boas práticas e definição dos controles do MVP.

A próxima etapa será definir a **arquitetura mínima da POC**, especificando apenas os componentes necessários para demonstrar os controles selecionados.
