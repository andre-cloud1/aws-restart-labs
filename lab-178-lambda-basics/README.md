# Lab 09: Trabalhar com o AWS Lambda

Este laboratório foca na computação *serverless*, implantando uma solução baseada em AWS Lambda para gerar e enviar relatórios de vendas automaticamente.

## 🏛️ Arquitetura Implementada

A solução é uma arquitetura orientada a eventos. Ela é acionada por um agendamento, orquestra duas funções Lambda para processar dados de um banco de dados e, finalmente, notifica um administrador por e-mail usando o SNS.

![Diagrama da Arquitetura Serverless](./arquitetura-serverless-lambda.png)

---

## ⚙️ Fluxo de Trabalho (Workflow)

O processo ocorre em 6 etapas principais:
1.  Um **Amazon CloudWatch Event** (agendador) invoca a função Lambda `salesAnalysisReport` todos os dias às 20h (Seg-Sáb).
2.  A Lambda `salesAnalysisReport` invoca uma segunda Lambda, a `salesAnalysisReportDataExtractor`.
3.  A `salesAnalysisReportDataExtractor` executa uma consulta no banco de dados "cafe_db" (que está em uma instância EC2).
4.  O resultado da consulta retorna para a primeira função Lambda.
5.  A `salesAnalysisReport` formata o relatório e o publica em um **Tópico SNS** (`salesAnalysisReportTopic`).
6.  O Tópico SNS envia o relatório final por **e-mail** para o administrador.

## 🎯 Objetivo
Com base nos objetivos do lab, o foco era:
* Criar funções Lambda que extraem dados e enviam relatórios.
* Configurar **Políticas de IAM** para permitir que o Lambda acesse outros serviços (como EC2 e SNS) e invoque outras Lambdas.
* Criar um **Lambda Layer** para gerenciar dependências externas (bibliotecas Python).
* Implantar uma Lambda que é iniciada por um **agendamento** (CloudWatch Event).
* Usar **CloudWatch Logs** para monitorar e solucionar problemas das funções.

## 🛠️ Tarefas Realizadas

Para construir esta solução, eu:

* **1. Configurei as Permissões (IAM):** Criei duas Roles de IAM. Uma para a Lambda principal (com permissão para invocar outras Lambdas e publicar no SNS) e outra para a Lambda "extratora" (com permissão para acessar o EC2/banco de dados).
* **2. Criei um Lambda Layer:** Empacotei e fiz o upload de bibliotecas Python necessárias para a conexão com o banco de dados.
* **3. Criei as Funções Lambda:** Implantei os códigos Python para as duas funções (`salesAnalysisReport` e `salesAnalysisReportDataExtractor`).
* **4. Configurei o Agendador:** Criei uma regra no **Amazon CloudWatch Events** (agora EventBridge) para disparar a Lambda principal no horário definido.
* **5. Configurei o Notificador:** Criei um **Tópico SNS** e uma subscrição de e-mail para receber o relatório.
* **6. Armazenei Segredos:** Usei o **AWS Systems Manager Parameter Store** para armazenar com segurança as credenciais do banco de dados (o Lambda lia do Parameter Store, em vez de ter senhas no código).

## 💡 Conceitos Aprendidos
-   A essência da computação **Serverless**: rodar código sem gerenciar servidores.
-   **Arquitetura Orientada a Eventos:** Como diferentes serviços da AWS podem se "encaixar" e disparar ações em cadeia.
-   **Orquestração de Lambdas:** Como uma função Lambda pode invocar (chamar) outra.
-   **Gerenciamento de Dependências** com Lambda Layers.
-   A importância das **IAM Roles** para a segurança entre serviços.
-   Como usar o **Parameter Store** para gerenciar segredos (credenciais de banco de dados).
-   **Troubleshooting Serverless** usando CloudWatch Logs.

## 📸 Minhas Provas (Screenshots)

*(Aqui vou adicionar meus próprios screenshots do CloudWatch Logs mostrando a execução bem-sucedida, o e-mail recebido do SNS e a configuração do Lambda Layer.)*
