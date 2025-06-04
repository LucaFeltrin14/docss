# Etapa 3 – Testes de Carga com Escalonamento Horizontal (HPA)

Uma etapa essencial do projeto foi a execução de **testes de carga** para verificar a capacidade da aplicação de lidar com variações de tráfego em tempo real. Através do uso de Kubernetes e do recurso **HPA (Horizontal Pod Autoscaler)**, conseguimos simular cenários de uso intenso e validar o comportamento do ambiente em situações de alta demanda.

---

## Objetivo da Etapa

O principal objetivo foi comprovar que a aplicação, ao ser submetida a um alto volume de requisições, seria capaz de escalar horizontalmente, aumentando o número de pods disponíveis para atender à demanda, e reduzir esses pods quando a carga diminuísse.

---

## O que foi feito

### ✅ Implementação do HPA (Horizontal Pod Autoscaler)

Foi criado um recurso do Kubernetes para o deployment `gateway`, com parâmetros específicos de escalabilidade baseados no uso de CPU:

- **CPU target:** 50%
- **Mínimo de pods:** 1
- **Máximo de pods:** 10

Esse autoscaler foi vinculado diretamente ao deployment ativo da aplicação no cluster EKS, permitindo que o Kubernetes monitorasse o consumo de recursos e tomasse decisões automáticas de escalonamento.

---

### ✅ Execução dos Testes de Carga

Para gerar carga artificial na aplicação, executamos um script de requisições contínuas ao endpoint `/info` da aplicação, exposto pelo gateway.

As requisições foram feitas com intervalos curtos (0.01 segundos), simulando um tráfego intenso de múltiplos usuários simultâneos. A ferramenta utilizada foi o próprio `wget`, executado em loop dentro de um pod `busybox` temporário.

O endpoint `/info` foi escolhido por ser leve, rápido de responder e ideal para simulações de carga em ambientes REST.

---

### ✅ Monitoramento em Tempo Real

Enquanto o teste de carga estava em execução, monitoramos:

- A resposta do **HPA**, observando o número de réplicas sendo ajustado automaticamente conforme o uso de CPU;
- A **criação de novos pods** pelo deployment do `gateway`;
- A **redução gradual** dos pods após o encerramento da carga.

Essa observação foi feita em tempo real, com janelas de terminal dedicadas para:

- Verificar o status do HPA com `kubectl get hpa`;
- Ver o escalonamento dos pods com `kubectl get pods -w`;
- Executar e pausar o gerador de carga com `kubectl run`.

---

### ✅ Demonstração em Vídeo

Gravamos um vídeo demonstrando o processo completo, incluindo:

1. Criação e visualização do HPA;
2. Execução do gerador de carga;
3. Acompanhamento da criação de réplicas;
4. Redução automática após fim da carga;
5. Conclusão com a exclusão do HPA para liberação de recursos.

Esse vídeo foi incluído como parte da apresentação final do projeto.

---

## Resultado

O ambiente se comportou conforme o esperado. Durante os testes:

- O número de pods do `gateway` aumentou automaticamente quando o consumo de CPU ultrapassou o limite configurado;
- O tráfego contínuo foi distribuído entre os pods disponíveis, sem queda de performance;
- Após a interrupção do teste, o HPA começou a reduzir o número de réplicas gradualmente, estabilizando a infraestrutura.

Essa etapa validou a capacidade do nosso cluster em responder elasticamente à variação de carga, um requisito essencial para aplicações modernas em nuvem.

---

## Observações Técnicas

- Utilizamos o comando `kubectl autoscale` para criar o HPA;
- A carga foi gerada via `wget` dentro de um pod `busybox`;
- O endpoint utilizado (`/info`) foi suficiente para gerar atividade contínua de CPU;
- A performance do HPA demonstrou alinhamento com as expectativas teóricas;
- Nenhum recurso apresentou falha ou erro durante a execução do teste.

---

## Conclusão

A etapa de teste de carga foi bem-sucedida e provou que nossa aplicação, implantada no EKS, está preparada para lidar com variações de tráfego. A estratégia de escalonamento automático demonstrou ser eficaz, consolidando a robustez do ambiente construído.

---

## Video explicando a estimação de teste de carga
Veja o vídeo no YouTube: [https://youtu.be/uAhrSJhylSg](https://youtu.be/uAhrSJhylSg)
