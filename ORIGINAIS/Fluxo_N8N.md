# Documentação Detalhada do Fluxo de Automação N8N - Criação de Conteúdo

## 📚 Índice

1. [Visão Geral](#visão-geral)
2. [Arquitetura do Fluxo](#arquitetura-do-fluxo)
3. [Etapas Detalhadas](#etapas-detalhadas)
4. [APIs Utilizadas](#apis-utilizadas)
5. [Credenciais Necessárias](#credenciais-necessárias)
6. [Troubleshooting](#troubleshooting)
7. [Arquivo do Fluxo](#arquivo-do-fluxo)

---

## Visão Geral

Este fluxo de trabalho automatizado em **N8N** foi projetado para criar conteúdo de alta qualidade para Instagram, desde a identificação de tendências até a geração de imagens e carrosséis. O workflow executa as seguintes funcionalidades principais:

- ✅ **Coleta de Tendências**: Identifica tendências em tempo real usando SerpAPI e agentes TESS
- ✅ **Coleta de Posts**: Scraping de posts do Instagram relacionados às tendências
- ✅ **Análise de Conteúdo**: Analisa posts relevantes para extrair padrões
- ✅ **Geração de Conteúdo**: Cria roteiros, legendas e briefings usando IA
- ✅ **Criação de Visuais**: Gera imagens e carrosséis em HTML e converte para PNG
- ✅ **Armazenamento**: Salva artes no Google Drive e dados em Google Sheets
- ✅ **Notificações**: Envia notificações via Gmail e Google Chat

---

## Arquitetura do Fluxo

### Estrutura Geral

```
┌─────────────────────────────────────────────────────────────────┐
│                  GATILHO / AGENDAMENTO                          │
│    Programação Diária à meia noite (dias específicos, a escolha do usuário)                  │
└────┬───────────────────────────────────────────────────────────┘
     │
     └──► [FASE 1] Coleta de Dados e Posts
     │    ├─ Tendências TESS
     │    ├─ Hashtags Twitter
     │    ├─ Trends Google (SerpAPI)
     │    ├─ Preparação Apify Instagram
     │    └─ Execução Apify + Coleta de posts do Instagram
     │
     └──► [FASE 2] Processamento de Tendências
     │    ├─ Merge de dados
     │    ├─ Filtragem de categorias
     │    ├─ Identificação de tendências relevantes
     │    └─ Seleção de tema
     │
     └──► [FASE 3] Análise e Curadoria
     │    ├─ Coleta de posts de referência
     │    ├─ Análise de capas dos carrosséis
     │    ├─ Pesquisa aprofundada
     │    └─ Seleção de tema final
     │
     └──► [FASE 4] Criação de Conteúdo
     │    ├─ Geração de roteiro
     │    ├─ Criação de briefing para imagens
     │    ├─ Geração de HTML para carrosséis
     │    └─ Criação de anúncios em HTML
     │
     └──► [FASE 5] Processamento de Imagens
     │    ├─ Conversão HTML → PNG
     │    ├─ Download de imagens
     │    ├─ Upload para Google Drive
     │    └─ Organização em pastas
     │
     └──► [FASE 6] Finalização
          ├─ Registro em Google Sheets
          ├─ Envio de notificação Gmail
          └─ Mensagem no Google Chat
```

---

Com certeza! Reorganizei e reescrevi a seção "Etapas Detalhadas" para que cada passo corresponda exatamente às 6 fases descritas na arquitetura do fluxo. A nova estrutura segue a lógica de cada fase, tornando a documentação mais clara e coesa.

Aqui está a versão revisada:

---

## Etapas Detalhadas

A seguir, detalhamos cada nó do fluxo de automação, organizados de acordo com as 6 fases principais do processo.

### **FASE 1: Coleta de Dados e Posts**

<img width="1057" height="395" alt="image" src="https://github.com/user-attachments/assets/9edcbad7-e7e6-42f8-b7c7-ab64898d4fde" />


Nesta fase inicial, o fluxo é acionado e executa coletas de dados em paralelo de múltiplas fontes: agentes TESS para tendências e hashtags, SerpAPI para tendências do Google e Apify para coletar posts recentes do Instagram.

*   **1.1. Gatilho por Agendamento**
    *   **Tipo**: Trigger
    *   **Frequência**: Segundas e Quartas-feiras à meia-noite.
    *   **Função**: Inicia o fluxo automaticamente, disparando os ramos de coleta de dados em paralelo.

*   **1.2. Coleta de Tendências da Semana (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: Executa o agente TESS (`31523`) para obter a lista de tendências da semana. A execução é assíncrona.
    *   **Próximo Nó**: Wait (para aguardar a resposta).

*   **1.3. Coleta de Hashtags do Twitter (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: Executa o agente TESS (`31107`) para coletar as hashtags mais relevantes da semana no Twitter/X. A execução é assíncrona.
    *   **Próximo Nó**: Wait (para aguardar a resposta).

*   **1.4. Coleta de Google Trends (SerpAPI)**
    *   **Tipo**: HTTP Request (GET)
    *   **API**: `https://serpapi.com/search`
    *   **Descrição**: Busca as tendências de pesquisa do Google no Brasil em tempo real usando o motor `google_trends_trending_now`.

*   **1.5. Preparação para Coleta de Posts (Apify)**
    *   **Tipo**: Code Node (JavaScript)
    *   **Descrição**: Formata o objeto `run_input` com os parâmetros para o scraper do Instagram, definindo perfis-alvo, hashtags, e o limite de posts (recentes, dos últimos 2 dias).

*   **1.6. Execução do Instagram Scraper (Apify)**
    *   **Tipo**: HTTP Request (POST)
    *   **API**: `https://api.apify.com/v2/acts/shu8hvrXbJbY3Eb9W/run-sync`
    *   **Descrição**: Dispara a execução do ator no Apify para coletar posts de referência que servirão de base para a análise.

*   **1.7. Aguardo e Validação da Coleta (Apify)**
    *   **Tipo**: Loop com Wait e IF
    *   **Descrição**: O fluxo aguarda e verifica periodicamente (`GET | Last Run Apify`) o status da execução no Apify. Ele só prossegue quando o status for `SUCCEEDED`.

*   **1.8. Recuperação dos Posts Coletados (Apify)**
    *   **Tipo**: HTTP Request (GET)
    *   **API**: `https://api.apify.com/v2/actor-runs/{run_id}/dataset/items`
    *   **Descrição**: Após a conclusão bem-sucedida, faz o download do dataset contendo os posts coletados (caption, imagens, likes, etc.).

*   **1.9. Recuperação dos Resultados (TESS)**
    *   **Tipo**: Wait e HTTP Request (GET)
    *   **Descrição**: Após um período de espera para garantir que os agentes TESS concluíram a execução, o fluxo recupera os resultados (tendências e hashtags) através da API de respostas (`GET /agent-responses/{response_id}`).

### **FASE 2: Processamento de Tendências**

Com os dados brutos coletados, esta fase foca em limpar, filtrar, unificar e preparar as informações para a análise de IA. (Obs: os nós descritos nessa etapa aparecem em prints das etapas 1 e 2, são os nós de tratamento de dados)

<img width="573" height="443" alt="image" src="https://github.com/user-attachments/assets/04d80d8d-b3f1-428f-b452-6f5b581b8a1d" />


*   **2.1. Filtragem de Posts do Instagram**
    *   **Tipo**: Filter Node
    *   **Função**: Filtra os posts coletados pelo Apify com base em critérios de qualidade, como remover posts fixados (`isPinned`) ou com baixo engajamento.

*   **2.2. Agregação dos Posts**
    *   **Tipo**: Aggregate Node
    *   **Função**: Consolida todos os posts filtrados em um único array (`instagram_posts`) para facilitar o processamento.

*   **2.3. Filtragem de Categorias do Google Trends**
    *   **Tipo**: Code Node (JavaScript)
    *   **Função**: Processa as tendências vindas da SerpAPI, removendo categorias indesejadas (ex: política, esportes) para focar em temas mais alinhados ao objetivo.

*   **2.4. Armazenamento e Estruturação dos Dados**
    *   **Tipo**: Set Variable
    *   **Função**: Organiza os dados de cada fonte (TESS, Twitter, Google Trends, Instagram) em variáveis separadas (`tendencias_tess`, `hashtags`, `top_10_trends`, `instagram_posts`).

*   **2.5. Merge (Unificação dos Dados)**
    *   **Tipo**: Merge Node
    *   **Função**: Consolida todos os dados coletados e processados (tendências, hashtags e posts) em um único objeto JSON.

*   **2.6. Formatação Final para IA**
    *   **Tipo**: JSON Stringify (Code Node)
    *   **Função**: Converte o objeto unificado em uma string JSON, preparando o payload completo para ser enviado ao primeiro agente de análise.

### **FASE 3: Análise e Curadoria**

Nesta fase, agentes de IA analisam os dados consolidados para identificar padrões, selecionar um tema principal e aprofundar a pesquisa sobre ele.

<img width="855" height="221" alt="image" src="https://github.com/user-attachments/assets/9cb4e0ad-80ff-4f55-a99b-29515629fb56" />

<img width="1069" height="367" alt="image" src="https://github.com/user-attachments/assets/786d200e-8b5b-4d6c-99e3-09e7bcb04451" />

*   **3.1. Identificação de Tendências Relevantes (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: Envia o JSON consolidado para o agente TESS (`31104`), que identifica os padrões e sugere temas potenciais.

*   **3.2. Análise das Capas de Carrosséis (TESS)**
    *   **Tipo**: Loop e HTTP Request (POST)
    *   **Descrição**: O fluxo itera sobre os posts de referência e utiliza um agente TESS específico (`33079`) para analisar os elementos visuais das capas (cores, composição, legibilidade), enriquecendo os dados.

*   **3.3. Seleção do Tema Final (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: Com base nas tendências e na análise visual, o agente de curadoria TESS (`31119`) seleciona o tema com maior potencial (relevância, viralidade, alinhamento) e fornece uma justificativa.

*   **3.4. Pesquisa Aprofundada sobre o Tema (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: Utilizando o agente TESS de pesquisa (`32754`), o fluxo realiza uma análise detalhada do tema escolhido, buscando histórico, dados estatísticos, sentimento do público e oportunidades de nicho.

*   **3.5. Formatação da Pesquisa**
    *   **Tipo**: Code Node
    *   **Função**: Estrutura o resultado da pesquisa aprofundada, preparando-o para ser o insumo principal da fase de criação de conteúdo.

### **FASE 4: Criação de Conteúdo**

Com um tema validado e pesquisado, o foco se volta para a geração do conteúdo em si: roteiro, legendas e os elementos visuais em formato HTML.

<img width="1235" height="208" alt="image" src="https://github.com/user-attachments/assets/25d7d13b-56e5-458e-8bc2-04bd56c367d6" />

*   **4.1. Criação do Roteiro do Post (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: O agente criador de roteiros TESS (`32061`) recebe os dados da pesquisa e gera a estrutura completa do post: textos para cada slide do carrossel, hooks, CTA e a legenda principal.

*   **4.2. Criação do Briefing para Imagens (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: Com base no roteiro, o agente TESS (`32060`) cria descrições detalhadas (briefings) para as imagens de fundo de cada slide, garantindo coerência visual com o tema e o texto.

*   **4.3. Geração do HTML dos Slides (TESS)**
    *   **Tipo**: HTTP Request (POST)
    *   **Descrição**: O agente criador de HTML TESS (`32059`) combina o roteiro (textos) e os briefings de imagem para gerar o código HTML de cada slide do carrossel, já estilizado e pronto para ser convertido em imagem.

### **FASE 5: Processamento de Imagens**

Esta fase transforma o código HTML gerado em arquivos de imagem, faz o download e os armazena de forma organizada no Google Drive.

*   **5.1. Divisão dos Arquivos HTML**
    *   **Tipo**: Code Node
    *   **Função**: Separa a saída do agente, que pode conter múltiplos blocos `<html>...</html>`, em itens individuais para processamento em lote.

*   **5.2. Conversão de HTML para Imagem (PNG)**
    *   **Tipo**: Loop e HTML/CSS to Image
    *   **Descrição**: Em um loop, cada código HTML é enviado à API do Htmlcsstoimg (`https://hcti.io/v1/convert`) para ser convertido em uma imagem PNG de 1080x1080 pixels.

*   **5.3. Download das Imagens Geradas**
    *   **Tipo**: HTTP Request (GET)
    *   **Função**: O fluxo faz o download do arquivo binário de cada imagem a partir da URL retornada pela API de conversão.

*   **5.4. Criação de Pasta no Google Drive**
    *   **Tipo**: Google Drive Node
    *   **Operação**: `Create folder`
    *   **Função**: Cria uma pasta no Google Drive com um nome único para a execução, usando o tema, o ID da execução e a data atual.

*   **5.5. Upload das Imagens para o Google Drive**
    *   **Tipo**: Google Drive Node
    *   **Operação**: `Upload file`
    *   **Função**: Sobe cada imagem baixada para a pasta recém-criada no Google Drive.

### **FASE 6: Finalização**

A fase final é responsável por registrar o trabalho concluído em uma planilha e notificar as partes interessadas.

<img width="700" height="261" alt="image" src="https://github.com/user-attachments/assets/5217eae1-fc01-4451-9be9-8b6c8fc5e9f0" />

*   **6.1. Preparação dos Dados para Registro**
    *   **Tipo**: Code Node
    *   **Função**: Extrai os dados finais do conteúdo (tema, motivo da escolha, legenda) e o link da pasta do Google Drive para registrar na planilha.

*   **6.2. Registro em Google Sheets**
    *   **Tipo**: Google Sheets Node
    *   **Operação**: `Append row`
    *   **Função**: Adiciona uma nova linha à planilha de controle com todas as informações do conteúdo gerado, incluindo o link para as artes no Drive.

*   **6.3. Envio de Notificação por E-mail (Gmail)**
    *   **Tipo**: Gmail Node
    *   **Função**: Envia um e-mail formatado em HTML para a equipe, informando sobre a conclusão do processo e incluindo botões para acessar diretamente a pasta de artes e a planilha de registro.

*   **6.4. Envio de Notificação no Google Chat**
    *   **Tipo**: HTTP Request (POST)
    *   **Função**: Envia uma mensagem em formato de "card" para um espaço específico no Google Chat, com as mesmas informações e links do e-mail, permitindo uma notificação rápida e integrada.


---

## APIs Utilizadas

### 1. **TESS - Pareto**
- **Base URL**: `https://tess.pareto.io/api`
- **Autenticação**: Bearer Token
- **Endpoints Utilizados**:
  - `POST /agents/{agent_id}/execute` - Executa um agente
  - `GET /agent-responses/{response_id}` - Recupera resultado
- **Agentes Utilizados**:
  - 31523: Tendências da Semana
  - 31107: Hashtags Twitter
  - 31104: Identificador de Tendências
  - 31119: Curadoria de Conteúdo Instagram
  - 32061: Criador de Roteiro
  - 32060: Criador de Carrosséis [Fundo]
  - 32059: Criar Anúncios em HTML
  - 32754: Pesquisa Aprofundada
  - 33079: Analisador de Capa
  - 32601: Filtro de Temas
- **Timeout**: 220 segundos
- **Retry**: Habilitado
- **Documentação**: https://cd277b84.tess-docs.pages.dev/api

### 2. **SerpAPI**
- **Base URL**: `https://serpapi.com/search`
- **Autenticação**: API Key (query parameter)
- **Endpoints**:
  - `GET /search?engine=google_trends_trending_now&geo=BR`
- **Funcionalidade**: Busca tendências de pesquisa do Google
- **Parâmetros**:
  - `engine`: google_trends_trending_now
  - `geo`: BR (Brasil)
  - `api_key`: [Sua chave SerpAPI]
- **Formato de Resposta**: JSON com trending searches
- **Documentação**: https://serpapi.com/docs

### 3. **Apify**
- **Base URL**: `https://api.apify.com/v2`
- **Autenticação**: API Token
- **Endpoints**:
  - `POST /acts/{act_id}/run-sync` - Executa ator sincronamente (Instagram Scraper)
  - `GET /acts/{scraper_name}/runs/last` - Última execução
  - `GET /actor-runs/{run_id}/dataset/items` - Dataset de resultados
- **Ator Utilizado**: Instagram Scraper (`apify~instagram-scraper`)
- **Funcionalidade**: Scraping de posts do Instagram para alimentar a fase inicial de coleta.
- **Documentação**: https://docs.apify.com

### 4. **Google Drive API**
- **Base URL**: `https://www.googleapis.com/drive/v3`
- **Autenticação**: OAuth2
- **Funcionalidades**:
  - Create folder
  - Upload file
- **Permissões Necessárias**: `drive`
- **Documentação**: https://developers.google.com/drive

### 5. **Google Sheets API**
- **Base URL**: `https://sheets.googleapis.com/v4`
- **Autenticação**: OAuth2
- **Funcionalidades**:
  - Read range
  - Append row
- **Permissões Necessárias**: `spreadsheets`, `drive`
- **Documentação**: https://developers.google.com/sheets

### 6. **Google Chat API**
- **Base URL**: `https://chat.googleapis.com/v1`
- **Autenticação**: API Key + Token
- **Endpoint**: `POST /spaces/{space_id}/messages`
- **Funcionalidade**: Envia cards e mensagens em espaços
- **Documentação**: https://developers.google.com/chat

### 7. **Gmail API**
- **Base URL**: `https://gmail.googleapis.com/gmail/v1`
- **Autenticação**: OAuth2
- **Funcionalidade**: Envio de emails
- **Permissões**: `gmail.send`
- **Documentação**: https://developers.google.com/gmail

### 8. **Htmlcsstoimg**
- **Base URL**: `https://hcti.io/v1`
- **Autenticação**: API Key
- **Endpoint**: `POST /convert`
- **Funcionalidade**: Converte HTML/CSS em imagem (PNG/JPEG)
- **Parâmetros**:
  - `html`: Código HTML
  - `css`: (opcional) CSS adicional
  - `width`: Largura em pixels
  - `height`: Altura em pixels
- **Documentação**: https://htmlcsstoimg.com

---

## Credenciais Necessárias

Para que o fluxo funcione corretamente, é preciso configurar as seguintes credenciais no N8N.

### Configuração no N8N

#### 1. **TESS API - Pareto**
- **Tipo**: HTTP Header Authentication
- **Método de Autenticação**: Bearer Token
- **Header necessário**: `Authorization: Bearer {seu-token-tess}`
- **Nós que utilizam esta credencial**: 
  - TESS - Tendências da Semana
  - TESS - Hashtags Twitter (Semana)
  - TESS - Agente Identificador de Tendências
  - GET | TESS - Tendências da Semana
  - GET | TESS - Hashtags Twitter (Semana)
  - GET | TESS - Agente Identificador de Tendências
  - TESS - Agente de Curadoria de Conteúdo Instagram
  - TESS - Criador de Roteiro de Post Instagram
  - TESS - Agente Criador Carrosséis Instagram [FUNDO]
  - TESS - Criar anúncios de Imagem em HTML
  - TESS - Agente de Pesquisa Aprofundada (Gerar temas)
  - TESS - Agente de Pesquisa Aprofundada (Aprofundar)
  - TESS - Analisador de Capa dos Carrosséis
  - Agente de Filtro para Temas Relevantes

**Onde obter**: https://tess.im/

---

#### 2. **SerpAPI**
- **Tipo**: API Key
- **Método de Autenticação**: Query Parameter
- **Parâmetro**: `api_key={sua-chave-serpapi}`
- **Nó que utiliza**: Conexão SerpAPI

**Onde obter**: https://serpapi.com/dashboard

---

#### 3. **Apify**
- **Tipo**: API Token
- **Método de Autenticação**: Bearer Token ou Query Parameter
- **Token necessário**: `{seu-token-apify}`
- **Nós que utilizam esta credencial**:
  - POST | Execute Apify
  - GET | Last Run Apify
  - GET | Dataset Items Apify

**Onde obter**: https://apify.com/account/integrations

---

#### 4. **Google Drive OAuth2**
- **Tipo**: OAuth2 Authentication
- **Escopos Necessários**:
  - `https://www.googleapis.com/auth/drive`
  - `https://www.googleapis.com/auth/drive.file`
- **Nós que utilizam esta credencial**:
  - Create folder
  - Subir imagem no drive

**Onde obter**: Google Cloud Console (https://console.cloud.google.com)

---

#### 5. **Google Sheets OAuth2**
- **Tipo**: OAuth2 Authentication
- **Escopos Necessários**:
  - `https://www.googleapis.com/auth/spreadsheets`
  - `https://www.googleapis.com/auth/drive`
- **Nós que utilizam esta credencial**:
  - Atualizar planilha
  - Nome do Perfil do Instagram

**Onde obter**: Google Cloud Console (https://console.cloud.google.com)

---

#### 6. **Gmail OAuth2**
- **Tipo**: OAuth2 Authentication
- **Escopos Necessários**:
  - `https://www.googleapis.com/auth/gmail.send`
  - `https://www.googleapis.com/auth/gmail.readonly` (opcional)
- **Nó que utiliza**: Send a message1

**Onde obter**: Google Cloud Console (https://console.cloud.google.com)

---

#### 7. **Google Chat API**
- **Tipo**: HTTP Header Authentication com API Key
- **Parâmetros necessários**: 
  - `key`: {sua-chave-google-chat}
  - `token`: {seu-token-google-chat}
- **Nó que utiliza**: Enviar Mensagem Gchat

**Onde obter**: Google Cloud Console (https://console.cloud.google.com)

---

#### 8. **Htmlcsstoimg**
- **Tipo**: API Key
- **Método de Autenticação**: API Key
- **Chave necessária**: `{sua-chave-htmlcsstoimg}`
- **Nó que utiliza**: Gerar imagem pelo HTML

**Onde obter**: https://htmlcsstoimg.com/account

---

## Troubleshooting

Para erros identificados na automação, favor criar uma Issue associada a este agente no Github.

---

## Arquivo do Fluxo
> *Obs: Lembre-se de criar as credenciais anteriormente no N8N, isso facilitará na importação do JSON, permitindo que ele seja preenchido com elas.*

Baixe o arquivo JSON completo aqui:  
https://cdn.tess.im/assets/uploads/6fc9363a-bd09-4825-9148-cf438927bd58.json
