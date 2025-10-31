# Lab 13: [Desafio] Exercício de S3

Este foi um "Challenge Lab" (Laboratório de Desafio) focado nas operações fundamentais do Amazon S3, validando o conhecimento essencial para manipulação de objetos.

## 🎯 O Desafio Proposto

O desafio era realizar tarefas de rotina no S3: criar um bucket, fazer upload de um objeto, torná-lo publicamente acessível e listar o conteúdo do bucket usando a AWS CLI.

![Visão Geral do Desafio S3](./s3-challenge-overview.png)

---

## 🛠️ A Solução que eu Implementei

Para completar este desafio, eu executei as seguintes etapas:

* **1. Criação do Bucket:**
    * Criei um novo bucket S3 com um nome globalmente exclusivo.

* **2. Upload de Objeto:**
    * Fiz o upload de um arquivo (ex: `index.html`) para a raiz do bucket.

* **3. Tornando o Objeto Público:**
    * Para que o objeto pudesse ser acessado via navegador, executei duas ações de segurança:
    * **Ação 1 (Bucket):** Desativei o "Bloqueio de Acesso Público" (Block Public Access) no nível do bucket.
    * **Ação 2 (Objeto):** Adicionei uma "ACL" (Access Control List) no próprio objeto, concedendo permissão de "Leitura" (Read) para "Todos" (Public).

* **4. Acesso via Navegador:**
    * Copiei a "Object URL" (URL do Objeto) e colei no navegador, confirmando que o arquivo foi carregado com sucesso.

* **5. Listagem via CLI:**
    * Configurei a AWS CLI e usei o comando `aws s3 ls s3://nome-do-meu-bucket` para listar o conteúdo do bucket, validando o acesso programático.

## 💡 Conceitos Validados
-   Criação e nomeação de buckets S3.
-   O processo de **tornar um objeto público**, que envolve desabilitar o "Block Public Access" e aplicar permissões (ACL ou Política).
-   A diferença entre ACLs (nível do objeto) e Políticas de Bucket (nível do bucket).
-   Como usar comandos básicos da AWS CLI (`aws s3 ls`) para interagir com o S3.

## 📸 Minhas Provas (Screenshots)

*(Aqui vou adicionar meus próprios screenshots mostrando o bucket sendo criado, a configuração de permissões e o objeto sendo exibido no navegador.)*
