# Lab 02: Introdução ao Amazon EC2

Este laboratório do AWS re/Start foi focado nos fundamentos do Amazon Elastic Compute Cloud (EC2).

## 🎯 Objetivo
O objetivo principal era lançar, gerenciar e monitorar uma instância EC2 que funcionasse como um servidor web básico.

## 🛠️ Tarefas Realizadas

Eu completei as seguintes tarefas, com base nas imagens do guia:

* **Task 1: Launching your EC2 instance (Passo 1-8)**
    * Lancei uma instância `t3.micro` com Amazon Linux 2023.
    * Configurei um Security Group (`Web Server Security group`).
    * Usei o campo "User Data" para instalar um servidor web Apache (`httpd`) e criar uma página `index.html` simples via script.

* **Task 2: Monitor Your Instance**
    * Monitorei os "Status checks" da instância até passarem.
    * Verifiquei o "instance screenshot" para ver o progresso do boot.

* **Task 3: Update Your Security Group and Access the Web Server**
    * Tentei acessar o IP público da instância (o que falhou).
    * Identifiquei que o Security Group precisava de uma regra "inbound" para liberar tráfego HTTP na porta 80.
    * Adicionei a regra e acessei com sucesso a página "Hello From Your Web Server!".

* **Task 4: Resize Your Instance**
    * Parei a instância.
    * Altei o tipo da instância (ex: de `t3.micro` para `t3.small`).
    * Aumentei o tamanho do Volume EBS.
    * Reiniciei a instância para aplicar as mudanças.

* **Task 5: Test Termination Protection**
    * Habilitei a "Termination Protection" na instância.
    * Tentei terminar a instância (o que falhou, como esperado).
    * Desabilitei a proteção e terminei a instância com sucesso.

## 💡 Conceitos Aprendidos
- Lançamento e configuração de instâncias EC2.
- A importância dos Security Groups como firewall.
- O poder do "User Data" para automatizar a inicialização.
- Como redimensionar recursos (scale-up/down).
- Mecanismos de proteção (Termination Protection) para evitar acidentes.

## 📸 Provas (Screenshots)

Abaixo estão os prints do meu console:

![Meu Servidor Web Rodando](./nome-do-seu-print-1.png)
![Regra do Security Group](./nome-do-seu-print-2.png)

*(Lembre-se de trocar 'nome-do-seu-print-1.png' pelo nome exato do arquivo que você subiu na Ação 2)*
