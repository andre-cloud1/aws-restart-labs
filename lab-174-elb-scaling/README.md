# Lab 19: Dimensionar e balancear a carga da arquitetura (ELB + ASG)

Este laboratório é um dos mais importantes, pois implementa o padrão de arquitetura fundamental da AWS para **Alta Disponibilidade (HA)** e **Escalabilidade**. O projeto transforma uma aplicação de servidor único em uma solução resiliente que se adapta automaticamente à demanda.

## 🏛️ Arquitetura: Antes e Depois da Escalabilidade

A mudança na arquitetura é drástica, movendo de uma falha única para uma solução robusta.

### Arquitetura Inicial (Antes)
O "Antes" é um único Servidor Web em uma **Sub-rede Pública**.
* **Problemas:** É um ponto único de falha (se a instância cair, o site sai do ar), não escala com o tráfego e o servidor fica perigosamente exposto na internet.

### Arquitetura Final (Depois)
O "Depois" é uma arquitetura desacoplada e segura:
* Um **Application Load Balancer (ALB)** é colocado nas **Sub-redes Públicas** para ser o único ponto de entrada do tráfego.
* Um **Auto Scaling Group (ASG)** é criado para lançar instâncias em múltiplas Zonas de Disponibilidade.
* **MELHOR PRÁTICA DE SEGURANÇA:** As instâncias do Servidor Web agora rodam em **Sub-redes Privadas**, protegidas do acesso direto da internet.

![Diagrama Antes e Depois da Arquitetura](./arquitetura-antes-e-depois-scaling.png)

---

## 🎯 Objetivo
Com base nos objetivos do lab, o foco era:
* Criar uma **Amazon Machine Image (AMI)** de uma instância EC2 existente (para criar um "modelo").
* Criar um **Application Load Balancer (ALB)**.
* Criar um **Launch Template** (Modelo de Lançamento) e um **Auto Scaling Group (ASG)**.
* Configurar o ASG para lançar novas instâncias nas **sub-redes privadas**.
* Usar **Alarmes do CloudWatch** para monitorar a performance e disparar o auto-scaling.

## 🛠️ Tarefas Realizadas

Para construir esta arquitetura, eu:

* **1. Criei a "Golden Image" (AMI):**
    * Criei uma AMI (Amazon Machine Image) a partir da instância "Web Server 1" original. Esta AMI serve como um "molde" que contém a aplicação web já instalada.

* **2. Criei o Modelo de Lançamento:**
    * Criei um **Launch Template** (Modelo de Lançamento) baseado na AMI. Este template define *exatamente* como cada nova instância EC2 deve ser lançada (tipo de instância, AMI, Security Group, etc.).

* **3. Configurei o Load Balancer (ALB):**
    * Provisionei um **Application Load Balancer** (ALB) do tipo "internet-facing".
    * Configurei o ALB para "ouvir" na porta 80 (HTTP) e ativei-o nas **duas Sub-redes Públicas** (Zona A e Zona B) para alta disponibilidade.
    * Criei um **Target Group** (Grupo de Destino) que o ALB usará para encaminhar o tráfego.

* **4. Configurei o Auto Scaling Group (ASG):**
    * Criei o **Auto Scaling Group**, vinculando-o ao **Launch Template** e ao **Target Group** do ALB.
    * Configurei o ASG para lançar instâncias nas **duas Sub-redes Privadas** (Zona A e Zona B).
    * Defini uma capacidade desejada (ex: 2 instâncias) e uma máxima (ex: 4).
    * Configurei **Políticas de Scaling** (ex: "Adicionar 1 instância se o uso de CPU for > 70% por 5 minutos").

* **5. Configurei os Alarmes (CloudWatch):**
    * Criei os **Alarmes do CloudWatch** (ex: CPUUtilization > 70%) e os vinculei às Políticas de Scaling do ASG.

* **6. Configurei a Segurança (Security Groups):**
    * **SG do ALB:** Permite HTTP (porta 80) de qualquer lugar (`0.0.0.0/0`).
    * **SG das Instâncias:** Permite tráfego na porta 80 **APENAS** a partir do Security Group do ALB. (Isso tranca as instâncias privadas).

## 💡 Conceitos Aprendidos
-   **Alta Disponibilidade (HA):** Usar múltiplas Zonas de Disponibilidade (AZs), tanto para o ALB quanto para o ASG.
-   **Escalabilidade Elástica:** O ASG adiciona (scale-out) e remove (scale-in) instâncias automaticamente com base na demanda.
-   **Auto-reparação (Self-healing):** Se uma instância falhar, o ASG automaticamente a encerra e lança uma nova e saudável.
-   **Desacoplamento:** O ALB atua como um "maestro" na frente, permitindo que as instâncias de back-end sejam escaláveis e substituíveis.
-   **Padrão de Segurança de VPC:** Esta é a lição mais importante. **Load Balancers em sub-redes públicas, Servidores de Aplicação em sub-redes privadas.**

## 📸 Minhas Provas (Screenshots)

*(Aqui vou adicionar meus próprios screenshots mostrando o ASG em ação, a política do CloudWatch disparando e, o mais importante, acessando o site através da URL do Load Balancer.)*
