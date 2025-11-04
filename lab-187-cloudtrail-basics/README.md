# Lab 22: Trabalhar com o AWS CloudTrail (Análise Forense)

Este foi um laboratório avançado focado em segurança, auditoria e **resposta a incidentes**. O cenário simulava um hack a um servidor web, e o objetivo era usar o AWS CloudTrail e o Amazon Athena para conduzir uma investigação forense e descobrir o culpado.

## 🏛️ Arquitetura de Auditoria e Análise

A arquitetura mostra como o **CloudTrail** atua como um serviço de vigilância, registrando todas as ações (chamadas de API) na conta. Esses logs são entregues a um **Bucket S3** para armazenamento seguro. Em seguida, o **Amazon Athena** é usado para executar consultas SQL diretamente nos arquivos de log, permitindo uma análise poderosa e rápida.

![Diagrama da Arquitetura de Análise Forense](./arquitetura-cloudtrail-athena.png)

---

## 🎯 Objetivo
Com base nos objetivos do lab, o foco era:
* Configurar uma "trilha" do CloudTrail para capturar todas as ações da conta.
* Analisar os logs do CloudTrail (via CLI/grep) para encontrar informações relevantes sobre um incidente de segurança (um Security Group modificado).
* Importar os logs do CloudTrail para o **Amazon Athena** para uma análise mais profunda.
* Executar **consultas SQL** no Athena para filtrar os logs e identificar o invasor.
* Resolver as preocupações de segurança (remover o acesso do invasor) e proteger a conta.

## 🛠️ O Processo de Investigação (Tarefas Realizadas)

O laboratório simulou um incidente de segurança de ponta a ponta:

* **1. O Incidente:**
    * O laboratório começou com a descoberta de que um servidor web ("Café Web Server") havia sido hackeado ("Hacked website content").
    * A investigação sugeriu que o hack foi possível porque alguém modificou o **Security Group** da instância.

* **2. Configuração da Ferramenta Forense:**
    * Eu configurei o **CloudTrail** para criar uma trilha, registrando todos os eventos de gerenciamento.
    * Configurei a trilha para entregar os arquivos de log (`.json.gz`) para um bucket S3.

* **3. Análise Rápida (CLI):**
    * Usei a AWS CLI (`aws cloudtrail lookup-events`) e ferramentas Linux (`grep`) para filtrar rapidamente os logs e procurar por eventos suspeitos, como `ModifySecurityGroupIngress`.

* **4. Análise Avançada (Amazon Athena):**
    * Para uma análise mais poderosa, naveguei até o **Amazon Athena**.
    * Criei uma tabela no Athena, apontando-a para os dados do CloudTrail no S3.
    * Executei consultas SQL para filtrar os logs. Por exemplo:
        ```sql
        SELECT eventTime, eventSource, eventName, userIdentity.arn
        FROM minha_tabela_cloudtrail
        WHERE eventName = 'ModifySecurityGroupIngress'
        ORDER BY eventTime DESC;
        ```
    * A consulta SQL revelou **exatamente qual usuário IAM** (`userIdentity.arn`) executou a ação `ModifySecurityGroupIngress` e quando (`eventTime`).

* **5. Resolução do Incidente:**
    * Com a identidade do invasor confirmada pela consulta do Athena, a etapa final foi a remediação.
    * Naveguei até o console do IAM, localizei o usuário culpado, desanexei suas políticas de permissão e/ou excluí o usuário para revogar seu acesso.
    * Corrigi o Security Group para fechar as portas de acesso indevidas.

## 💡 Conceitos Aprendidos
-   **CloudTrail é o "Big Brother" da AWS:** Ele registra TUDO (quem, o quê, quando) que acontece via chamada de API.
-   **Análise Forense com Athena:** Usar SQL no Athena para consultar terabytes de logs no S3 é a forma profissional de conduzir investigações de segurança, sendo muito mais rápido e poderoso do que `grep`.
-   **Logs como Prova:** Os logs do CloudTrail são a fonte da verdade para auditoria e compliance, provando quem realizou uma ação.
-   **Fluxo de Resposta a Incidentes:** O lab ensinou o fluxo completo: **Detectar** (site hackeado) -> **Investigar** (CloudTrail + Athena) -> **Remediar** (Excluir usuário IAM, corrigir SG).

## 📸 Minhas Provas (Screenshots)

*(Aqui vou adicionar meus próprios screenshots mostrando a consulta SQL no Athena, os resultados da consulta identificando o culpado e o usuário IAM sendo desativado.)*
