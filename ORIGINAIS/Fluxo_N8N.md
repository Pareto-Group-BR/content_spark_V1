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
│    Programação Diária à meia noite (Seg e Qua)                  │
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

## Etapas Detalhadas

### **FASE 1: COLETA INICIAL DE DADOS (TENDÊNCIAS + POSTS DO INSTAGRAM)**

#### 1.1 Programação Diária à meia noite
- **Tipo**: Gatilho (Trigger) por Agendamento
- **Frequência**: Segundas e Quartas-feiras à meia noite
- **Função**: Inicia o fluxo automaticamente
- **Saída**: Dispara 4 fluxos paralelos:
  - TESS - Tendências da Semana
  - TESS - Hashtags Twitter (Semana)
  - Conexão SerpAPI
  - Format Apify run input | Instagram Scraper (ramo Apify / Instagram)

#### 1.2 TESS - Tendências da Semana
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/31523/execute`
- **Método**: POST
- **Descrição**: Obtém a lista de tendências da semana através do agente TESS
- **Payload**:
  ```json
  {
    "messages": [
      {
        "role": "user",
        "content": "Oi, pode me enviar a lista de tendências da semana?"
      }
    ],
    "wait_execution": false
  }
  ```
- **Próximo Nó**: Wait (45s)
- **Retry**: Habilitado

#### 1.3 TESS - Hashtags Twitter (Semana)
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/31107/execute`
- **Método**: POST
- **Descrição**: Coleta as hashtags mais relevantes do Twitter/X
- **Payload**:
  ```json
  {
    "messages": [
      {
        "role": "user",
        "content": "Oi, pode me enviar a lista de hashtags?"
      }
    ],
    "wait_execution": false
  }
  ```
- **Próximo Nó**: Wait1
- **Retry**: Habilitado

#### 1.4 Conexão SerpAPI
- **Tipo**: HTTP Request (GET)
- **API**: `https://serpapi.com/search`
- **Parâmetros**:
  - `engine`: google_trends_trending_now
  - `geo`: BR (Brasil)
  - `api_key`: [Chave da SerpAPI]
- **Descrição**: Busca as tendências de pesquisa do Google em tempo real
- **Próximo Nó**: Code in JavaScript
- **Formato de Resposta**: JSON com trending searches

#### 1.5 Code in JavaScript - Preparação dos dados do Google Trends
- **Tipo**: Code Node (JavaScript)
- **Função**: Filtra e limpa as tendências vindas da SerpAPI, removendo categorias indesejadas (por exemplo, ids 17 e 20 conforme configurado no fluxo)
- **Lógica principal**:
  ```javascript
  const trendingData = item.json;

  if (trendingData && trendingData.trending_searches) {
    const dadosFiltrados = trendingData.trending_searches.filter(searchItem => {
      return !searchItem.categories.some(categoria =>
        categoria.id === 17 || categoria.id === 20
      );
    });

    item.json.trending_searches = dadosFiltrados;
  }

  return items;
  ```
- **Próximo Nó**: Output2 (Set) → JSON Parse4 → Agente de Filtro para Temas Relevantes (nas fases seguintes)

#### 1.6 Format Apify run input | Instagram Scraper
- **Tipo**: Code Node (JavaScript)
- **Descrição**: Formata os parâmetros de entrada para o scraper do Instagram (Apify). Esse nó já é disparado diretamente pelo gatilho inicial, em paralelo com as coletas de TESS e SerpAPI.
- **Configuração (exemplo)**:
  ```javascript
  const run_input = {
    "addParentData": false,
    "directUrls": [
      "https://www.instagram.com/valorgi",
      "https://www.instagram.com/motivation_mondays/",
      "https://www.instagram.com/motivationmafia/",
      // ... mais URLs
    ],
    "enhanceUserSearchWithFacebookPage": false,
    "isUserReelFeedURL": false,
    "isUserTaggedFeedURL": false,
    "onlyPostsNewerThan": "2 days",
    "resultsLimit": 6,
    "resultsType": "posts",
    "searchLimit": 1,
    "searchType": "hashtag"
  }

  return { run_input }
  ```
- **Próximo Nó**: POST | Execute Apify

#### 1.7 POST | Execute Apify
- **Tipo**: HTTP Request (POST)
- **API**: `https://api.apify.com/v2/acts/shu8hvrXbJbY3Eb9W/run-sync`
- **Descrição**: Executa o Instagram Scraper do Apify para coletar posts de referência logo no início do fluxo, em paralelo às demais coletas (TESS, SerpAPI).
- **Body**:
  - `jsonBody`: `={{ $json.run_input }}` (objeto preparado no nó anterior)
- **Parâmetros principais (via `run_input`)**:
  - Hashtags e perfis para buscar
  - Limite de resultados: 6
  - Apenas posts dos últimos 2 dias
  - Tipo de resultado: posts
- **Próximo Nó**: Wait5

#### 1.8 Wait5
- **Tipo**: Wait
- **Duração**: 10 segundos (conforme configurado no fluxo)
- **Função**: Dá um pequeno intervalo antes de consultar o status da última execução do scraper no Apify, garantindo que o run esteja registrado.
- **Próximo Nó**: GET | Last Run Apify

#### 1.9 GET | Last Run Apify
- **Tipo**: HTTP Request (GET)
- **API**: `https://api.apify.com/v2/acts/apify~instagram-scraper/runs/last?token={seu-token-apify}`
- **Descrição**: Obtém informações sobre a última execução do scraper do Instagram.
- **Próximo Nó**: If (validação de conclusão)

#### 1.10 If (Validação de conclusão do Apify)
- **Tipo**: Nó Condicional (IF)
- **Condições (baseadas no JSON do Apify)**:
  - `data.status` = `"SUCCEEDED"`
- **Branches**:
  - ✅ **SIM**: Prossegue para GET | Dataset Items Apify
  - ❌ **NÃO**: Volta para GET | Last Run Apify (loop de retry até a conclusão)
- **Função**: Garante que os posts só sejam consumidos depois da execução bem-sucedida do scraper.

#### 1.11 GET | Dataset Items Apify
- **Tipo**: HTTP Request (GET)
- **API**:  
  `https://api.apify.com/v2/actor-runs/{{ $json.data.id }}/dataset/items?format=json`
- **Descrição**: Recupera os posts coletados pelo scraper do Instagram.
- **Headers** (exemplo):
  - `Accept: application/json`
  - `Authorization: Bearer {seu-token-apify}`
- **Formato**: JSON com array de posts (caption, imagens, likes, hashtags, etc.)
- **Próximo Nó**: instagram_posts

#### 1.12 instagram_posts (Set Variable)
- **Tipo**: Set Node
- **Campo**: `instagram_posts`
- **Valor**: Array de posts do Instagram retornados pelo dataset do Apify
- **Função**: Normaliza a estrutura de saída para encaixar no Merge de dados de tendências.
- **Próximo Nó**: Filter1

#### 1.13 Filter1
- **Tipo**: Filter Node
- **Função**: Filtra posts de Instagram por critérios de qualidade conforme configurado no fluxo (por exemplo: posts não fixados, ou outros campos como likes/engajamento).
- **Próximo Nó**: Aggregate

#### 1.14 Aggregate
- **Tipo**: Aggregate Node
- **Função**: Consolida todos os posts filtrados em um array único
- **Output**: Estrutura unificada de `instagram_posts` pronta para entrar no nó Merge junto com:
  - `tendencias_tess`
  - `hashtags`
  - `top_10_trends`
- **Próximo Nó**: Merge (na fase de consolidação de dados)

---

### **FASE 2: PROCESSAMENTO E AGUARDAMENTO**

#### 2.1 Wait Nodes (Wait, Wait1, Wait 30s etc.)
- **Tipo**: Wait (Espera)
- **Duração**: 30–60 segundos (conforme cada nó)
- **Função**: Aguarda a conclusão da execução dos agentes TESS antes de buscar resultados via `GET /agent-responses`.
- **Motivo**: As APIs TESS executam de forma assíncrona e podem levar alguns segundos até o output estar disponível.

#### 2.2 GET | TESS - Tendências da Semana
- **Tipo**: HTTP Request (GET)
- **API**: `https://tess.pareto.io/api/agent-responses/{response_id}`
- **Descrição**: Recupera o resultado da execução do agente de tendências.
- **URL Dinâmica**: Usa o ID da resposta anterior (`responses[0].id`).
- **Próximo Nó**: If Output is Not Empty

#### 2.3 If Output is Not Empty (Validações)
- **Tipo**: Nó Condicional (IF)
- **Condição**: Verifica se o campo `output` não está vazio
- **Branches**:
  - ✅ **SIM**: Continua com a etapa seguinte
  - ❌ **NÃO**: Volta ao wait e tenta novamente

#### 2.4 Output2 (Set Variable)
- **Tipo**: Set Variable
- **Função**: Armazena os trending searches filtrados do Google Trends em um campo estruturado para uso posterior.
- **Próximo Nó**: JSON Parse4 / Agente de Filtro para Temas Relevantes

---

### **FASE 3: MERGE E CONSOLIDAÇÃO DE DADOS**

#### 3.1 tendencias_tess (Set Variable)
- **Tipo**: Set Variable
- **Campo**: `tendencias_tess`
- **Valor**: Output da validação TESS (Tendências da Semana)
- **Próximo Nó**: Merge

#### 3.2 hashtags twitter (Set Variable)
- **Tipo**: Set Variable
- **Campo**: `hashtags`
- **Valor**: Output das hashtags do Twitter
- **Próximo Nó**: Merge

#### 3.3 top_10_trends (Set Variable)
- **Tipo**: Set Variable
- **Campo**: `top_10_trends`
- **Valor**: Top 10 tendências filtradas do Google Trends
- **Próximo Nó**: Merge

#### 3.4 instagram_posts (via Aggregate)
- **Origem**: Pipeline Apify já executado na Fase 1 (Format Apify → Execute Apify → GET Last Run → GET Dataset Items → Filter1 → Aggregate).
- **Campo**: `instagram_posts`
- **Conteúdo**: Array consolidado de posts do Instagram para servir como base de referência visual e textual.
- **Destino**: Uma das entradas do Merge.

#### 3.5 Merge
- **Tipo**: Merge Node
- **Modo**: Combinar por Posição (`combineByPosition`)
- **Número de Entradas**: 4
- **Função**: Consolida todos os dados coletados em um único objeto:
  - `tendencias_tess`
  - `hashtags`
  - `top_10_trends`
  - `instagram_posts` (via Apify)
- **Próximo Nó**: JSON Stringify

#### 3.6 JSON Stringify
- **Tipo**: Code Node
- **Função**: Converte o objeto JavaScript em string JSON, concatenando:
  - dados da API do Google Trends,
  - dados da pesquisa TESS,
  - hashtags do Twitter,
  - posts de perfis públicos do Instagram.
- **Próximo Nó**: TESS - Agente Identificador de Tendências

---

### **FASE 4: IDENTIFICAÇÃO E SELEÇÃO DE TENDÊNCIAS**

#### 4.1 TESS - Agente Identificador de Tendências
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/31104/execute`
- **Descrição**: Agente IA que identifica as tendências mais relevantes a partir dos dados consolidados.
- **Payload (conceitual)**:
  ```json
  {
    "messages": [
      {
        "role": "user",
        "content": "Processar tendências e identificar padrões"
      }
    ],
    "wait_execution": true
  }
  ```
- **Próximo Nó**: Wait 30s
- **Retry**: Habilitado

#### 4.2 JSON Formatter
- **Tipo**: Code Node
- **Função**: Formata a resposta JSON do agente, removendo possíveis marcações de markdown (``` ``` ```json) e garantindo que a estrutura seja um JSON válido.
- **Próximo Nó**: suggested_themes (Split Out)

#### 4.3 Agente de Filtro para Temas Relevantes
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/32601/execute`
- **Função**: Filtra os temas mais relevantes a partir do conjunto de tendências identificadas.
- **Próximo Nó**: top_10_trends (Set) / fluxo de curadoria

#### 4.4 suggested_themes (Split Out)
- **Tipo**: Split Out
- **Campo**: `suggested_themes`
- **Função**: Divide cada tema sugerido em um item separado, preparando para o processamento em lote.
- **Próximo Nó**: Loop Over Items1

---

### **FASE 5: CURADORIA E ANÁLISE DE CONTEÚDO**

#### 5.1 Loop Over Items1 (Split in Batches)
- **Tipo**: Split In Batches
- **Função**: Processa cada tema sugerido em iteração.
- **Próximo Nó**: Filter ou reference_posts

#### 5.2 reference_posts (Split Out)
- **Tipo**: Split Out
- **Campo**: `reference_posts`
- **Função**: Separa cada post de referência para análise individual.
- **Próximo Nó**: Instagram Post (IF)

#### 5.3 Instagram Post (IF Conditional)
- **Tipo**: Nó Condicional
- **Função**: Verifica se o item é um post válido do Instagram de acordo com os campos esperados (URL, imagem, etc.).
- **Próximo Nó**: post_url ou Loop Over Items1 (skip)

#### 5.4 post_url (IF Conditional)
- **Tipo**: Nó Condicional
- **Função**: Valida a URL do post (por exemplo, se é `post_url` ou `display_url`).
- **Branch 1**: TESS - Analisador de Capa dos Carrosséis
- **Branch 2**: display_url

#### 5.5 TESS - Analisador de Capa dos Carrosséis
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/33079/execute`
- **Descrição**: Analisa a capa/thumbnail do carrossel:
  - Cores, composição, legibilidade, elementos visuais
- **Próximo Nó**: Loop Over Items1 (retorna para o fluxo de temas)

#### 5.6 display_url (IF Conditional)
- **Tipo**: Nó Condicional
- **Função**: Processa URLs de exibição de imagens (`display_url`) para análise complementar.
- **Branch 1**: TESS - Analisador de Capa dos Carrosséis1
- **Branch 2**: Loop Over Items1

#### 5.7 Filter & Include Image Description
- **Tipo**: Filter + Code Node
- **Função**:
  - Filtra itens com base em flags como `isPinned` etc.
  - Adiciona descrições de imagem geradas por IA aos objetos de post (campo `image_description`), percorrendo os `reference_posts` no array resultante.
- **Próximo Nó**: JSON Parse

---

### **FASE 6: SELEÇÃO DO TEMA E PESQUISA APROFUNDADA**

#### 6.1 JSON Parse
- **Tipo**: Code Node
- **Função**: Converte o output JSON da filtragem e enriquecimento (descrições de imagem) em um objeto pronto para ser enviado ao agente de curadoria.
- **Próximo Nó**: TESS - Agente de Curadoria de Conteúdo Instagram

#### 6.2 TESS - Agente de Curadoria de Conteúdo Instagram
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/31119/execute`
- **Descrição**: Seleciona o melhor tema entre as tendências identificadas, com base em:
  - Relevância de tendência
  - Qualidade dos posts de referência
  - Potencial viral
  - Alinhamento com audiência
- **Output**: Um tema selecionado com justificativa e metadados.
- **Próximo Nó**: Pré Pesquisa Aprofundada

#### 6.3 Pré Pesquisa Aprofundada
- **Tipo**: Code Node
- **Função**: Prepara o payload para o agente de pesquisa aprofundada (estrutura de `chosen_theme`, `motivo_da_escolha`, etc.).
- **Próximo Nó**: TESS - Agente de Pesquisa Aprofundada - Gerar temas

#### 6.4 TESS - Agente de Pesquisa Aprofundada - Gerar temas
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/32754/execute`
- **Descrição**: Gera temas alternativos e inicia a cadeia de aprofundamento.
- **Output**: Lista de temas com análise detalhada.
- **Próximo Nó**: TESS - Agente de Pesquisa Aprofundada - Aprofundar

#### 6.5 TESS - Agente de Pesquisa Aprofundada - Aprofundar
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/32754/execute`
- **Descrição**: Realiza análise aprofundada do tema selecionado:
  - Histórico e origem da tendência
  - Dados estatísticos
  - Sentimento do público
  - Oportunidades de nicho
- **Próximo Nó**: Wait 60s + GET | TESS - Agente Identificador de Tendências1

#### 6.6 Pós Pesquisa Aprofundada
- **Tipo**: Code Node
- **Função**: Formata o resultado da pesquisa aprofundada para alimentar o criador de roteiro (tema, dados de pesquisa, etc.).
- **Próximo Nó**: TESS - Criador de Roteiro de Post Instagram

---

### **FASE 7: CRIAÇÃO DE ROTEIRO E BRIEFING**

#### 7.1 TESS - Criador de Roteiro de Post Instagram
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/32061/execute`
- **Descrição**: Cria o roteiro completo do post com:
  - Estrutura do carrossel
  - Textos para cada slide
  - Hooks visuais
  - CTA (Call to Action)
  - Legenda principal
- **Output**: JSON com roteiro estruturado.
- **Próximo Nó**: Pós criação de roteiro

#### 7.2 Pós criação de roteiro
- **Tipo**: Code Node
- **Função**: Prepara briefing para criação de imagens de fundo do carrossel, com instruções claras sobre coerência entre tema, textos e imagens.
- **Instrução (conceitual)**:
  ```
  "Transformar o tema escolhido em um roteiro completo para carrossel,
  onde as imagens sejam coerentes com o tema, textos e entre si."
  ```
- **Próximo Nó**: TESS - Agente Criador Carrosséis Instagram [FUNDO]

#### 7.3 TESS - Agente Criador Carrosséis Instagram [FUNDO]
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/32060/execute`
- **Descrição**: Gera descrições detalhadas para as imagens de fundo de cada slide.
- **Output** (por slide):
  - `tipo_imagem_fundo`: Descrição rica para geração via IA
  - `estilo`: Paleta de cores / mood
  - `composição`: Elementos visuais principais
  - `tipografia`: Recomendações de uso de texto no visual
- **Próximo Nó**: Wait 30s1 → GET | TESS - Agente Criador Carrosséis Instagram

---

### **FASE 8: GERAÇÃO DE HTML E ANÚNCIOS**

#### 8.1 GET | TESS - Agente Criador Carrosséis Instagram
- **Tipo**: HTTP Request (GET)
- **API**: Recupera resultado da geração de imagens de fundo e especificações de design.
- **Próximo Nó**: If Output is Not Empty3

#### 8.2 If Output is Not Empty3
- **Tipo**: Nó Condicional
- **Função**: Garante que o `output` do agente de criação de carrosséis [FUNDO] não está vazio antes de seguir.
- **Branch SIM**: Nome do Perfil do Instagram

#### 8.3 Nome do Perfil do Instagram
- **Tipo**: Google Sheets Read
- **Função**: Obtém o nome do perfil de Instagram configurado na aba de Config do Google Sheets.
- **Próximo Nó**: JSON Parse5

#### 8.4 JSON Parse5
- **Tipo**: Code Node
- **Função**: Monta o payload final para o agente de criação de anúncios em HTML, combinando:
  - briefing do roteiro
  - URLs das imagens de fundo
  - nome do perfil do Instagram
- **Próximo Nó**: TESS - Criar anúncios de Imagem em HTML

#### 8.5 TESS - Criar anúncios de Imagem em HTML
- **Tipo**: HTTP Request (POST)
- **API**: `https://tess.pareto.io/api/agents/32059/execute`
- **Descrição**: Gera HTML para cada slide do carrossel.
- **Saída**: HTMLs prontos para:
  - Conversão em imagem (PNG)
  - Uso em ferramentas de design
  - Testes A/B
- **Próximo Nó**: Wait 30s2 → GET | TESS - Criar anúncios de Imagem em HTML → Dividir saída em arquivos HTML diferentes

---

### **FASE 9: CONVERSÃO DE HTML PARA IMAGEM**

#### 9.1 Dividir saída em arquivos HTML diferentes
- **Tipo**: Code Node
- **Função**: Separa múltiplos documentos HTML concatenados em itens individuais.
- **Lógica**:
  - Usa regex para identificar blocos `<html>...</html>`.
  - Retorna um item do n8n para cada HTML encontrado.
- **Output**: Array de itens com campo `html` contendo o código de cada slide.
- **Próximo Nó**: Loop Over Items

#### 9.2 Loop Over Items
- **Tipo**: Split In Batches
- **Função**: Processa cada HTML em iteração (batch).
- **Próximo Nó**: Gerar imagem pelo HTML

#### 9.3 Gerar imagem pelo HTML
- **Tipo**: HTML CSS to Image (Htmlcsstoimg)
- **API**: `https://hcti.io/v1/convert`
- **Parâmetros principais**:
  - `html_content`: Código HTML do slide
  - `viewport_height`: 1080px
  - `viewport_width`: 1080px
  - `response_format_html`: png
- **Output**: URL da imagem gerada
- **Próximo Nó**: Baixar imagem

#### 9.4 Baixar imagem
- **Tipo**: HTTP Request (GET)
- **URL Dinâmica**: `{{ $json.image_url }}`
- **Função**: Faz download do binário da imagem gerada.
- **Próximo Nó**: Subir imagem no drive

---

### **FASE 10: ARMAZENAMENTO EM GOOGLE DRIVE**

#### 10.1 Subir imagem no drive
- **Tipo**: Google Drive
- **Operação**: Upload de arquivo
- **Credenciais**: Google Drive OAuth2 API
- **Destino**: Pasta criada para este conteúdo (ver Create folder)
- **Próximo Nó**: Wait4 → Loop Over Items (para continuar o processamento dos demais HTMLs)

#### 10.2 Create folder
- **Tipo**: Google Drive
- **Operação**: Create folder
- **Nome da Pasta**:
  - `{{ $json.tema }} - {{ $execution.id }} - {{ $today.format('dd/MM/yyyy') }}`
- **Exemplo**:
  - `Tendência XYZ - exec-123456 - 27/01/2026`
- **Próximo Nó**: Dividir saída em arquivos HTML diferentes

---

### **FASE 11: REGISTRO EM GOOGLE SHEETS**

#### 11.1 Dados do conteúdo
- **Tipo**: Code Node
- **Função**: Extrai dados principais do conteúdo criado a partir dos nós:
  - Pós Pesquisa Aprofundada
  - TESS - Criador de Roteiro de Post Instagram
- **Variáveis Extraídas**:
  - `tema`: Tema selecionado
  - `motivo_da_escolha`: Justificativa
  - `legenda`: Texto completo da legenda
- **Próximo Nó**: Create folder (pasta que será referenciada na planilha)

#### 11.2 Atualizar planilha
- **Tipo**: Google Sheets
- **Operação**: Append row
- **Credenciais**: Google Sheets OAuth2 API
- **Planilha ID**: [ID configurado no seu ambiente]
- **Aba**: Página / Config definida no fluxo
- **Colunas Preenchidas (exemplo)**:
  | Coluna                     | Valor                          |
  |----------------------------|--------------------------------|
  | Tema                       | Nome do tema                   |
  | Motivo da Seleção do Tema | Justificativa                  |
  | Legenda                    | Texto da legenda               |
  | ID da criação              | ID da execução (`$execution`) |
  | Artes                      | Link da pasta no Drive        |
  | Plataforma                 | Instagram                      |
  | Data de Criação           | Data/hora atual                |

- **Próximo Nó**: Send a message1

---

### **FASE 12: NOTIFICAÇÕES**

#### 12.1 Send a message1
- **Tipo**: Gmail
- **Função**: Envia email com relatório da criação de conteúdo.
- **Destinatários**: Configurados no ambiente (ex.: time de marketing).
- **Assunto**: "Finalização da Geração do Carrossel"
- **Corpo**: HTML formatado com:
  - Tema do carrossel
  - Botões para acessar as artes (pasta do Drive)
  - Link para planilha de registro
  - Branding profissional da Pareto
- **Próximo Nó**: Enviar Mensagem Gchat

#### 12.2 Enviar Mensagem Gchat
- **Tipo**: HTTP Request (POST)
- **API**: `https://chat.googleapis.com/v1/spaces/{space_id}/messages`
- **Função**: Envia notificação no Google Chat para o espaço configurado.
- **Formato**: Card com:
  - Logo profissional
  - Título: "Nova Criação de Conteúdo - ORIGINAIS"
  - Tema do carrossel
  - Botão para acessar artes no Drive
  - Botão para abrir planilha de registro
- **Próximo Nó**: FIM

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

## 5. Credenciais Necessárias

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

## 6. Troubleshooting

Para erros identificados na automação, favor criar uma Issue associada a este agente no Github.

---

## 7. Arquivo do Fluxo

Baixe o arquivo JSON completo aqui:  
https://cdn.tess.im/assets/uploads/6fc9363a-bd09-4825-9148-cf438927bd58.json
