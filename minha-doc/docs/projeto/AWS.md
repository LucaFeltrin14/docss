# Etapa 1 – Configuração da AWS

Nesta primeira etapa do projeto, realizamos toda a configuração necessária para preparar a infraestrutura em nuvem utilizando a Amazon Web Services (AWS), que foi a base para o provisionamento dos serviços e do cluster Kubernetes.

---

## Visão Geral

A AWS foi escolhida como plataforma de infraestrutura por sua escalabilidade, confiabilidade e integração nativa com serviços de orquestração como o Amazon EKS. Essa escolha permitiu que o projeto explorasse os principais conceitos de infraestrutura como serviço (IaaS) e container orchestration com segurança e performance.

---

## O que foi feito

### ✅ Criação e Configuração da Conta AWS

Iniciamos criando e ativando uma conta na AWS, com vinculação de um cartão de crédito e ativação do acesso completo ao console. A conta foi configurada na região **us-west-2 (Oregon)**, escolhida por sua boa cobertura de serviços e latência estável.

---

### ✅ IAM: Gestão de Usuários e Permissões

Criamos um usuário específico para o projeto com permissões programáticas. Atribuímos políticas necessárias para manipular recursos do EKS, EC2, VPC e ECR, garantindo acesso seguro via CLI. As chaves de acesso foram geradas e utilizadas para autenticação local via terminal.

---

### ✅ AWS CLI

Configuramos o ambiente local com a **AWS Command Line Interface (CLI)**, o que permitiu realizar todas as operações da nuvem diretamente pelo terminal, incluindo criação de cluster, monitoramento e manipulação de recursos.

A CLI foi vinculada ao usuário IAM criado anteriormente, e todos os comandos foram executados autenticados por esse canal.

---

### ✅ Preparação para o EKS

Como parte da preparação para o cluster Kubernetes, instalamos ferramentas auxiliares como `kubectl` e `eksctl`, utilizadas nas etapas seguintes do projeto. Com essas ferramentas, garantimos que seria possível gerenciar o cluster sem depender exclusivamente do console web.

---

### ✅ Organização de recursos e boas práticas

Durante a configuração inicial da AWS, adotamos algumas boas práticas:

- Uso de **nomes padronizados** para todos os recursos (ex: clusters, instâncias, políticas);
- Controle de acesso refinado via IAM;
- **Eliminação de recursos não utilizados** para evitar cobranças inesperadas;
- Registros de comandos e ações salvos em logs locais para documentação e rastreabilidade.

---

## Resultado

Ao final desta etapa, a infraestrutura base estava pronta para suportar a criação do cluster Kubernetes, deploy da aplicação e integração com ferramentas de CI/CD. A configuração da AWS foi estável, funcional e seguiu as recomendações de segurança e governança da própria plataforma.

---

## Observações

A configuração realizada nesta etapa foi fundamental para o sucesso das etapas seguintes. Sem ela, não seria possível:

- Provisionar o cluster EKS;
- Realizar testes de carga em ambiente escalável;
- Implantar um pipeline de CI/CD integrado com serviços de nuvem;
- Estimar custos com base em uso real da infraestrutura.

---

