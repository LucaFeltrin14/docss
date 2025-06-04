# Projeto de Deploy em Nuvem com AWS, Kubernetes (EKS) e mais

#### Dupla: Eduardo Takei e Luca Feltrin

Este projeto teve como objetivo implementar um ambiente completo de deploy em nuvem para uma aplicação Spring Boot utilizando a infraestrutura da Amazon Web Services (AWS), orquestrada com Kubernetes via Amazon EKS. Além disso, foram realizados testes de carga, integração (parcial) com Jenkins para CI/CD, uma análise de custos detalhada e a criação de uma apresentação de storytelling do projeto.

*Observação*: Utilizamos o repostório para dar continuidade para a parte em grupo.
- [Plataform Project](https://github.com/EduardoTakeiYaginuma/platafformProject)

*Observação*: Vídeos no final de cada página.

---

## Visão Geral

A computação em nuvem revolucionou o desenvolvimento e a entrega de software. Com a AWS, temos acesso sob demanda a serviços de infraestrutura escaláveis, seguros e altamente disponíveis. Este projeto explorou essa realidade ao:

- Prover infraestrutura com **Amazon EC2 e EKS**;
- Executar deploy automatizado de microsserviços com **Docker** e **Kubernetes**;
- Tentar pipeline CI/CD com **Jenkins**;
- Monitorar escalabilidade com **HPA (Horizontal Pod Autoscaler)**;
- Simular carga real de usuários;
- Estimar custos com **AWS Pricing Calculator**;
- Criar uma apresentação para stakeholders.

---

## ✔️ Resumo das Entregas

| Entregável     | Status       | Comentário |
|----------------|--------------|------------|
| **Configuração da AWS** | ✅ Concluído | Provisionamento, EC2, IAM, EKS |
| **EKS e Deploy da Aplicação** | ✅ Concluído | Aplicação Spring Boot publicada e acessível via LoadBalancer |
| **Testes de Carga** | ✅ Concluído | Escalonamento automático testado com HPA |
| **CI/CD com Jenkins** | ⚠️ Parcial | Pipeline configurado até DockerHub, deploy no EKS não automatizado |
| **Análise de Custos** | ✅ Concluído | Estimativas detalhadas com AWS Calculator |
| **Apresentação e Storytelling** | ✅ Concluído | Slides e vídeo explicativo produzidos |
| **Exploração de PaaS** | ❌ Não Implementado | Não foi abordado no escopo atual |

---

## AWS – Amazon Web Services

A **AWS** foi a espinha dorsal da nossa infraestrutura. Utilizamos:

- **IAM (Identity and Access Management)**: para criar usuários com políticas restritas;
- **EC2**: para rodar instâncias no plano de controle e simular cargas;
- **EKS (Elastic Kubernetes Service)**: para gerenciar os containers com alta disponibilidade;
- **Elastic Load Balancer**: para expor a aplicação ao mundo externo;
- **CloudWatch** (implícito): para monitoramento e logs;
- **Cost Explorer / Budgets**: para controle e simulação de gastos.

A escolha pela AWS se deve à sua robustez, documentação extensa e liderança de mercado.

---

## Kubernetes com Amazon EKS

O **Amazon EKS** permitiu que focássemos no deploy da aplicação sem nos preocupar com os detalhes do plano de controle do Kubernetes.

### Passos realizados:

1. Criação do cluster EKS com permissões apropriadas;
2. Deploy da aplicação Spring Boot com `kubectl apply`;
3. Exposição do serviço com `type: LoadBalancer`;
4. Acompanhamento de pods, logs e health checks;
5. Uso do **Horizontal Pod Autoscaler** para escalar conforme CPU.

### Exemplo de comando HPA:

```bash
kubectl autoscale deployment gateway --cpu-percent=50 --min=1 --max=10
