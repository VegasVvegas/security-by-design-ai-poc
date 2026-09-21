# Passo 1 — Definição da POC

## 1. Tema central

**Boas práticas de Security by Design aplicadas ao desenvolvimento assistido por IA, com atenção aos pequenos detalhes de configuração, acesso, código e infraestrutura que podem ampliar silenciosamente a superfície de ataque.**

A POC parte da ideia de que segurança não depende apenas da existência de ferramentas específicas de proteção.

Muitos riscos surgem de pequenas decisões tomadas durante o desenvolvimento, como uma permissão mais ampla que o necessário, uma credencial armazenada de forma inadequada, uma configuração padrão insegura, uma dependência vulnerável ou um container executado com privilégios excessivos.

Com o aumento do desenvolvimento assistido por Inteligência Artificial, esses detalhes se tornam ainda mais relevantes, pois código e configurações podem ser produzidos em grande velocidade e posteriormente incorporados ao ambiente sem uma análise adequada.

---

## 2. Problema

Ferramentas de Inteligência Artificial estão sendo utilizadas para auxiliar na criação de código, scripts, Dockerfiles, pipelines, manifests Kubernetes, configurações de infraestrutura e na resolução de problemas técnicos.

Esses artefatos podem funcionar corretamente e, ainda assim, conter pequenas decisões que não seguem boas práticas de segurança.

Exemplos incluem:

- secrets incorporados diretamente ao código;
- permissões excessivas;
- containers executados como root;
- imagens ou dependências vulneráveis;
- portas e serviços expostos desnecessariamente;
- configurações Kubernetes excessivamente permissivas;
- informações sensíveis registradas em logs;
- tokens sem limitação adequada;
- credenciais compartilhadas;
- configurações padrão mantidas sem revisão;
- dados internos fornecidos à IA durante uma solicitação;
- código ou infraestrutura gerados pela IA utilizados sem validação.

Isoladamente, alguns desses problemas podem parecer pequenos. Quando combinados, entretanto, podem aumentar significativamente a superfície de ataque da aplicação.

### Pergunta central

**Como aplicar boas práticas de Security by Design ao desenvolvimento assistido por Inteligência Artificial, identificando e reduzindo pequenos riscos de configuração, código, acesso e infraestrutura antes que eles alcancem o ambiente de produção?**

---

## 3. Motivação

A adoção de IA no desenvolvimento tende a tornar a criação e modificação de sistemas cada vez mais rápida.

Entretanto:

**Funcionar não significa estar seguro.**

Um código pode compilar.

Um container pode iniciar.

Um deployment pode funcionar.

Uma aplicação pode responder corretamente.

E ainda assim existirem configurações inseguras que não são imediatamente perceptíveis.

Por isso, esta POC considera que:

**A segurança está também nos pequenos detalhes que normalmente não são percebidos durante o desenvolvimento.**

O objetivo não será impedir o uso de IA, mas estabelecer um processo no qual aquilo que é produzido ou alterado com auxílio de IA seja analisado antes de avançar dentro do ciclo de desenvolvimento.

---

## 4. Objetivo geral

Demonstrar como boas práticas de **Security by Design** podem ser incorporadas ao desenvolvimento assistido por Inteligência Artificial para identificar, prevenir e reduzir riscos presentes em pequenos detalhes de código, configuração, acesso e infraestrutura.

A POC utilizará prioritariamente tecnologias e ferramentas open source para implementar ou validar esses controles.

---

## 5. Objetivos específicos

A POC deverá demonstrar como:

- identificar configurações inseguras que podem passar despercebidas durante o desenvolvimento;
- evitar exposição de credenciais e secrets;
- reduzir privilégios desnecessários;
- verificar código produzido ou modificado com auxílio de IA;
- analisar dependências e componentes open source utilizados pela aplicação;
- verificar boas práticas em Dockerfiles e containers;
- verificar boas práticas em configurações Kubernetes;
- controlar o acesso de aplicações e serviços aos recursos da infraestrutura;
- aplicar o princípio de menor privilégio;
- melhorar o gerenciamento de secrets;
- utilizar OpenBao para apoiar o gerenciamento seguro de credenciais;
- incorporar verificações de segurança ao CI/CD;
- impedir que determinadas falhas avancem no pipeline;
- produzir evidências das verificações realizadas;
- demonstrar a diferença entre apenas executar uma aplicação e executá-la seguindo boas práticas de segurança.

---

## 6. Princípios da POC

### Security by Design

A segurança deve fazer parte das decisões de desenvolvimento desde o início, e não ser adicionada somente depois que o sistema estiver pronto.

### Least Privilege

Cada usuário, serviço, aplicação ou componente deve possuir somente as permissões necessárias para executar sua função.

### Defense in Depth

A segurança não deve depender de um único mecanismo de proteção.

### Secure Defaults

Sempre que possível, configurações devem partir de opções mais restritivas e seguras.

### Minimização

Serviços, portas, permissões, dependências e acessos que não forem necessários devem ser removidos.

### Validação

Artefatos gerados por humanos ou por IA não devem ser considerados seguros simplesmente porque funcionam.

---

## 7. IA dentro da POC

A Inteligência Artificial será considerada uma ferramenta de apoio ao desenvolvimento.

Ela poderá auxiliar na produção ou modificação de:

- código;
- scripts;
- Dockerfiles;
- arquivos YAML;
- configurações Kubernetes;
- pipelines;
- documentação;
- configurações de infraestrutura.

Entretanto, sua saída não será considerada automaticamente confiável.

A regra da POC será:

**IA auxilia. Boas práticas orientam. Segurança valida.**

---

## 8. Fluxo analisado

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

Paralelamente, secrets e credenciais deverão possuir gerenciamento e controle de acesso apropriados, incluindo a utilização do OpenBao quando aplicável.

---

## 9. Escopo

Serão analisadas principalmente boas práticas relacionadas a:

- código;
- secrets;
- dependências;
- Git;
- CI/CD;
- Docker;
- Kubernetes;
- permissões;
- gestão de credenciais;
- OpenBao;
- logs;
- configurações produzidas ou modificadas com auxílio de IA.

O foco estará nos pequenos detalhes capazes de aumentar a superfície de ataque mesmo quando a aplicação continua funcionando normalmente.

---

## 10. Fora do escopo

A POC não pretende:

- criar uma nova Inteligência Artificial;
- treinar modelos;
- analisar a arquitetura interna de um LLM;
- provar que toda saída produzida por IA é insegura;
- substituir desenvolvedores;
- substituir revisão humana;
- implementar todos os controles existentes de cibersegurança;
- construir uma infraestrutura completa de produção;
- eliminar todos os riscos possíveis.

O objetivo é demonstrar **boas práticas aplicáveis e reproduzíveis**.

---

## 11. Hipótese

A hipótese da POC é:

**A aplicação sistemática de boas práticas de Security by Design durante o desenvolvimento assistido por IA permite identificar pequenos problemas de código, configuração, privilégios, dependências e infraestrutura antes que eles se acumulem e ampliem silenciosamente a superfície de ataque da aplicação.**

---

## 12. Mensagem principal da POC

A POC será construída sobre três ideias:

**Funcionar não significa estar seguro.**

**A segurança também está nos pequenos detalhes.**

**IA auxilia. Boas práticas orientam. Segurança valida.**

---

## 13. Resultado do Passo 1

Ao final deste passo ficam definidos:

- o tema central;
- o problema;
- a motivação;
- o objetivo geral;
- os objetivos específicos;
- os princípios orientadores;
- o papel da IA;
- o escopo;
- o que está fora do escopo;
- a hipótese que a POC pretende avaliar.

O próximo passo será transformar esses conceitos em uma **matriz de boas práticas**, relacionando cada prática ao risco que pretende reduzir, ao detalhe técnico observado, à forma de validação e à evidência produzida.
