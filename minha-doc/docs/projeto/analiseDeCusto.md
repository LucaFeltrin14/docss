# Etapa 4 – Análise de Custos na AWS

Nesta etapa do projeto, realizamos uma análise detalhada dos custos envolvidos na infraestrutura provisionada na AWS. O objetivo foi estimar o impacto financeiro das decisões arquiteturais, entender quais serviços representam os maiores gastos e documentar o uso consciente de recursos na nuvem.

---

## Objetivo da Etapa

A análise de custos foi essencial para garantir que o ambiente construído fosse sustentável, mesmo em uso prolongado. Também permitiu identificar possíveis otimizações para reduzir gastos sem comprometer a qualidade e a escalabilidade da aplicação.

---

## O que foi feito

### ✅ Estimativa com a AWS Pricing Calculator

Utilizamos a ferramenta oficial **AWS Pricing Calculator** para projetar o custo mensal de cada serviço utilizado no projeto. A simulação considerou:

- **Cluster EKS com 1 nó ativo**
- **Instância EC2 do tipo `t3.medium`**
- **Uso de Load Balancer para expor a aplicação**
- **Tráfego de saída estimado para simulação de uso real**

Os valores foram estimados em dólares, com base na região **us-west-2 (Oregon)**.

---

### ✅ Avaliação de Custos dos Principais Serviços

#### 🧩 EKS (Elastic Kubernetes Service)
- **Custo fixo mensal** do cluster: aproximadamente **$73** (para o plano de controle).
- **Custos adicionais** relacionados ao uso de instâncias EC2 no Node Group.

#### 🖥️ EC2 (Node Group – `t3.medium`)
- Instância com 2 vCPUs e 4GB de RAM;
- **Custo estimado**: cerca de **$30 por mês** por instância em uso contínuo;
- Apenas uma instância foi utilizada para simulação do ambiente produtivo.

#### 🌐 Load Balancer (ELB)
- Utilizado para expor o serviço do gateway;
- **Custo variável**, dependendo do tempo ativo e tráfego roteado;
- Estimamos **cerca de $18 por mês** com base na média de horas ativas e volume de dados.

#### 📡 Tráfego de Rede
- Tráfego de saída simulado com base em testes de carga;
- Faixa gratuita utilizada parcialmente;
- Estimativa conservadora: **$1 a $3 mensais** para um tráfego leve.

---

### ✅ Custos Totais Estimados

| Serviço               | Estimativa Mensal (USD) |
|-----------------------|-------------------------|
| EKS Cluster (control plane) | $73.00                  |
| EC2 (1 x t3.medium)          | $30.00                  |
| **Total estimado**          | **$88.00/mês**         |

> Obs.: Esses valores são estimativas baseadas em uso contínuo. Em ambientes de produção real, custos podem variar significativamente.

---

### ✅ Consideração de RDS

Embora o **Amazon RDS** não tenha sido implantado no projeto, consideramos uma estimativa para seu uso com instância mínima para PostgreSQL:

- **Instância db.t3.micro**
- **Uso leve e contínuo**
- **Estimativa:** aproximadamente **$15 a $20 por mês**

Essa simulação foi incluída no plano de custos para projetar possíveis expansões futuras da arquitetura.

---

### ✅ Ferramentas Utilizadas

- **AWS Pricing Calculator**: para estimativas realistas e combinadas;
- **AWS Cost Explorer**: para visualizar consumo real durante a execução dos testes;
- **Planilhas** auxiliares para simular diferentes cenários de escalabilidade e uso intensivo.

---

## Resultado

A análise de custos foi concluída com sucesso, fornecendo uma visão clara do impacto financeiro do ambiente implantado. A simulação mostrou que, mesmo com um cluster funcional e escalável, é possível manter os custos controlados com boas práticas, desligando recursos fora de uso e dimensionando corretamente os serviços.

---

## Observações Técnicas

- Todos os custos foram analisados com base em uso contínuo (24/7) para representar o pior cenário;
- O uso de instâncias **t3.medium** foi considerado um bom equilíbrio entre custo e performance;
- A eliminação de recursos após os testes contribuiu para manter o custo real próximo de zero.

---

## Conclusão

A etapa de análise de custos permitiu avaliar a viabilidade do projeto na AWS. O entendimento detalhado dos preços e o uso consciente da infraestrutura são fatores críticos para qualquer aplicação moderna que opere na nuvem.

Essa visão também preparou o terreno para tomada de decisões futuras sobre otimização, uso de PaaS ou migração de serviços específicos para modelos mais econômicos.

---


## Video explicando a estimação de custos
Veja o vídeo no YouTube: [https://www.youtube.com/watch?v=IADculDt_II](https://www.youtube.com/watch?v=IADculDt_II)
