# Fluxo da Automação no N8N: Agente de Criação de Conteúdo (Brasilidades)

## 1. Visão Geral

Este documento detalha o fluxo de trabalho da automação construída na plataforma N8N, denominada **"[PARETO] Agente Criação de Conteúdo (Brasilidades)"**. O objetivo principal desta automação é identificar posts de referência no Instagram, extrair seu conteúdo, traduzi-lo, gerar novas mídias (imagens) baseadas no conteúdo original e, por fim, organizar e notificar sobre o novo conteúdo criado.

O fluxo é projetado para ser robusto, tratando diferentes tipos de posts (imagem única e carrossel) e garantindo que todas as etapas sejam registradas em uma [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=1213580918#gid=1213580918).

**Link de Acesso ao Fluxo:**
- [Arquivo JSON do Fluxo N8N](https://cdn.tess.im/assets/uploads/8bdc782a-b147-4f6a-842d-70edb6220f36.json)

---

## 2. Arquitetura do Fluxo

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
- **Configuração**: Executa a automação em dias e horários pré-definidos à meia noite (o usuário configura essa programação na [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=1213580918#gid=1213580918)).
- **Exemplo**: A cada segunda-feira, quinta-feira e domingo à meia-noite.

#### 1.2 Webhook
- **Nó**: `n8n-nodes-base.webhook`
- **Finalidade**: Permite que a automação seja iniciada por uma chamada de API externa, através do menu da [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=1213580918#gid=1213580918).
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

## 7. Configuração e Deploy

*Observação: É importante ressaltar que, para utilizar a planilha de Criação de Conteúdo, não é necessário ter acesso ao N8N. Mas sim, ter as permissões do Google AppScript configuradas.*

### 7.1 Pré-requisitos
1. Conta N8N (Self-hosted ou Cloud)
2. Credenciais configuradas para todas as APIs listadas na Seção 3
3. Planilhas do Google Sheets criadas:
   - "Perfis de Referência" - com lista de perfis do Instagram
   - "BRASILIDADES" - planilha de controle com as colunas mapeadas

### 7.2 Passos para Implementação
1. Importar o arquivo JSON do fluxo no N8N
2. Configurar as credenciais de cada API
3. Verificar os IDs e URLs de planilhas e pastas do Google
4. Testar o fluxo com uma execução manual
5. Ativar o gatilho de schedule ou webhook conforme necessário

### 7.3 Monitoramento
- Verificar os logs de execução no N8N para identificar possíveis erros
- Acompanhar a planilha "BRASILIDADES" para validar dados gerados
- Verificar o Google Chat para confirmar envio de notificações

---

## 8. Conclusão

Este fluxo automático de criação de conteúdo integra múltiplas APIs e agentes de IA para:
- ✅ Identificar conteúdo relevante no Instagram
- ✅ Traduzir e adaptar o conteúdo para português
- ✅ Gerar novas imagens baseadas no conteúdo original
- ✅ Registrar todo o processo em uma planilha de controle
- ✅ Notificar a equipe sobre conclusão

A robustez do fluxo é garantida através de validação de dados, retry automático e tratamento de erros em pontos críticos.

---
