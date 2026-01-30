# Agente de Criação de Conteúdo - Manual de Operação

> Este repositório contém a documentação completa do Agente de Criação de Conteúdo - Originais ("Content Spark - Originais"), uma automação projetada para gerar Carrosséis com **foco motivacional** para Instagram com alto potencial de engajamento, baseados em tendências do momento.

> Cabe destacar que esse foco motivacional foi pré-definido devido ao objetivo original deste fluxo, com base nas boas práticas de mercaso. **Ao duplicar o fluxo, é possível  alterar esse foco Motivacional diretamente nos prompts dos agentes de IA, em caso de de desejar fazer uma mudança de objetivo.**

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

*OBS: O fluxo NÃO inclui a publicação automática no Instagram, esse processo exige a validação manual do gestor do Instagram.*

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
*   **Acesso de Editor** à [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0) e Conta Google para acessar a planilha e o Google Drive.

Também é necessário liberar as permissões do AppScript, o que pode ser feito via menu da planilha **"Permissões do Script"**. Essa liberação é feita uma única vez, ao utilizar a planilha pela primeira vez.

<img width="319" height="83" alt="image" src="https://github.com/user-attachments/assets/9b342bb8-9a77-48d3-8e21-97906da01e8b" />
  
*   Fazer parte do canal do Google Chat para receber as notificações (Opcional para receber as notificações).


> **Importante:** Não é necessário ter acesso direto ao ambiente do n8n. Toda a interação é feita através desta planilha.
  

### A Planilha de Controle: O Centro de Comando
A [planilha](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0) é a interface principal para gerenciar toda a automação. As abas mais importantes são:

*   **Abas de Resultados:** Nela você encontrará os conteúdos gerados, separados nas abas: `ORIGINAL`, `BRASILIDADES` e `SUGESTÃO`. Para o fluxo atual, concentre-se na aba ORIGINAIS. Lá, você verifica o tema, a legenda pronta, o link para as artes no Google Drive e as colunas de status.
*   **Perfis de Referência**: Lista de perfis do Instagram que a IA utiliza como inspiração para entender padrões visuais e de conteúdo. Você pode adicionar ou remover perfis nesta aba.
*   **Config**: Contém configurações técnicas da automação. **Não altere esta aba** sem orientação.
*   **Controles Manuais:** A planilha contém botões para controlar as automações, localizados no menu superior **"Pareto AI"**.

> **Importante:** Não altere ou inclua as colunas da planilha pois isso vai prejudicar a execução do fluxo.

### Modos de Operação
Você pode gerar conteúdo de duas formas:

1.  **Execução Manual via Menu (Sob Demanda)**
   *   **Acesse a planilha** Qualquer página, mas o registro da criação ficará na aba ORIGINAIS.
   *   **Selecione o menu:** Na parte superior, selecione `Pareto AI` -> `ORIGINAIS`.
   *   **Execute:** Escolha a opção `Executar fluxo`.
   *   **Aguarde:** Uma mensagem de aviso informará que o fluxo foi iniciado (ele poderá levar de 20 a 25 minutos). Você poderá fechar o pop up (inclusive, a planilha pode ser fechada sem nenhum problema);
   *   **Verifique o Resultado:** Após rodar a automação, o conteúdo gerado aparecerá na última linha preenchida da aba `ORIGINAIS`. Também serão enviados 2 avisos (e-mail e Google Chat) após a finalização da criação.


2.  **Execução Automática (Agendada)**
    *   **Quando usar**: Este é o modo padrão, ideal para manter um fluxo constante de conteúdo.
    *   **Como funciona**: A automação é executada automaticamente em todos os dias programados (à escolha do usuário atrabés do Menu da [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=1213580918#gid=1213580918)) à meia-noite (00h00). É possível selecionar quantos quiser via menu da planilha: **Segunda, Terça,Quarta, Quinta, Sábado e Domingo**.

### Gerenciando as Automações Agendadas
Através do menu **"Pareto AI"** na planilha, você pode controlar os agendamentos:

*   **Pausar execução automática**: Interrompe os ciclos automáticos agendados. É útil durante períodos de férias, pausas estratégicas ou para manutenções. A execução manual continua disponível.
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

## Exemplo de Carrossel Gerado

Abaixo, um exemplo da saída completa da automação, desde a escolha do tema até a legenda final para o Instagram.

| Campo | Descrição |
| :--- | :--- |
| **Tema** | `Disciplina silenciosa: O segredo dos vencedores que ninguém vê` |
| **Artes** | [Link para a pasta com as imagens](https://drive.google.com/drive/folders/1FzK_F_TEUh0eoREpGsvrOUe7cuP3k_R4) |
| **Motivo da Seleção** | A análise de tendências indicou alto engajamento com o tema "disciplina silenciosa" em perfis motivacionais. A escolha foi validada por posts de referência com milhares de interações, como o de `@agentsteven` (8.348 likes), confirmando a ressonância do tópico com o público. |
| **Legenda** | "Enquanto muitos buscam reconhecimento, os verdadeiros vencedores treinam no silêncio. 💭<br><br>Disciplina não é sobre aplausos — é sobre persistir mesmo sem plateia. É no escuro dos bastidores que a luz do sucesso começa a brilhar. ✨<br><br>Qual parte da sua rotina ninguém vê, mas você sabe que te transforma?<br><br>#DisciplinaSilenciosa #Autodomínio #CrescimentoPessoal #FocoInterior" |
| **Dados da Criação** | <ul><li>**ID:** `411221`</li><li>**Data:** `12-01-2026`</li><li>**Plataforma:** `Instagram`</li></ul> |

### Artes Geradas no Exemplo

<img width="360" height="360" alt="77964a49-8325-4522-8104-c33c88aeede7" src="https://github.com/user-attachments/assets/c31d7ae5-3f0b-4349-bb13-b51529e8e2e9" />
<img width="360" height="360" alt="8e3cbde6-4c1e-4e25-9730-ae9d81c23eca" src="https://github.com/user-attachments/assets/bf8efc81-c686-4721-b001-85aa71cb7f85" />
<img width="360" height="360" alt="1866ea6a-9d0a-4101-a44e-b9e736aab0f6" src="https://github.com/user-attachments/assets/c4b2e072-ddef-4813-842b-3bd137acd61f" />
<img width="360" height="360" alt="cba67a10-6cb0-4982-a490-17c1babd794a" src="https://github.com/user-attachments/assets/a8473d73-af9a-4701-ace0-08690e593647" />
<img width="360" height="360" alt="e1f94b53-213f-4c11-94a3-4c5029fb8cba" src="https://github.com/user-attachments/assets/9a883ad9-432c-4dc8-a217-6b3bafbee9fb" />
<img width="360" height="360" alt="6f00aa92-36d4-494d-b60d-d8e3ee26d542" src="https://github.com/user-attachments/assets/e2b8a143-adc5-4f9a-af3a-b34426b371f3" />
<img width="360" height="360" alt="8e3cbde6-4c1e-4e25-9730-ae9d81c23eca" src="https://github.com/user-attachments/assets/c776e563-217d-4948-8926-e22b0d3da23e" />
<img width="360" height="360" alt="005e9ffa-ae1d-49b5-9f16-75b136422a1d" src="https://github.com/user-attachments/assets/cd54eb66-db14-4ff1-b0dc-c8576303871e" />
<img width="360" height="360" alt="566e8258-a476-4c97-b5a5-13b0b938a1db" src="https://github.com/user-attachments/assets/1d842b9a-4c5f-42b1-aa26-b66bd5caec8a" />


---


## 8. Links e Recursos

*   **Planilha de Controle**: [Link para a Planilha](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0)
*   **Repositório Completo do "Content Spark V1" no Github**: [Link para o Repositório](https://github.com/Pareto-Group-BR/content_spark_V1/tree/main)
*   **Documentação Técnica do Fluxo**: [Fluxo N8N no Github](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/ORIGINAIS/Fluxo_N8N.md)
