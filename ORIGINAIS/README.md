# Agente de Criação de Conteúdo - Manual de Operação

> Este repositório contém a documentação completa do Agente de Criação de Conteúdo - Originais ("Content Spark - Originais"), uma automação projetada para gerar Carrosséis com **foco motivacional** para Instagram com alto potencial de engajamento, baseados em tendências do momento.

> Cabe destacar que esse foco motivacional foi pré-definido deviso ao objetivo original deste fluxo, com base nas boas práticas d emercaso. Ao duplicar o fluxo, é possível  alterar esse foco diretamente nos prompts dos agentes de IA, em caso de de desejar fazer uma mudança de objetivo.


---

## 📖 Índice

1.  [**Visão Geral e Objetivo**](#1-visão-geral-e-objetivo)
2.  [**Estratégia Principal**](#2-estratégia-principal)
3.  [**Manual de Operação (Para Usuários)**](#3-manual-de-operação-para-usuários)
    *   [Pré-requisitos](#pré-requisitos)
    *   [A Planilha de Controle: O Centro de Comando](#a-planilha-de-controle-o-centro-de-comando)
    *   [Modos de Operação](#modos-de-operação)
    *   [Gerenciando as Automações Agendadas](#gerenciando-as-automações-agendadas)
    *   [Solução de Problemas (Troubleshooting)](#solução-de-problemas-troubleshooting)
4.  [**Arquitetura e Ferramentas**](#4-arquitetura-e-ferramentas)
5.  [**Fluxo de Trabalho no N8N (Execução Técnica)**](#5-fluxo-de-trabalho-no-n8n-execução-técnica)
6.  [**Agentes de IA Utilizados**](#6-agentes-de-ia-utilizados)
7.  [**Exemplos de Saída da Automação**](#7-exemplos-de-saída-da-automação)
8.  [**Links e Recursos**](#8-links-e-recursos)

## 1. Visão Geral e Objetivo

Esta automação foi desenhada para resolver o desafio de criar conteúdo de alta qualidade e engajamento para o Instagram, um processo que tradicionalmente consome tempo e exige pesquisa constante de tendências.

O **Agente de Criação de Conteúdo** transforma temas e tendências atuais em conteúdo visual completo (carrosséis), de forma totalmente automatizada. O objetivo é fornecer, em minutos, um carrossel com 6 a 10 imagens, textos otimizados e legendas prontas para publicação, com foco em perfis de nicho motivacional. A sua única tarefa é monitorar, aprovar e publicar o conteúdo gerado.

## 2. Estratégia Principal

A automação opera com um fluxo estratégico que vai da descoberta da ideia à entrega do material pronto, minimizando a intervenção humana.

```
┌──────────────────────────────────────────────────────────────┐
│  1️⃣  Identifica Tendências      (Google, Twitter, etc.)       │
│                      ↓                                        │
│  2️⃣  Coleta Posts de Referência (Instagram Scraping)         │
│                      ↓                                        │
│  3️⃣  Analisa Padrões e Seleciona Tema (IA)                    │
│                      ↓                                        │
│  4️⃣  Cria Roteiro, Textos e Legenda (IA)                      │
│                      ↓                                        │
│  5️⃣  Gera Imagens para o Carrossel (IA)                       │
│                      ↓                                        │
│  6️⃣  Organiza e Registra      (Google Drive & Sheets)        │
│                      ↓                                        │
│  7️⃣  Notifica a Equipe          (Email & Google Chat)        │
└──────────────────────────────────────────────────────────────┘
```

O resultado final é um conteúdo original e coeso, baseado em dados de tendências, mas com uma execução criativa única.

## 3. Manual de Operação (Para Usuários)

### Pré-requisitos
*   **Acesso de Editor** à [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0).
*   Conta Google para acessar a planilha e o Google Drive.
*   Fazer parte do canal do Google Chat para receber as notificações (Opcional).

### A Planilha de Controle: O Centro de Comando
A [planilha](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0) é a interface principal para gerenciar toda a automação. As abas mais importantes são:

*   **Conteúdo Original**: Onde você acompanha, revisa e aprova todo o conteúdo gerado pela automação. Contém o tema, a legenda pronta, o link para as artes no Google Drive e as colunas de status.
*   **Perfis de Referência**: Lista de perfis do Instagram que a IA utiliza como inspiração para entender padrões visuais e de conteúdo. Você pode adicionar ou remover perfis nesta aba.
*   **Config**: Contém configurações técnicas da automação. **Não altere esta aba** sem orientação.

### Modos de Operação
Você pode gerar conteúdo de duas formas:

1.  **Execução Manual (Sob Demanda)**
    *   **Quando usar**: Para gerar conteúdo imediatamente, sem esperar o próximo ciclo agendado.
    *   **Como fazer**: Na planilha, acesse o menu **Pareto AI > ORIGINAIS > Executar fluxo**. Uma confirmação aparecerá, e o processo iniciará em segundo plano, levando de 20 a 25 minutos. Você será notificado via e-mail e Google Chat quando terminar.

2.  **Execução Automática (Agendada)**
    *   **Quando usar**: Este é o modo padrão, ideal para manter um fluxo constante de conteúdo.
    *   **Como funciona**: A automação é executada automaticamente toda **segunda-feira** e **quarta-feira** à meia-noite (00h00).

### Gerenciando as Automações Agendadas
Através do menu **"Pareto AI"** na planilha, você pode controlar os agendamentos:

*   **Pausar execução automática**: Interrompe os ciclos automáticos de segunda e quarta. É útil durante períodos de férias, pausas estratégicas ou para manutenções. A execução manual continua disponível.
*   **Ativar execução automática**: Reativa os ciclos automáticos caso eles estejam pausados.

### Solução de Problemas (Troubleshooting)
*   **Link de Artes Quebrado**: Verifique se você tem acesso à pasta do Google Drive. Se o erro persistir, pode ter ocorrido uma falha no upload. Tente gerar novamente ou abra uma issue.
*   **Menu "Pareto AI" não aparece**: Recarregue a planilha. Se o problema continuar, verifique se sua conta tem as permissões corretas.
*   **Execução manual falhou**: Aguarde alguns minutos e tente novamente. Se a falha for recorrente, contate o suporte técnico ou abra uma issue no Github.

## 4. Arquitetura e Ferramentas

A automação integra diversas ferramentas para orquestrar o fluxo de trabalho:

*   **Orquestrador**: N8N.
*   **Centro de Controle**: Google Sheets.
*   **Armazenamento de Mídia**: Google Drive.
*   **Fontes de Tendências**: Google Trends (via SerpAPI), Twitter API, API Pareto Trends.
*   **Coleta de Dados**: Apify (para scraping do Instagram).
*   **Geração de Imagem**: Htmlcsstoimg API (para converter HTML em PNG).
*   **Inteligência Artificial**: Modelos de linguagem para análise, pesquisa e criação de conteúdo.
*   **Notificações**: E-mail e Google Chat.

## 5. Fluxo de Trabalho no N8N (Execução Técnica)

O processo é dividido em fases sequenciais e paralelas, executadas dentro do N8N.

```
[INÍCIO: Gatilho Manual ou Agendado]
     |
     +---- [FASE 1: COLETA DE DADOS EM PARALELO] (~2 min)
     |     |
     |     +---> Google Trends (Top 10)
     |     +---> Twitter Hashtags
     |     +---> API Pareto Trends
     |     +---> Instagram Scraper (Posts recentes)
     |
     +---- [FASE 2: PROCESSAMENTO E SELEÇÃO] (~5 min)
     |     |
     |     +---> Consolida dados das fontes.
     |     +---> IA filtra ruídos e categorias inadequadas.
     |     +---> IA seleciona o tema final com maior potencial viral.
     |
     +---- [FASE 3: PESQUISA E CRIAÇÃO DO ROTEIRO] (~5 min)
     |     |
     |     +---> IA pesquisa a fundo o tema (história, dados, sentimento).
     |     +---> IA estrutura o carrossel (6-10 slides), escreve textos e a legenda.
     |     +---> IA cria os prompts detalhados para a geração de cada imagem.
     |
     +---- [FASE 4: GERAÇÃO VISUAL] (~8 min)
     |     |
     |     +---> Para cada slide, gera um HTML com o design.
     |     +---> Converte cada HTML em uma imagem PNG (1080x1080).
     |
     +---- [FASE 5: ARMAZENAMENTO E REGISTRO]
     |     |
     |     +---> Cria uma pasta no Google Drive.
     |     +---> Faz upload das imagens geradas.
     |     +---> Registra todas as informações na planilha Google Sheets.
     |
     +---- [FASE 6: NOTIFICAÇÃO] (~1 min)
     |     |
     |     +---> Envia e-mail para a equipe.
     |     +---> Envia mensagem para o canal do Google Chat.
     |
[FIM]
```
**Tempo Total Estimado**: 20-25 minutos.

## 6. Agentes de IA Utilizados

A automação emprega múltiplos agentes de IA especializados em diferentes tarefas ao longo do fluxo:

*   **Agente Analista de Tendências**: Filtra e cruza dados de diferentes fontes (Google, Twitter) para identificar os temas com maior potencial de engajamento.
*   **Agente Curador de Conteúdo**: Analisa os posts de referência e os padrões visuais dos perfis de inspiração para guiar o estilo do conteúdo a ser criado.
*   **Agente Pesquisador**: Aprofunda-se no tema selecionado, buscando dados, contexto histórico e ângulos únicos para enriquecer o conteúdo.
*   **Agente Roteirista e Copywriter**: Estrutura a narrativa do carrossel, cria os textos para cada slide (hooks, desenvolvimento, CTA) e redige a legenda final para o Instagram.
*   **Agente Designer Gráfico**: Traduz as descrições de texto em composições visuais (via código HTML) que são então convertidas em imagens, seguindo uma identidade visual coesa.

## 7. Exemplos de Saída da Automação

*(Esta seção será preenchida com exemplos concretos de carrosséis e legendas gerados pela automação).*

## 8. Links e Recursos

*   **Planilha de Controle**: [Link para a Planilha](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0)
*   **Repositório no Github**: [Link para o Repositório](https://github.com/liviatagliari/pareto_content_spark_originais)
*   **Documentação Técnica do Fluxo**: [Fluxo N8N no Github](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/ORIGINAIS/Fluxo_N8N.md)
