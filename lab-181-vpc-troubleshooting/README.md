# Lab 16: Solucionar problemas de uma VPC (VPC Flow Logs)

Este laboratório foi focado em uma das habilidades mais críticas de um engenheiro de nuvem: *troubleshooting* (solução de problemas) de rede. O cenário envolvia um cliente incapaz de acessar um servidor web, e o objetivo era usar **VPC Flow Logs** para diagnosticar e corrigir o problema.

## 🏛️ Arquitetura e Cenário do Problema

A arquitetura consistia em um servidor web em uma sub-rede pública na `VPC 1`. Um cliente externo (Client) tentava acessá-lo, mas a conexão falhava. O desafio era usar os logs gerados pela `VPC 1` (enviados para um bucket S3) para descobrir o que estava bloqueando o tráfego.

![Diagrama da Arquitetura de Troubleshooting](./arquitetura-vpc-flow-logs.png)

---

## 🎯 Objetivo
Com base nos objetivos do lab, o foco era:
* Criar e configurar **VPC Flow Logs** para capturar tráfego IP.
* Analisar os dados dos logs para identificar problemas de conectividade.
* Solucionar problemas de configuração da VPC (Security Groups, NACLs, Route Tables) para restaurar o acesso.

## 🛠️ Processo de Troubleshooting (Tarefas Realizadas)

O processo de diagnóstico e solução seguiu 4 etapas principais:

* **1. Configuração do S3:**
    * Criei um bucket S3 (`flow-logs-bucket`) para ser o destino dos logs de tráfego.

* **2. Criação do VPC Flow Log:**
    * Habilitei o **VPC Flow Log** na `VPC 1`.
    * Configurei o log para capturar todo o tráfego (ACCEPT e REJECT) e enviá-lo para o bucket S3 criado.

* **3. Análise dos Logs (O Diagnóstico):**
    * Simulei novas tentativas de acesso do "Client" para gerar dados de log.
    * Acessei o bucket S3, baixei os arquivos de log (`.log.gz`) e os analisei (usando o `CLI host` na `VPC 2` ou ferramentas locais).
    * Ao analisar os logs, procurei por entradas com **"REJECT"** (Rejeitado) originadas do IP do "Client".
    * A entrada de log "REJECT" indicou *exatamente* qual regra de segurança estava bloqueando o tráfego (se era a Network ACL ou o Security Group).

* **4. Correção do Problema (A Solução):**
    * Com base na análise dos logs, o problema foi identificado: o **Security Group** (`Security group`) do servidor web não estava permitindo tráfego de entrada na **porta 80 (HTTP)**.
    * Editei as regras de "inbound" do Security Group para permitir o tráfego HTTP.
    * Após a correção, testei novamente o acesso do "Client" e confirmei que a aplicação web ficou acessível.

## 💡 Conceitos Aprendidos
-   **VPC Flow Logs** são a principal ferramenta para monitorar e diagnosticar o tráfego IP em uma VPC.
-   Como ler os logs: identificar `ACCEPT` vs. `REJECT`, IPs de origem/destino e portas.
-   A metodologia de troubleshooting de conectividade na AWS:
    1.  Verificar **Route Table** (O tráfego tem um caminho? Ex: Rota para o IGW).
    2.  Verificar **Network ACL** (A "parede" da sub-rede permite o tráfego? É *stateless*).
    3.  Verificar **Security Group** (O "firewall" da instância permite o tráfego? É *stateful*).
-   Como os logs confirmam qual desses 3 componentes está causando a falha.

## 📸 Minhas Provas (Screenshots)

*(Aqui vou adicionar meus próprios screenshots mostrando a entrada de log "REJECT" no arquivo de log, a configuração incorreta do Security Group (antes) e a configuração corrigida (depois).) *
