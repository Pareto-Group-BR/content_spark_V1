# Documentação Técnica - Fluxo N8N: Agente de Criação de Conteúdo (Brasilidades)

Com certeza! Aqui está um índice em formato Markdown, pronto para ser usado no GitHub, com base na documentação que você forneceu.

## 📚 Índice

- [1. Visão Geral](#1-visão-geral)
- [2. Arquitetura do Fluxo e Replicação](#2-arquitetura-do-fluxo-e-replicação)
    - [Passo a Passo para Replicar o Fluxo BRASILIDADES](#passo-a-passo-para-replicar-o-fluxo-brasilidades)
- [3. APIs Utilizadas e Credenciais](#3-apis-utilizadas-e-credenciais)
- [4. Etapas Detalhadas do Fluxo](#4-etapas-detalhadas-do-fluxo)
    - [Etapa 1: Início (Triggers)](#etapa-1-início-triggers)
    - [Etapa 2: Coleta de Dados](#etapa-2-coleta-de-dados)
    - [Etapa 3: Processamento e Análise](#etapa-3-processamento-e-análise)
    - [Etapa 4: Branch de Conteúdo (Tradução e Geração)](#etapa-4-branch-de-conteúdo-tradução-e-geração)
    - [Etapa 5: Armazenamento e Registro](#etapa-5-armazenamento-e-registro)
    - [Etapa 6: Notificação](#etapa-6-notificação)
- [5. Tratamento de Erros e Resiliência](#5-tratamento-de-erros-e-resiliência)
- [6. Variáveis e Expressões Importantes](#6-variáveis-e-expressões-importantes)
- [7. Arquivo JSON](#7-arquivo-json)

## 1. Visão Geral

Este documento detalha o fluxo de trabalho da automação construída na plataforma N8N, denominada **"[PARETO] Agente Criação de Conteúdo (Brasilidades)"**. O objetivo principal desta automação é identificar posts de referência no Instagram, extrair seu conteúdo, traduzi-lo, gerar novas mídias (imagens) baseadas no conteúdo original e, por fim, organizar e notificar sobre o novo conteúdo criado.

O fluxo é projetado para ser robusto, tratando diferentes tipos de posts (imagem única e carrossel) e garantindo que todas as etapas sejam registradas em uma [Planilha de Controle](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit?gid=0#gid=0).
>IMPORTANTE: Duplique esta planilha, pois trata-se de um TEMPLATE.


**Link de Acesso ao Fluxo:**
- [Arquivo JSON do Fluxo N8N](https://cdn.tess.im/assets/uploads/8bdc782a-b147-4f6a-842d-70edb6220f36.json)

---

## 2. Arquitetura do Fluxo e Replicação

O fluxo é dividido em várias etapas lógicas, que são executadas em sequência. Abaixo está a descrição da arquitetura macro.

```
Início: Gatilho
    ↓
Coleta de Dados
    ↓
Processamento e Análise
    ↓
Decisão: Tipo de Post
    ├─→ Imagem Única → Branch de Imagem Única
    └─→ Carrossel → Branch de Carrossel
    ↓
Geração de Conteúdo
    ↓
Armazenamento e Registro
    ↓
Notificação
    ↓
Fim
```

### Passo a Passo para Replicar o Fluxo BRASILIDADES

Para replicar este fluxo em seu próprio ambiente, siga as etapas abaixo.

#### **Etapa 1: Preparar Ativos (Credenciais, Agentes e Pasta)**

Antes de importar o fluxo, você precisa preparar todos os recursos externos.

1.  **Credenciais no N8N:** Acesse sua instância do N8N e, na seção **Credentials**, crie as credenciais necessárias (Google Sheets API, Google Drive API, Apify, Tess AI API, HtmlCssToImage API, etc.).
2.  **Agentes na Tess AI:** Os IDs dos agentes são únicos por workspace. Você precisa recriá-los:
    *   Consulte a seção **"6. Agentes de IA Utilizados"** deste documento para ver a lista de agentes (ex: 34674, 34683, etc.).
    *   Em seu próprio workspace da Tess AI, **crie ou duplique cada agente**, utilizando os mesmos prompts e configurações do fluxo original.
    *   Anote os **novos IDs** de cada um dos seus agentes.
3.  **Pasta no Drive:** Crie uma pasta principal no seu Google Drive onde as artes serão salvas e copie o **ID da pasta** (a parte final do URL).

#### **Etapa 2: Replicar a Planilha de Controle**

1.  **Faça uma cópia** do template da planilha: [**Template - Planilha de Controle**](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit).
2.  Em sua nova planilha, acesse **`Extensões > Apps Script`** e conceda as permissões de execução do script.

#### **Etapa 3: Importar e Configurar o Fluxo no N8N**

1.  **Importe o arquivo JSON** deste fluxo (`BRASILIDADES`) para a sua instância do N8N. [Link para Download](https://cdn.tess.im/assets/uploads/2ce4845c-0bd8-4dd1-99c2-4a0373259bc0.json?_gl=1*xomccc*_gcl_au*MTg4Nzg4OTA0My4xNzY5Njk3NTg4LjE0OTc4MDAyNzUuMTc3MDA1NzgwNS4xNzcwMDYwMjUw*_ga*OTM0Mzg4NjAxLjE3Njk2OTc1ODg.*_ga_K1Q8FJY3BS*czE3NzAwNTY0NjUkbzkkZzEkdDE3NzAwNjAyNTAkajUkbDAkaDA.*_ga_9D17W435GL*czE3NzAwNTI2OTgkbzI4JGcxJHQxNzcwMDYwMjUwJGo1JGwwJGgw)
2.  **Copie o URL do seu novo Webhook** no nó `Webhook` (aba "Production").
3.  **Cole o Webhook na sua Planilha** no Apps Script, na variável `WEBHOOK_URL_BRASILIDADES`, e salve.
4.  **Atualize os IDs no N8N:**
    *   **Pasta do Drive:** No nó que salva os arquivos no Drive (ex: "Create folder" ou "Upload to Drive"), cole o **ID da sua pasta** no campo apropriado.
    *   **Agentes de IA:** Nos nós que fazem chamadas para a Tess AI, **substitua os IDs dos agentes antigos pelos novos IDs** que você criou.
5.  **Verifique os Nós Manualmente:** Percorra os demais nós para confirmar se suas credenciais foram associadas corretamente.

>*Observação: É importante ressaltar que, para UTILIZAR a planilha de Criação de Conteúdo, não é necessário ter acesso ao N8N. Mas sim, ter as permissões do Google AppScript configuradas.*


---

## 3. APIs Utilizadas e Credenciais

Para que o fluxo funcione, é necessário configurar o acesso às seguintes APIs no N8N. Cada uma requer um tipo específico de credencial.

### 3.1 Google Suite (Sheets, Drive, Gmail)
- **Finalidade**: Leitura de perfis de referência (Sheets), armazenamento de artes (Drive) e envio de notificações por e-mail (Gmail).
- **Tipo de Credencial**: **OAuth2**
- **Descrição**: É necessário autenticar uma conta Google que tenha permissão para acessar esses serviços.
- **Serviços Utilizados**:
  - Google Sheets API
  - Google Drive API
  - Gmail API

### 3.2 Apify API
- **Finalidade**: Execução do "Instagram Scraper" para coletar dados de posts e perfis do Instagram.
- **Tipo de Credencial**: **API Token**
- **Descrição**: A chave da API é enviada como um parâmetro de consulta (`token=...`) na URL da requisição HTTP.
- **Endpoint**: `https://api.apify.com/v2/actor-tasks/...`

### 3.3 Tess AI Agents API
- **Finalidade**: Orquestração de agentes de IA para tradução de legendas, análise de imagens, geração de código HTML para novas imagens e gerenciamento de arquivos na plataforma Tess.
- **Tipo de Credencial**: **Bearer Token**
- **Descrição**: Um token de acesso é enviado no cabeçalho `Authorization` de cada requisição HTTP.
- **Endpoint**: `https://tess.pareto.io/api/agents/...`
- **Exemplo de Header**: `Authorization: Bearer 1205|jjz9BIyc94n2S4NFqDrmzNV6cyNxoeTrBb6v92dX675dadb8`

### 3.4 Google Chat API (via Webhook)
- **Finalidade**: Envio de notificações estruturadas para um espaço (sala) no Google Chat.
- **Tipo de Credencial**: **URL de Webhook**
- **Descrição**: A URL contém uma chave de API e um token de segurança, que autorizam o envio de mensagens para o espaço específico.
- **Endpoint**: `https://chat.googleapis.com/v1/spaces/AAQAbJ7q6g4/messages?key=...&token=...`

### 3.5 HtmlCssToImage API
- **Finalidade**: Conversão de código HTML em imagens PNG.
- **Tipo de Credencial**: **API Key**
- **Descrição**: A chave é enviada como parâmetro nas requisições HTTP.

---

## 4. Etapas Detalhadas do Fluxo

### Etapa 1: Início (Triggers)

O fluxo pode ser iniciado de duas maneiras:

#### 1.1 Programação (Schedule)
- **Nó**: `n8n-nodes-base.scheduleTrigger`
- **Configuração**: Executa a automação em dias e horários pré-definidos à meia noite (o usuário configura essa programação na [Planilha de Controle](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit?gid=0#gid=0).
- **Exemplo**: A cada segunda-feira, quinta-feira e domingo à meia-noite.

#### 1.2 Webhook
- **Nó**: `n8n-nodes-base.webhook`
- **Finalidade**: Permite que a automação seja iniciada por uma chamada de API externa, através do menu da [Planilha de Controle](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit?gid=0#gid=0).
- **Vantagem**: Oferece flexibilidade para execuções independentes da programação.

<img width="258" height="341" alt="image" src="https://github.com/user-attachments/assets/d042ed42-1215-4f71-b29d-dabd9cf479fa" />

---

### Etapa 2: Coleta de Dados

#### 2.1 Leitura de Perfis de Referência
- **Nó**: `Get row(s) in sheet` (Google Sheets)
- **Ação**: Lê a planilha **"Perfis de Referência"** do Google Sheets.
- **Output**: Lista de perfis do Instagram que servirão de base para a busca de conteúdo.

#### 2.2 Preparação de Parâmetros para Apify
- **Nó**: `Format Apify run input` (Code - JavaScript)
- **Ação**: Prepara dois conjuntos de parâmetros:
  - **Parâmetro 1**: `resultsType: "posts"` - Busca posts recentes dos perfis
  - **Parâmetro 2**: `resultsType: "details"` - Busca detalhes dos perfis (número de seguidores, etc.)

#### 2.3 Execução do Instagram Scraper
- **Nó**: `Instagram Scraper` (HTTP Request - Apify)
- **Ação**: Faz chamadas HTTP para a API da Apify.
- **Input**: Parâmetros preparados na etapa anterior.
- **Output**: Dados brutos de posts e perfis do Instagram.

<img width="743" height="401" alt="image" src="https://github.com/user-attachments/assets/1bd941c7-ae87-4ff7-86ad-551ac6b88e1e" />


---

### Etapa 3: Processamento e Análise

#### 3.1 Agregação de Dados
- **Nó**: `Aggregate` e `Set`
- **Ação**: Agregam e formatam os dados brutos da Apify.
- **Objetivo**: Preparar os dados para análise posterior.

#### 3.2 Identificação do Post com Maior Engajamento
- **Nó**: `Posts Tendência` (Code - JavaScript)
- **Lógica**: Analisa todos os posts coletados e encontra aquele com a **maior taxa de engajamento proporcional**.
- **Cálculo**:
  ```
  Taxa de Engajamento = (curtidas + comentários) / número de seguidores do perfil
  ```
- **Output**: Post selecionado como referência principal, com todos os seus dados.

#### 3.3 Decisão: Tipo de Post
- **Nó**: `If: Childposts > 1` (If)
- **Lógica**: Verifica se o post de referência é um carrossel (contém múltiplas imagens em `childPosts`).
- **Branches**:
  - **True (Carrossel)**: Segue o fluxo para tratamento de carrosséis.
  - **False (Imagem Única)**: Segue o fluxo para tratamento de imagens únicas.

<img width="1087" height="358" alt="image" src="https://github.com/user-attachments/assets/0831c646-4e9b-4b58-82af-b8e9de99abca" />


---

### Etapa 4: Branch de Conteúdo (Tradução e Geração)

Ambos os branches (Imagem Única e Carrossel) seguem uma lógica semelhante.

<img width="385" height="529" alt="image" src="https://github.com/user-attachments/assets/c4889c13-e225-4fd4-9ed3-b3c8cec7bfa5" />


#### 4.1 Mapeamento de Campos Relevantes
- **Nó**: `Mapear Campos Relevantes` (Set)
- **Ação**: Extrai as informações mais importantes do post de referência:
  - Legenda original
  - URL da imagem/post
  - Nome do perfil
  - URL do post
- **Output**: Dados estruturados e prontos para processamento.

#### 4.2 Preparação do JSON para Tradução
- **Nó**: `JSON Legenda` (Code - JavaScript)
- **Ação**: Prepara o JSON contendo a legenda original.
- **Output**: Objeto JSON estruturado.

#### 4.3 Tradução da Legenda
- **Nó**: `TESS - Traduzir Legenda` (HTTP Request)
- **Ação**: Faz uma chamada ao agente TESS com a instrução: "Traduza a legenda para o português".
- **API**: `https://tess.pareto.io/api/agents/34357/execute`
- **Método**: POST
- **Autenticação**: Bearer Token
- **Output**: Legenda traduzida do inglês para o português.

#### 4.4 Formatação da Legenda Traduzida
- **Nó**: `Formatar Legenda Traduzida` (Code - JavaScript)
- **Ação**: Processa a resposta da tradução e extrai apenas o texto limpo.
- **Limpeza**: Remove caracteres de escape (`
`, `"`, `\`, etc.) e converte códigos Unicode.
- **Output**: Legenda traduzida e limpa.

#### 4.5 Geração de HTML da Imagem Original
- **Nó**: `TESS - Gerar HTML da Imagem Original` (HTTP Request)
- **Ação**: Um agente TESS descreve o conteúdo visual da imagem de referência e gera um código HTML correspondente.
- **API**: `https://tess.pareto.io/api/agents/34686/execute`
- **Output**: Código HTML que representa a imagem original.

#### 4.6 Descrição e Tradução de Textos na Imagem
- **Nó**: `TESS - Descrever e Traduzir Texto` (HTTP Request)
- **Ação**: Outro agente analisa os textos presentes na imagem original, descreve-os e os traduz.
- **API**: `https://tess.pareto.io/api/agents/34357/execute`
- **Output**: Descrição e tradução dos textos.

#### 4.7 Geração do HTML Traduzido
- **Nó**: `TESS - Gerar HTML da Imagem Nova` (HTTP Request)
- **Ação**: Um terceiro agente recebe o HTML original e a tradução dos textos, gerando um **novo código HTML** com o conteúdo completamente em português.
- **API**: `https://tess.pareto.io/api/agents/34686/execute`
- **Input**: 
  - HTML original
  - Tradução dos textos
  - Instrução: "Gere o HTML da Imagem, substituindo os textos em Inglês pelos textos em Português"
- **Output**: Código HTML completamente em português.

#### 4.8 Conversão de HTML para Imagem
- **Nó**: `Gerar imagem pelo HTML` (HtmlCssToImage)
- **Ação**: Converte o novo código HTML em uma imagem PNG.
- **Parâmetros**:
  - `viewPortHeight`: 1350 (altura da página)
  - `response_format_html`: "png"
- **Output**: Imagem PNG da nova criação.

<img width="279" height="171" alt="image" src="https://github.com/user-attachments/assets/4829a16a-1f41-46c2-9dea-24ecf7986963" />

---

### Etapa 5: Armazenamento e Registro

<img width="807" height="283" alt="image" src="https://github.com/user-attachments/assets/ab37ab6a-4fc3-4259-934e-68f411a95685" />



#### 5.1 Criação de Pasta no Google Drive
- **Nó**: `Create folder` (Google Drive)
- **Ação**: Cria uma nova pasta no Google Drive para armazenar as imagens geradas.
- **Nomenclatura**: Nome incluindo a data de execução.
- **Output**: ID da pasta criada.

#### 5.2 Download da Imagem
- **Nó**: `Baixar imagem` (HTTP Request)
- **Ação**: Realiza o download da imagem PNG gerada pelo HtmlCssToImage.
- **Output**: Imagem em formato binário.

#### 5.3 Upload da Imagem para Google Drive
- **Nó**: `Subir imagem no drive` (Google Drive)
- **Ação**: Faz upload da imagem PNG para a pasta recém-criada no Google Drive.
- **Output**: Link público da imagem armazenada.

#### 5.4 Atualização da Planilha de Controle
- **Nó**: `Atualizar planilha` (Google Sheets)
- **Operação**: `append` (adiciona uma nova linha)
- **Planilha**: **"BRASILIDADES"**
- **Dados Registrados**:
  - **Tema**: Tema do conteúdo gerado
  - **Legenda**: Legenda traduzida
  - **ID da criação**: ID único da execução
  - **Artes**: Link para a pasta no Google Drive com as imagens
  - **Plataforma**: "Instagram"
  - **Perfil de Referência**: Nome do perfil do Instagram
  - **Link do Post Original**: URL do post de referência
  - **Post de Referência (DisplayURL)**: URL da imagem do post
  - **Taxa de Engajamento**: Cálculo de likes + comments / followers
  - **Data de Criação**: Data/hora da execução
  - **Custo de créditos**: Quantidade de créditos gastos na execução

---

### Etapa 6: Notificação

<img width="565" height="275" alt="image" src="https://github.com/user-attachments/assets/ae459607-5c81-4bb8-94b2-6a0cf6dbf854" />


#### 6.1 Unificação de Branches
- **Nó**: `Merge`
- **Ação**: Combina os diferentes branches do fluxo (Imagem Única e Carrossel) antes da notificação final.

#### 6.2 Notificação no Google Chat
- **Nó**: `Enviar Mensagem Gchat` (HTTP Request)
- **Ação**: Envia uma mensagem estruturada para um espaço no Google Chat.
- **API**: `https://chat.googleapis.com/v1/spaces/AAQAbJ7q6g4/messages`
- **Conteúdo**:
  - Tema do carrossel/imagem
  - Botão: "ACESSAR ARTES" (link para pasta no Drive)
  - Botão: "POST DE REFERÊNCIA" (link para o post original)
  - Botão: "ABRIR PLANILHA" (link para a planilha de controle)

#### 6.3 Notificação por E-mail
- **Nó**: `Send a message` (Gmail)
- **Ação**: Envia um e-mail formatado em HTML para a lista de destinatários.
- **Conteúdo do E-mail**:
  - Header com logo Pareto e gradiente de cor
  - Título: "Carrossel gerado com sucesso"
  - Subtítulo com informações sobre a geração
  - Highlight box com o tema do carrossel
  - Descrição do processo
  - Botão primário: "Abrir Planilha no Google Sheets"
  - Footer com créditos da equipe
- **Design**: Totalmente responsivo, segue a paleta de cores Pareto (Infinite Blue, Tess Purple, Cool Blue)

---

## 5. Tratamento de Erros e Resiliência

### 5.1 Retry em Chamadas Críticas
- **Nós com Retry**: As chamadas HTTP para as APIs TESS, Apify e Google possuem configurações de `retryOnFail`.
- **Tentativas**: Até 5 tentativas com intervalo de 5 segundos entre elas.
- **Objetivo**: Aumentar a resiliência em caso de falhas temporárias de rede ou API.

### 5.2 Validação de Dados
- **Nós If**: Verificam se as respostas das APIs não estão vazias antes de prosseguir.
- **Exemplo**: `If5` verifica se `responses[0].output` não é vazio.
- **Objetivo**: Evitar que o fluxo prossiga com dados inválidos ou incompletos.

### 5.3 Stop and Error
- **Nó**: `Stop and Error`
- **Posicionamento**: Estrategicamente colocado para interromper a execução em caso de falha crítica.
- **Objetivo**: Prevenir a geração de dados inconsistentes.

---

## 6. Variáveis e Expressões Importantes

### 6.1 Acesso a Dados de Nós Anteriores
O fluxo utiliza a sintaxe N8N `{{ $('NomeDonó').item.json.campo }}` para acessar dados de nós anteriores.

**Exemplos**:
- `{{ $('Posts Tendência').item.json.postComMaiorEngajamento.likesCount }}`
- `{{ $('Mapear Campos Relevantes - Imagem').item.json.tema }}`

### 6.2 Função de Data
- `{{ $now.format('dd-MM-yyyy') }}` - Retorna a data atual no formato DD-MM-YYYY.

### 6.3 ID de Execução
- `{{ $execution.id }}` - Retorna o ID único da execução atual do fluxo.

---

## 7. Arquivo JSON

**Link de Acesso ao arquivo JSON do Fluxo:**
- [Arquivo JSON do Fluxo N8N](https://cdn.tess.im/assets/uploads/2ce4845c-0bd8-4dd1-99c2-4a0373259bc0.json?_gl=1*xomccc*_gcl_au*MTg4Nzg4OTA0My4xNzY5Njk3NTg4LjE0OTc4MDAyNzUuMTc3MDA1NzgwNS4xNzcwMDYwMjUw*_ga*OTM0Mzg4NjAxLjE3Njk2OTc1ODg.*_ga_K1Q8FJY3BS*czE3NzAwNTY0NjUkbzkkZzEkdDE3NzAwNjAyNTAkajUkbDAkaDA.*_ga_9D17W435GL*czE3NzAwNTI2OTgkbzI4JGcxJHQxNzcwMDYwMjUwJGo1JGwwJGgw)
