# Lab 25: Gerenciar recursos com marcação (Tagging)

Este laboratório foi focado em uma das práticas de governança e automação mais importantes na AWS: **Tagging** (Marcação de Recursos). O lab demonstrou como usar a AWS CLI para inspecionar, modificar e automatizar ações em instâncias EC2 com base em suas tags.

## 🏛️ Arquitetura e Cenário

O cenário consistia em um ambiente com 8 instâncias EC2 privadas já implantadas, cada uma com um conjunto de tags (`Project`, `Version`, `Environment`). A partir de um "Command Host" (Bastion) na sub-rede pública, usei a AWS CLI para gerenciar essas instâncias.

![Diagrama da Arquitetura de Gerenciamento por Tags](./arquitetura-tagging.png)

---

## 🎯 Objetivo
Com base nos objetivos do lab, o foco era:
* Aplicar e modificar tags em recursos AWS existentes.
* **Encontrar recursos** (instâncias) com base em suas tags.
* Usar a **AWS CLI** (e a sintaxe de consulta **JMESPath**) para filtrar saídas.
* (Desafio) Escrever scripts para **automatizar ações** (como parar/iniciar) em instâncias com base em uma tag específica (ex: `Environment=development`).

## 🛠️ Tarefas Realizadas

Todo o laboratório foi executado a partir do terminal SSH do "Command Host":

* **1. Filtragem de Recursos (O Ponto Central):**
    * Usei extensivamente o comando `aws ec2 describe-instances` com o parâmetro `--filters`.
    * Filtrei instâncias por diferentes tags, por exemplo: `Name=tag:Environment,Values=development` para encontrar todas as instâncias de desenvolvimento.

* **2. Análise de Saída (JMESPath):**
    * Usei o parâmetro `--query` para filtrar o JSON de saída da CLI e extrair apenas as informações que eu precisava (ex: `Reservations[].Instances[].InstanceId`).

* **3. Modificação de Tags:**
    * Pratiquei a modificação de tags, como atualizar a `Version` de `1.0` para `1.1` em todas as instâncias do projeto "ERPSystem" usando o comando `aws ec2 create-tags`.

* **4. Automação Baseada em Tags (O Desafio):**
    * Combinei os comandos para criar um script.
    * Primeiro, usei `aws ec2 describe-instances` com `--filters` e `--query` para obter uma **lista de IDs** de instâncias (ex: todas com `Environment=development`).
    * Em seguida, "injetei" essa lista de IDs no comando `aws ec2 stop-instances --instance-ids ...` para desligar todas as instâncias de desenvolvimento de uma só vez.

## 💡 Conceitos Aprendidos
-   **Tagging é a base da Governança na AWS:** Sem tags, é impossível gerenciar custos, automação ou permissões em escala.
-   **Poder do `--filters` na CLI:** Esta é a forma programática de "pesquisar" recursos em sua conta.
-   **Filtragem de JSON com JMESPath (`--query`):** Uma habilidade essencial da CLI para extrair dados específicos (como um `InstanceId`) de uma saída JSON complexa.
-   **Automação para FinOps:** O desafio de parar instâncias "development" é um caso de uso clássico de FinOps para economizar custos, desligando ambientes fora do horário comercial.
-   As tags são usadas para:
    * **Alocação de Custos:** Agrupar custos por `Project` ou `Environment`.
    * **Automação:** Parar/iniciar instâncias por `Environment`.
    * **Controle de Acesso (IAM):** Criar políticas de IAM que permitem a um usuário (ex: `dev-team`) modificar apenas instâncias com a tag `Project=ERPSystem`.

## 📸 Minhas Provas (Screenshots)

*(Aqui vou adicionar meus próprios screenshots do terminal do Command Host mostrando o comando `aws ec2 describe-instances --filters` funcionando e o script de automação parando as instâncias.)*
