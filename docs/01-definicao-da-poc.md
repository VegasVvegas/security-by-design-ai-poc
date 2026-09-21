# Passo 1 — Definição da POC

## 1. Tema central

**Boas práticas de Security by Design aplicadas ao ciclo de desenvolvimento assistido por IA, desde a definição de requisitos até a operação, com atenção aos pequenos detalhes de configuração, acesso, código, dependências e infraestrutura que podem ampliar silenciosamente a superfície de ataque.**

A POC parte da ideia de que segurança não depende apenas da existência de ferramentas específicas de proteção.

Muitos riscos surgem de pequenas decisões tomadas durante o desenvolvimento, como uma permissão mais ampla que o necessário, uma credencial armazenada de forma inadequada, uma configuração padrão insegura, uma dependência vulnerável ou desatualizada, uma funcionalidade sem necessidade real ou um container executado com privilégios excessivos.

Com o aumento do desenvolvimento assistido por Inteligência Artificial, esses detalhes se tornam ainda mais relevantes, pois requisitos, código, configurações e infraestrutura podem ser produzidos ou modificados em grande velocidade e posteriormente incorporados ao ambiente sem análise suficiente.

---

## 2. Problema

Ferramentas de Inteligência Artificial já podem participar de várias etapas do desenvolvimento de software, incluindo:

- definição e refinamento de requisitos;
- geração e modificação de código;
- criação de scripts;
- Dockerfiles;
- pipelines;
- manifests Kubernetes;
- documentação;
- análise de dados;
- testes;
- revisão e correção de código;
- configurações de infraestrutura.

Esses artefatos podem funcionar corretamente e, ainda assim, conter decisões que não seguem boas práticas de segurança ou de engenharia.

Exemplos incluem:

- requisitos incompletos, excessivos ou ambíguos;
- funcionalidades implementadas sem necessidade real;
- secrets incorporados diretamente ao código;
- permissões excessivas;
- containers executados como root;
- bibliotecas, imagens ou APIs vulneráveis, obsoletas ou desatualizadas;
- dependências desnecessárias;
- portas e serviços expostos sem necessidade;
- configurações Kubernetes excessivamente permissivas;
- informações sensíveis registradas em logs;
- tokens sem limitação adequada;
- credenciais compartilhadas;
- configurações padrão mantidas sem revisão;
- dados internos fornecidos à IA durante uma solicitação;
- uma IA validando a saída de outra IA sem mecanismo independente de verificação;
- código ou infraestrutura gerados por IA utilizados sem compreensão suficiente por quem irá operar o sistema.

Isoladamente, alguns desses problemas podem parecer pequenos. Quando combinados, entretanto, podem aumentar significativamente a superfície de ataque e reduzir a capacidade humana de compreender, operar e evoluir o sistema.

### Pergunta central

**Como aplicar boas práticas de Security by Design ao ciclo de desenvolvimento assistido por Inteligência Artificial, mantendo rastreabilidade, confiabilidade, compreensibilidade e controle humano enquanto pequenos riscos são identificados e reduzidos antes de alcançar a produção?**

---

## 3. Motivação

A adoção de IA no desenvolvimento tende a tornar a criação e modificação de sistemas cada vez mais rápida.

Entretanto:

**Funcionar não significa estar seguro.**

Um requisito pode parecer completo.

Um código pode compilar.

Um container pode iniciar.

Um deployment pode funcionar.

Uma aplicação pode responder corretamente.

E ainda assim existirem decisões desnecessárias, dependências inadequadas ou configurações inseguras que não são imediatamente perceptíveis.

A IA não será tratada como o problema. Ela pode reduzir tempo, apoiar análise e aumentar produtividade. O problema surge quando sua velocidade é acompanhada por pouca compreensão, pouca validação ou ausência de critérios sobre o que realmente precisa existir.

Por isso, esta POC considera que:

**A segurança está também nos pequenos detalhes que normalmente não são percebidos durante o desenvolvimento.**

E acrescenta uma segunda pergunta:

**Não basta perguntar se funciona. É preciso perguntar se precisa existir.**

---

## 4. Objetivo geral

Demonstrar como boas práticas de **Security by Design** podem ser incorporadas ao ciclo de desenvolvimento assistido por Inteligência Artificial para identificar, prevenir e reduzir riscos presentes em requisitos, código, dependências, configuração, acesso e infraestrutura, preservando controle humano e capacidade de operação e evolução do sistema.

A POC utilizará prioritariamente tecnologias e ferramentas open source para implementar ou validar esses controles.

---

## 5. Objetivos específicos

A POC deverá demonstrar como:

- avaliar a necessidade real de funcionalidades, componentes e dependências antes de incorporá-los;
- melhorar a clareza e a validação de requisitos antes da geração de código;
- identificar configurações inseguras que podem passar despercebidas durante o desenvolvimento;
- evitar exposição de credenciais e secrets;
- reduzir privilégios desnecessários;
- verificar código produzido ou modificado com auxílio de IA;
- evitar que uma segunda IA seja o único mecanismo de validação da primeira;
- analisar dependências e componentes open source quanto à necessidade, origem, atualização e vulnerabilidades;
- verificar boas práticas em Dockerfiles e containers;
- verificar boas práticas em configurações Kubernetes;
- controlar o acesso de aplicações e serviços aos recursos da infraestrutura;
- aplicar o princípio de menor privilégio;
- melhorar o gerenciamento de secrets;
- utilizar OpenBao para apoiar o gerenciamento seguro de credenciais;
- incorporar verificações de segurança ao CI/CD;
- impedir que determinadas falhas avancem no pipeline;
- produzir evidências das verificações realizadas;
- manter rastreabilidade das alterações e decisões;
- favorecer a compreensão, operação e evolução dos artefatos produzidos;
- demonstrar a diferença entre apenas executar uma aplicação e executá-la seguindo boas práticas de segurança e engenharia.

---

## 6. Princípios da POC

### Security by Design

A segurança deve fazer parte das decisões de desenvolvimento desde o início, inclusive durante a definição de requisitos, e não ser adicionada somente depois que o sistema estiver pronto.

### Necessidade antes da implementação

Antes de proteger um novo componente, deve-se perguntar se ele realmente precisa existir.

### Least Privilege

Cada usuário, serviço, aplicação ou componente deve possuir somente as permissões necessárias para executar sua função.

### Defense in Depth

A segurança não deve depender de um único mecanismo de proteção.

### Secure Defaults

Sempre que possível, configurações devem partir de opções mais restritivas e seguras.

### Minimização

Serviços, rotas, portas, permissões, dependências, funcionalidades e acessos que não forem necessários devem ser removidos.

### Validação independente

Uma saída gerada por IA não deve ser considerada validada somente porque outra IA a revisou. Sempre que aplicável, deverão existir testes, políticas, scanners, verificações determinísticas ou revisão humana.

### Rastreabilidade

Deve ser possível compreender o que foi alterado, quando, por qual processo e com qual justificativa.

### Compreensibilidade

Quem opera o sistema deve ser capaz de entender os artefatos e decisões relevantes para sua execução e segurança.

### Confiabilidade

O avanço de um artefato deve depender de evidências verificáveis, e não apenas de sua aparência de correção.

### Evolução

O sistema deve poder ser atualizado, corrigido e mantido sem depender cegamente da reprodução de uma resposta anterior de IA.

---

## 7. IA dentro da POC

A Inteligência Artificial será considerada uma ferramenta de apoio ao desenvolvimento.

Ela poderá auxiliar na produção ou modificação de:

- requisitos e especificações;
- código;
- scripts;
- Dockerfiles;
- arquivos YAML;
- configurações Kubernetes;
- pipelines;
- testes;
- documentação;
- configurações de infraestrutura;
- análise e correção de artefatos.

Entretanto, sua saída não será considerada automaticamente confiável.

A pessoa responsável pelo sistema deverá ser capaz de justificar, compreender ou validar aquilo que será incorporado ao ambiente.

A regra da POC será:

**IA auxilia. Boas práticas orientam. Segurança valida.**

---

## 8. Fluxo analisado

```text
Necessidade real
      |
      v
Requisitos / Especificação
      |
      v
Desenvolvedor + IA
      |
      v
Código / Configuração / Infraestrutura
      |
      v
Minimização + Validação independente
      |
      v
Rastreabilidade
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
      |
      v
Operação / Evolução
```

Paralelamente, secrets e credenciais deverão possuir gerenciamento e controle de acesso apropriados, incluindo a utilização do OpenBao quando aplicável.

---

## 9. Critérios de controle humano

Ao longo da POC, quatro critérios serão tratados como transversais:

### Rastreabilidade

É possível identificar a origem e as alterações relevantes do artefato?

### Confiabilidade

Existem evidências independentes de que o artefato atende aos critérios definidos?

### Compreensibilidade

A pessoa responsável consegue explicar e operar aquilo que foi incorporado?

### Evolução

O artefato pode ser atualizado e mantido de forma controlada ao longo do tempo?

Esses critérios não significam que toda decisão deva ser executada manualmente. O objetivo é evitar que automação seja confundida com ausência de responsabilidade ou controle.

---

## 10. Escopo

Serão analisadas principalmente boas práticas relacionadas a:

- requisitos e especificação;
- código;
- secrets;
- dependências e bibliotecas;
- atualidade e origem de componentes;
- Git;
- CI/CD;
- Docker;
- Kubernetes;
- permissões;
- gestão de credenciais;
- OpenBao;
- logs;
- rastreabilidade;
- validação independente;
- configurações produzidas ou modificadas com auxílio de IA.

O foco estará nos pequenos detalhes capazes de aumentar a superfície de ataque mesmo quando a aplicação continua funcionando normalmente.

---

## 11. Fora do escopo

A POC não pretende:

- criar uma nova Inteligência Artificial;
- treinar modelos;
- analisar a arquitetura interna de um LLM;
- provar que toda saída produzida por IA é insegura;
- provar que desenvolvimento humano é sempre mais seguro;
- substituir desenvolvedores;
- eliminar o uso de IA na revisão;
- substituir completamente revisão humana;
- implementar todos os controles existentes de cibersegurança;
- construir uma infraestrutura completa de produção;
- eliminar todos os riscos possíveis.

O objetivo é demonstrar **boas práticas aplicáveis e reproduzíveis**.

---

## 12. Hipótese

A hipótese da POC é:

**A aplicação sistemática de boas práticas de Security by Design durante todo o ciclo de desenvolvimento assistido por IA — começando nos requisitos e incluindo minimização, validação independente, rastreabilidade e controle de dependências — permite identificar problemas antes que eles se acumulem e ampliem silenciosamente a superfície de ataque ou reduzam a capacidade humana de compreender e operar o sistema.**

---

## 13. Mensagem principal da POC

A POC será construída sobre quatro ideias:

**Funcionar não significa estar seguro.**

**A segurança também está nos pequenos detalhes.**

**Não basta perguntar se funciona. É preciso perguntar se precisa existir.**

**IA auxilia. Boas práticas orientam. Segurança valida.**

---

## 14. Resultado do Passo 1

Ao final deste passo ficam definidos:

- o tema central;
- o problema;
- a motivação;
- o objetivo geral;
- os objetivos específicos;
- os princípios orientadores;
- o papel da IA;
- os critérios de controle humano;
- o escopo;
- o que está fora do escopo;
- a hipótese que a POC pretende avaliar.

O próximo passo será transformar esses conceitos em uma **matriz de boas práticas**, relacionando cada prática ao risco que pretende reduzir, ao detalhe técnico observado, à forma de validação e à evidência produzida.
