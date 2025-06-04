# Etapa 2 – Configuração e Execução do Amazon EKS

Nesta etapa do projeto, utilizamos o **Amazon Elastic Kubernetes Service (EKS)** como solução de orquestração para containerização da aplicação. O EKS é um serviço gerenciado que facilita a criação, o gerenciamento e a escalabilidade de clusters Kubernetes, permitindo foco total na aplicação e não na infraestrutura subjacente.

---

## Objetivo da Etapa

O principal objetivo foi criar um ambiente gerenciado de Kubernetes na AWS, capaz de receber a aplicação Spring Boot via container, garantindo escalabilidade, balanceamento de carga e integração com os serviços já configurados.

---

## O que foi feito

### ✅ Criação do Role para o Cluster EKS

Foi criado um **papel IAM específico** com as permissões necessárias para permitir que o Amazon EKS gerencie os recursos do cluster. Essa role foi associada ao serviço `eks.amazonaws.com`, garantindo que o plano de controle tivesse autoridade para criar e controlar recursos em nome do cluster.

---

### ✅ Provisionamento de uma VPC dedicada

A infraestrutura foi isolada em uma **VPC (Virtual Private Cloud)** própria, permitindo controle granular de sub-redes, grupos de segurança e rotas de acesso. A VPC foi gerada automaticamente com as configurações recomendadas pela AWS para ambientes EKS, incluindo:

- Sub-redes públicas e privadas;
- Tabelas de rotas;
- Gateways NAT e internet.

Essa rede serviu como base para todos os recursos criados no cluster.

---

### ✅ Criação do Cluster EKS

Com a VPC e o role do cluster prontos, criamos o **cluster EKS** nomeado e vinculado à região `us-west-2`. O cluster foi criado com a role apropriada e associado às sub-redes da VPC.

Durante esse processo, não foi necessário se preocupar com a criação do plano de controle, já que o EKS gerencia esse componente automaticamente com alta disponibilidade, backups e atualizações automáticas.

---

### ✅ Criação da Role para o Node Group

Foi criada uma **nova role IAM**, desta vez para o **grupo de nós (node group)**. Essa role permitiu que as instâncias EC2 que compõem os nós do cluster executassem tarefas como:

- Acesso ao container registry (ECR);
- Comunicação com o plano de controle Kubernetes;
- Coleta de métricas e logs.

As políticas principais associadas foram:
- `AmazonEKSWorkerNodePolicy`
- `AmazonEC2ContainerRegistryReadOnly`
- `AmazonEKS_CNI_Policy`

---

### ✅ Definição do Node Group

Criamos um **node group gerenciado**, especificando:

- Tipo de instância: `t3.medium` (2 vCPUs, 4GB RAM);
- Quantidade mínima e máxima de nós;
- Sistema operacional: Amazon Linux 2;
- Integração com Auto Scaling para escalabilidade automática;

Esse grupo de nós passou a atuar como plano de dados, executando os containers implantados no cluster.

---

### ✅ Acesso ao Cluster EKS

Com o cluster em execução, configuramos o acesso via `kubectl` através da AWS CLI. Foi possível listar, aplicar e monitorar recursos no cluster diretamente pelo terminal, integrando os microsserviços da aplicação.

Também foi feita a autenticação do `kubectl` com o `aws eks update-kubeconfig`, garantindo controle completo sobre o namespace padrão do Kubernetes.

---

## Resultado

Ao final desta etapa, tínhamos um ambiente Kubernetes funcional, escalável e gerenciado pela AWS. A aplicação pôde ser implantada com comandos padrão `kubectl`, e os serviços foram expostos via Load Balancer automaticamente.

Esse cluster se tornou a base da operação do projeto, sendo fundamental para os testes de carga e análise de custos executados nas etapas seguintes.

---

## Observações Técnicas

- A criação dos recursos foi feita utilizando uma combinação do Console AWS e comandos `eksctl`/`kubectl`;
- O cluster foi configurado com permissões mínimas necessárias, evitando riscos de segurança;
- Monitoramos os recursos do cluster por meio da interface gráfica do EKS e do terminal.

---



# Video mostrando a aplicação rodando na AWS EKS
Veja o vídeo no YouTube: [https://www.youtube.com/watch?v=KIOW6v2Fwws](https://www.youtube.com/watch?v=KIOW6v2Fwws)