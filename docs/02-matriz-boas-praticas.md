# Passo 2 — Matriz de Boas Práticas

## 1. Objetivo desta etapa

Esta etapa transforma os princípios definidos no Passo 1 em controles técnicos que possam ser demonstrados, validados e reproduzidos.

A POC não utilizará ferramentas apenas para aumentar a quantidade de tecnologias envolvidas. Cada controle deverá responder a um risco concreto.

A lógica de validação será:

```text
Detalhe inseguro
      |
      v
Risco gerado
      |
      v
Detecção
      |
      v
Correção
      |
      v
Nova validação
      |
      v
Evidência
```

A pergunta para cada componente será:

> **Este componente é necessário? Se for, qual superfície de ataque ele adiciona e como ela será reduzida?**

---

## 2. Critério de prioridade

Os controles serão classificados em três níveis:

- **P0 — Essencial:** deve fazer parte da primeira demonstração funcional da POC.
- **P1 — Importante:** será incorporado após o fluxo principal estar funcionando.
- **P2 — Evolução:** poderá ser adicionado posteriormente sem bloquear a primeira versão.

O primeiro objetivo é construir uma POC pequena, reproduzível e capaz de produzir evidências claras.

---

## 3. Matriz inicial

| Prioridade | Camada | Pequeno detalhe observado | Risco | Boa prática | Validação proposta | Evidência |
|---|---|---|---|---|---|---|
| P0 | IA | Secret, token ou dado interno enviado no prompt | Exposição de informação sensível a um serviço externo | Nunca fornecer credenciais ou dados sensíveis reais à IA | Checklist e exemplos controlados de prompt seguro/inseguro | Comparação documentada dos prompts |
| P0 | Git | Secret incluído em arquivo versionado | Vazamento de credenciais pelo histórico Git | Não versionar secrets; utilizar variáveis e secret stores | Gitleaks | Relatório antes/depois |
| P0 | Docker | Container executado como root | Maior impacto em caso de comprometimento | Executar processo com usuário não privilegiado | Dockerfile + `docker inspect` + Hadolint | UID do processo e análise do Dockerfile |
| P0 | Docker | Imagem ou dependência com vulnerabilidade conhecida | Uso de componente vulnerável | Verificar imagens e dependências antes do deploy | Trivy | Relatório de vulnerabilidades |
| P0 | Docker / Rede | Porta exposta sem necessidade | Aumento desnecessário da superfície de ataque | Expor somente portas realmente utilizadas | Revisão do Dockerfile/Compose e inspeção do container | Lista de portas antes/depois |
| P0 | CI/CD | Pipeline permite que falhas críticas avancem | Artefato inseguro alcança etapas seguintes | Criar security gates para falhas definidas pela POC | Pipeline CI com retorno diferente de zero | Execução aprovada e execução bloqueada |
| P0 | Kubernetes | Pod executado como root ou privilegiado | Escalada de privilégios e maior impacto no cluster | `runAsNonRoot`, restrição de privilégios e capabilities | KubeLinter ou Kubescape + revisão do manifest | Relatório e manifest corrigido |
| P0 | Kubernetes | ServiceAccount token montado sem necessidade | Credencial do cluster disponível para aplicação que não precisa dela | Desabilitar automount quando não necessário | Revisão de YAML + validação do manifest | YAML antes/depois |
| P0 | Secrets | Credencial fixa armazenada em arquivo/configuração | Vazamento e dificuldade de rotação | Centralizar secrets e recuperar credenciais em tempo de execução | OpenBao | Secret removido do código e recuperação controlada |
| P1 | CI/CD | Token do pipeline com permissões amplas | Comprometimento do repositório ou pipeline com privilégios excessivos | Aplicar menor privilégio às permissões do workflow | Revisão de `permissions` no workflow | Workflow antes/depois |
| P1 | Dependências | Dependência desnecessária incluída no projeto | Aumento da superfície de ataque e manutenção | Remover componentes não utilizados | Inventário + análise de dependências | Lista antes/depois |
| P1 | Kubernetes | Container com filesystem gravável sem necessidade | Persistência/modificação indevida dentro do container | Utilizar filesystem somente leitura quando aplicável | Manifest + validação em execução | Configuração e teste |
| P1 | Logs | Token, senha ou dado sensível registrado em log | Vazamento indireto de informação | Sanitizar e minimizar logs | Teste controlado e inspeção dos logs | Log inseguro vs. log corrigido |
| P1 | Imagens | Tag genérica como `latest` | Build/deploy não determinístico | Utilizar versão explícita ou digest quando aplicável | Revisão automática/manual | Manifest/Dockerfile corrigido |
| P2 | Políticas | Regras de segurança dependem apenas de revisão manual | Inconsistência entre revisões | Transformar controles estáveis em policy as code | Conftest/OPA quando necessário | Política e resultado da validação |

---

## 4. Controles selecionados para o MVP

A primeira versão da POC deverá demonstrar somente os controles **P0**.

Isso significa que o MVP terá os seguintes pontos:

1. uso seguro de IA sem exposição de secrets;
2. detecção de secrets no Git;
3. container executado sem root;
4. análise de vulnerabilidades da imagem;
5. minimização de portas expostas;
6. security gate no CI/CD;
7. restrições básicas de segurança no Kubernetes;
8. redução do uso desnecessário de ServiceAccount tokens;
9. gestão de secrets com OpenBao.

Esses controles foram escolhidos porque atravessam o fluxo completo da POC e permitem demonstrar a diferença entre uma aplicação que apenas funciona e uma aplicação que funciona com decisões de segurança incorporadas.

---

## 5. Ferramentas candidatas

A POC utilizará ferramentas somente quando elas tiverem uma função clara dentro do controle.

### Gitleaks

Função:

- detectar credenciais e secrets presentes no repositório ou histórico Git.

Justificativa:

- atende diretamente ao risco de secrets versionados.

### Hadolint

Função:

- analisar boas práticas e problemas comuns em Dockerfiles.

Justificativa:

- permite detectar decisões inseguras ou inadequadas antes da construção da imagem.

### Trivy

Função:

- analisar imagens, dependências e configurações em busca de vulnerabilidades e problemas conhecidos.

Justificativa:

- reduz a necessidade de múltiplas ferramentas na primeira versão da POC.

### KubeLinter ou Kubescape

Função:

- verificar configurações inseguras em manifests Kubernetes.

Justificativa:

- permite automatizar a detecção de problemas como execução privilegiada, ausência de restrições e configurações excessivamente permissivas.

A POC deverá escolher **uma** dessas ferramentas para o MVP, evitando duplicação de função.

### OpenBao

Função:

- gerenciamento centralizado de secrets e controle de acesso às credenciais.

Justificativa:

- demonstra a substituição de credenciais fixas em arquivos por recuperação controlada em tempo de execução.

---

## 6. Regra para adoção de ferramentas

Antes de adicionar qualquer nova ferramenta à POC, deverão ser respondidas quatro perguntas:

1. Qual risco concreto ela reduz?
2. Já existe outra ferramenta fazendo a mesma função?
3. O novo componente aumenta a superfície de ataque ou a complexidade operacional?
4. A evidência produzida por ele é necessária para demonstrar a hipótese da POC?

Se a ferramenta não tiver uma função clara, ela não deverá ser adicionada.

---

## 7. Estrutura dos experimentos

Cada controle será demonstrado utilizando o mesmo formato.

### Estado inseguro

Será criado um exemplo propositalmente simples contendo uma configuração insegura.

### Detecção

A POC deverá demonstrar como o problema pode ser percebido por revisão, ferramenta ou política.

### Correção

O artefato será modificado aplicando a boa prática correspondente.

### Revalidação

A mesma validação será executada novamente.

### Evidência

Serão armazenados os resultados necessários para comparar o comportamento antes e depois.

O padrão será:

```text
INSEGURO
   |
   v
DETECTADO
   |
   v
CORRIGIDO
   |
   v
VALIDADO
```

---

## 8. Evidências esperadas

A POC deverá priorizar evidências simples e auditáveis, como:

- saída de ferramentas de análise;
- logs do pipeline;
- manifests antes e depois;
- Dockerfiles antes e depois;
- resultado de `docker inspect`;
- resultado de validações Kubernetes;
- registro de um pipeline bloqueado;
- registro do mesmo pipeline aprovado após a correção;
- demonstração de secret fora do repositório e recuperado pelo OpenBao;
- documentação explicando o risco e a correção.

---

## 9. O que esta etapa evita

A matriz foi criada também para evitar alguns desvios comuns:

- adicionar scanners sem uma hipótese clara;
- tratar a quantidade de ferramentas como indicador de segurança;
- transformar a POC em uma infraestrutura de produção completa;
- criar controles que não possam ser demonstrados;
- adicionar Kubernetes, OpenBao ou CI/CD apenas por aparência técnica;
- perder o foco nos pequenos detalhes que ampliam a superfície de ataque.

---

## 10. Resultado do Passo 2

Ao final desta etapa ficam definidos:

- os riscos que serão demonstrados;
- os controles correspondentes;
- a prioridade de cada controle;
- as ferramentas candidatas;
- o formato dos experimentos;
- as evidências que deverão ser produzidas;
- o conjunto mínimo que fará parte do MVP.

O próximo passo será definir a **arquitetura mínima da POC**, especificando quais componentes realmente precisam existir e como eles se relacionam sem adicionar complexidade desnecessária.
