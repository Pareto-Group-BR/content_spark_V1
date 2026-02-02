# Agente de Criação de Conteúdo (BRASILIDADES) - Manual de Operação

Este repositório contém a documentação completa do Agente de Criação de Conteúdo - Brasilidades ("Content Spark - Brasilidades"), uma automação projetada para gerar posts para Instagram com alto potencial de engajamento, baseados em referências internacionais de sucesso.

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
5.  [**Fluxo de Trabalho no N8N (Execução Técnica)**](#5-fluxo-de-trabalho-no-N8N-execução-técnica)
6.  [**Agentes de IA Utilizados**](#6-agentes-de-ia-utilizados)
7.  [**Exemplos de Saída da Automação**](#7-exemplos-de-saída-da-automação)
8.  [**Links e Recursos**](#8-links-e-recursos)

---

## 1. Visão Geral e Objetivo

O **Agente Criador de Conteúdo** foi desenvolvido para automatizar a criação de posts virais e inspiracionais (Carrossel ou Imagem Única) para Instagram, com foco em conteúdo motivacional. O objetivo é gerar conteúdo relevante e de alta qualidade de forma ágil, baseando-se em Perfis de Referência no Instagram. Ele vai selecionar posts de alto engajamento de perfis escolhidos (que podem ser alterados) para traduzir e adaptar para o português..

O agente entrega conteúdo pronto para publicação, incluindo imagens e legendas traduzidas e adaptadas para o português, com potencial de viralização e engajamento. A seleção do post de referência se baseia na taxa de engajamento (engajamentos/seguidores) dos posts dos últimos 3 dias para cada perfil de referência listado. Depois de selecionado o post, segue para a adaptação para o português e registro na planilha de conteúdos criados;

*OBS: O fluxo NÃO inclui a publicação automática no Instagram, esse processo exige a validação manual do gestor do Instagram.*

## 2. Estratégia Principal

*   **Criação Automatizada:** Gerar conteúdo para Instagram baseado em referências de perfis motivacionais (os quais podem ser alterados pelo usuário que está utilizando a automação, incluindo ou removendo perfis da lista através da planilha de controle).
*   **Foco Inspiracional:** Manter a linha editorial de conteúdo motivacional.
*   **Tradução e Adaptação:** Traduzir e adaptar completamente o post de referência (legenda e texto na imagem) para o público brasileiro. O post de referência receberá os devidos créditos na legenda do post criado, mencionando o @ do perfil.
*   **Fonte de Conteúdo Viral:** Criar um canal de publicações com alto potencial de engajamento.
*   **Diversificação dos Posts:** A automação está treinada a não repetir perfis de referência em sequência, mesmo que o post com maior engajamento seja de um perfil repetido. Assim, espera-se que tenha mais diversidade nas criações.

## 3. Manual de Operação (Para Usuários)

Esta seção é destinada à equipe que irá interagir com a automação no dia a dia (ex: Marketing, Conteúdo).

### Pré-requisitos

Para utilizar a ferramenta, é indispensável ter acesso de **Editor** à seguinte planilha no Google Sheets:
*   **Planilha de Controle:** [Agente de Criação de Conteúdo](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit)

Também é necessário liberar as permissões do AppScript, o que pode ser feito via menu da planilha **"Permissões do Script"**. Essa liberação é feita uma única vez, ao utilizar a planilha pela primeira vez.

<img width="319" height="83" alt="image" src="https://github.com/user-attachments/assets/9b342bb8-9a77-48d3-8e21-97906da01e8b" />


> **Importante:** Não é necessário ter acesso direto ao ambiente do n8n. Toda a interação é feita através desta planilha.

### A Planilha de Controle: O Centro de Comando

A planilha é a interface principal para configurar, operar e visualizar os resultados da ferramenta.

*   **Abas de Resultados:** Nela você encontrará os conteúdos gerados, separados nas abas: `ORIGINAL`, `BRASILIDADES` e `SUGESTÃO`. Para o fluxo atual, concentre-se na aba BRASILIDADES.
*   **Configuração de Perfis:** A lista de perfis do Instagram a serem monitorados fica na aba `perfis de referência`.
*   **Controles Manuais:** A planilha contém botões para controlar as automações, localizados no menu superior **"Pareto AI"**.

> **Importante:** Não altere ou inclua as colunas da planilha pois isso vai prejudicar a execução do fluxo.

### Modos de Operação

#### Execução Automatizada (Agendada)
A ferramenta possui um fluxo que roda automaticamente para garantir um fornecimento constante de conteúdo novo, baseado no melhor post dos perfis de referência.
*   **Frequência:** O fluxo roda à meia-noite nos dias que forem selecionados pelo usuário na planilha. É possível selecionar quantos quiser via menu da planilha: **Segunda, Terça,Quarta, Quinta, Sábado e Domingo**.

#### Execução Manual via Menu sob Demanda
Para buscar posts de referência e gerar um conteúdo imediatamente, sem repetir o perfil anterior.
1.  **Acesse a planilha** Qualquer página, mas o registro da criação ficará na aba BRASILIDADES.
2.  **Selecione o menu:** Na parte superior, selecione `Pareto AI` -> `Brasilidades`.
3.  **Execute:** Escolha a opção `Executar fluxo`.
4.  **Aguarde:** Uma mensagem de aviso informará que o fluxo pode levar até 3 minutos. Você poderá fechar o pop up (inclusive, a planilha pode ser fechada sem nenhum problema);
5.  **Verifique o Resultado:** Após rodar a automação, o conteúdo gerado aparecerá na última linha preenchida da aba `BRASILIDADES`. Também serão enviados 2 avisos (e-mail e Google Chat) após a finalização da criação.

### Gerenciando as Automações Agendadas
Você pode pausar e ativar as execuções agendadas dos fluxos diretamente pela planilha.

*   **Para Pausar:** No menu `Pareto AI`, selecione o fluxo desejado (no caso, `Brasilidades`) e clique na opção de **`Pausar Fluxo`**. Isso desliga o fluxo agendado no n8n.
*   **Para Reativar:** No menu `Pareto AI`, selecione o fluxo desejado (no caso, `Brasilidades`) e clique na opção de **`Ativar Fluxo`**. Isso reativa o fluxo agendado no n8n.
<img width="729" height="241" alt="image" src="https://github.com/user-attachments/assets/de735126-ae89-42ee-a899-ec7a2ab73c8c" />



Você também pode consultar o status de cada fluxo, a fim de verificar se estão ativos ou pausados. Além de alterar os dias programados para sua execução agendada.
*   **Para Consultar Status:** No menu `Pareto AI`, clique na opção de **`Verificar Status dos Fluxos`**. Uma tela lateral vai mostrar o status (ativo ou pausado) de cada um dos 3 fluxos existentes.
<img width="306" height="287" alt="image" src="https://github.com/user-attachments/assets/8d0d9d6e-81e5-486b-8ff3-6df4af4a58c9" />

 
*   **Para Alterar Dias Agendados:** No menu `Pareto AI`, selecione o fluxo desejado (no caso, `Brasilidades`) e clique na opção de **`Selecionar Dias de Execução`** . Na tela que irá aparecer, basta selecionar os dias desejados para a execução agendada.
<img width="495" height="473" alt="image" src="https://github.com/user-attachments/assets/31699a2c-49df-4499-adfd-abc8a302743f" />


### Solução de Problemas (Troubleshooting)
1.  **A automação manual falhou?** Verifique se todas as colunas de tema na planilha foram preenchidas corretamente.
2.  **O conteúdo não foi gerado no horário?** Verifique no menu se a execução automática não está pausada.
3.  **Problema persiste ou outras intercorrências?** Contate o responsável pela manutenção da ferramenta. Para reportar um bug, por favor, abra uma **Issue** detalhando o problema.

*OBS: Como a automação conta com a integração de diferentes plataformas e agentes de IA, pode haver alguma instabilidade temporária em algum deles.*

## 4. Arquitetura e Ferramentas

O agente é orquestrado pela plataforma **n8n** e se integra com diversas APIs para realizar seu trabalho:

*   **n8n:** Orquestrador central do fluxo de trabalho.
*   **Apify:** Para extração de dados e posts de perfis de referência no Instagram.
*   **Tess AI API:** Para tarefas de IA, como tradução, descrição de imagens e geração de código HTML.
*   **Google Sheets API:** Para registrar os conteúdos gerados e controlar o fluxo.
*   **Google Drive API:** Para armazenar os criativos (imagens) gerados.
*   **HtmlCssToImage API:** Para converter o código HTML final em uma imagem (PNG).

## 5. Fluxo de Trabalho no N8N (Execução Técnica)

Esta seção descreve o passo a passo da automação no n8n, desde a coleta de dados até o registro final. Para mais detalhes sobre os nós presentes no fluxo, APIs utilizadas e credenciais necessárias, consulte a seção [Fluxo do N8N](https://github.com/liviatagliari/pareto_content_spark_brasilidades/blob/main/Fluxo_N8N.md).

1.  **Gatilhos (Schedule e Webhook):** O workflow pode ser iniciado de duas formas:
    *   **Agendamento (Cron):** O fluxo é ativado automaticamente à meia-noite nos dias da semana selecionados pelo usuário na planilha de controle.
    *   **Manual (Webhook):** O usuário pode acionar o fluxo sob demanda através do menu na planilha, que dispara um webhook para o n8n.

2.  **Busca e Coleta de Dados (Apify):** O processo inicia ativando um "scraper" na Apify para coletar os posts mais recentes dos perfis de referência listados na planilha.

3.  **Filtragem e Seleção:** Os dados coletados são processados:
    *   Posts fixados e de perfis já analisados recentemente são removidos.
    *   Vídeos são descartados.
    *   Um nó de código seleciona o post com o maior engajamento proporcional (curtidas + comentários).

4.  **Processamento e Tradução da Legenda (Agente 34674):** A legenda do post de maior engajamento é enviada para o **Agente 34674 (TESS - Descrever e Traduzir Texto)**. Este agente descreve, reformata e traduz o texto para o português, preparando-o para a publicação.

5.  **Ramificação (Imagem Única vs. Carrossel):** O fluxo verifica se o post é uma imagem única ou um carrossel. Se for um carrossel, um loop é iniciado para que cada imagem seja processada individualmente, sendo todas adicionadas ao mesmo ID de criação no final.

6.  **Análise e Geração da Nova Imagem (Processo Multiagente):**
    *   **Análise de Imagem e Tradução do Texto:** A imagem original é analisada por um agente de IA para identificar o texto principal contido nela. Esse texto é então traduzido para o português.
    *   **Transformação da Imagem em HTML (Agente 34683):** A imagem original é enviada ao **Agente 34683 (TESS - Gerar HTML da Imagem Original)**. Ele transforma a imagem em um código HTML, identificando o texto principal e a imagem de fundo, ao mesmo tempo que remove logomarcas e aplica sutis diferenciações.
    *   **Criação do Novo HTML com Texto Traduzido (Agente 34686):** O **Agente 34686 (TESS - Gerar HTML da Imagem Nova)** recebe o código HTML da etapa anterior e o texto já traduzido, combinando-os para gerar uma nova versão do HTML com o conteúdo em português.
    *   **Geração da Imagem Final (HtmlCssToImage):** O HTML final é enviado para a API **HtmlCssToImage**, que renderiza o código e gera a nova imagem do post no formato PNG.

7.  **Armazenamento e Registro:**
    *   **Google Drive:** A nova imagem (ou imagens, no caso de carrossel) é salva em uma pasta no Google Drive.
    *   **Google Sheets:** Todos os dados relevantes (tema, link da arte, perfil de referência, legenda traduzida, etc.) são registrados em uma nova linha na planilha "Conteúdos Baseados nas Referências para Validação".

8.  **Limpeza de Arquivos:** Ao final do processo, os arquivos temporários gerados e carregados na memória da Tess AI são deletados para manter o ambiente organizado e otimizado.



## 6. Agentes de IA Utilizados

A automação utiliza um conjunto de agentes de IA especializados, hospedados na plataforma Tess AI, para realizar as tarefas de processamento de linguagem e imagem. Cada agente possui uma configuração específica para garantir a qualidade e a consistência do resultado.

*   **Agente 34674 (TESS - Descrever e Traduzir Texto)**
    *   **Objetivo:** Utilizado para descrever, reformatar e traduzir a **legenda** do post original.
    *   **LLM:** Gemini 2.5 Flash
    *   **Temperatura:** Imaginativa
    *   **Ferramentas:** Nano Banana +

*   **Agente 34683 (TESS - Gerar HTML da Imagem Original)**
    *   **Objetivo:** Responsável por transformar a imagem original em uma primeira versão de código HTML, separando texto e fundo.
    *   **LLM:** Claude 4.5 Sonnet
    *   **Temperatura:** Criativa
    *   **Ferramentas:** Nano Banana +

*   **Agente 34686 (TESS - Gerar HTML da Imagem Nova)**
    *   **Objetivo:** Recebe o HTML da imagem original e o texto já traduzido para gerar uma nova versão do HTML com o texto em português.
    *   **LLM:** Claude 4.5 Haiku
    *   **Temperatura:** Sistemática
    *   **Inputs:** `html` (código HTML da imagem original), `traducao` (frase traduzida).

*   **Agente 34884 (TESS - Traduzir Legenda1)**
    *   **Objetivo:** Especializado em traduzir a legenda do post para o português (utilizado em um fluxo paralelo/de apoio).
    *   **LLM:** Gemini 2.5 Flash
    *   **Temperatura:** Natural
    *   **Inputs:** `legenda-do-post` (legenda em inglês), `perfil` (perfil de referência).

> Para uma documentação completa e atualizada dos prompts utilizados por cada agente, consulte o seguinte documento: [Descrição e Prompts dos Agentes de IA](https://github.com/liviatagliari/pareto_content_spark_brasilidades/blob/main/Agentes%20de%20IA.md)


## 7. Exemplos de Saída da Automação

Para ilustrar o resultado final do processo, veja abaixo dois exemplos reais de um post de referência que foi selecionado e transformado pela automação.

### Exemplo 1: Post Único

*   **Perfil de Referência Selecionado:** `@valorgi`
*   **Post Original:** [Link para o Instagram](https://www.instagram.com/p/DUAF5SpkccM/)
*   **Arte Gerada:** [Link para o Google Drive](https://drive.google.com/drive/folders/19SRFV70HyHepC5EZLinHJIS4SF_s3JzQ)

#### Comparativo: Original vs. Gerado

| Atributo | Post Original (Referência) | Post Gerado (Pela Automação) |
| :--- | :--- | :--- |
| **Texto na Imagem** | "You only win when your mind is stronger than your emotions." | "Você só pode vencer quando sua mente é mais forte do que suas emoções." |
| **Legenda** | "You only win when your mind stays stronger than your emotions.<br><br>Feelings can be loud, but discipline is quiet and consistent.<br><br>Master your reactions and your life gets easier.<br><br>Follow @valorgi for more mindset reminders you can live by.<br><br>#mindset #emotionalcontrol #selfdiscipline #mentalstrength #personalgrowth" | "A vitória é sua quando a mente se mantém mais forte que a emoção.<br><br>Os sentimentos podem ser barulhentos, mas a disciplina é silenciosa e constante.<br><br>Domine suas reações e sua vida fica mais leve.<br><br>#mindset #controleemocional #autodisciplina #forçamental #crescimentopessoal<br><br>Créditos: @valorgi" |


#### Imagens do Exemplo:

<img width="324" height="405" alt="0  - 27_01_2026" src="https://github.com/user-attachments/assets/55d22d3c-c675-43c4-b92d-53f22ae0024d" />

<img width="324" height="405" alt="0  - 27_01_2026" src="https://github.com/user-attachments/assets/397659f5-7c5a-4eaf-9436-e3b52717287f" />


### Exemplo 2: Carrossel


*   **Perfil de Referência Selecionado:** `@motivationmafia`
*   **Post Original:** [Link para o Instagram](https://www.instagram.com/p/DT6DaSBEqYU/)
*   **Arte Gerada:** [Link para o Google Drive](https://drive.google.com/file/d/1WUrj_VTmq0l6MFi-lbiqroxI9w4OJjfq/view?usp=drive_link)

#### Comparativo: Original vs. Gerado

| Atributo | Post Original (Referência) | Post Gerado (Pela Automação) |
| :--- | :--- | :--- |
| **Texto na Imagem** | "People who want to see you win, will help you win. Remember that." | "Pessoas que querem te ver vencer, vão te ajudar a vencer. Lembre-se disso." |
| **Legenda** | "The people that genuinely want the best for you will want that whether you are on their team or another.  Being a good friend isn’t conditional 💯" | "Quem quer o seu bem de verdade, vai querer isso sempre, não importa se você está no mesmo time que essa pessoa ou em outro. Amizade verdadeira não tem condição 💯 Créditos: @motivationmafia" |

#### Imagens do Exemplo:

<img width="324" height="405" alt="0  - 27_01_2026" src="https://github.com/user-attachments/assets/7194df2b-383b-4f93-881e-2570226af7fe" />

<img width="324" height="405" alt="0  - 27_01_2026 (1)" src="https://github.com/user-attachments/assets/835e535b-597a-4457-aff9-0e313d410e86" />



### Observações sobre os Exemplos:

*   **Tradução e Adaptação:** O texto da imagem foi traduzido de forma literal, mas adaptado ao idioma português (caso uma palavra não exista em português, o agente está orientado a adaptar para um sinônimo).
*   **Geração de Legenda:** A automação não apenas traduziu, mas também gerou uma legenda em português, adicionando contexto e hashtags relevantes para o público brasileiro.
*   **Manutenção dos Créditos:** O perfil de referência (`@valorgi`) foi mantido nos créditos, garantindo a atribuição da inspiração.
*   **Qualidade da Imagem:** A imagem de fundo foi preservada, mantendo a estética do post original, enquanto o texto foi substituído.



## 8. Links e Recursos

*   **Planilha de Controle:** [Agente de Criação de Conteúdo](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit)
*   **Pasta de Criativos Gerados:** [Google Drive]([https://drive.google.com/drive/folders/1AK1bYdQyZsdHxH3iN_BchRRmBaZ4bdB9](https://drive.google.com/drive/folders/1SuTClpwIvKs_vyyiftJY8bQpVmqiELYf))
*   **Workflow no n8n:** [Link do Workflow](https://app.engine.pareto.io/workflow/RFiyrqPeQ5BtZCsM)
*   **Canal no Google Chat:** [Link do Canal de avisos](https://chat.google.com/room/AAQAbJ7q6g4?cls=7)
*   **Testes Realizados:**
    *   [Teste 1](https://drive.google.com/drive/folders/18J9mKlbema34W0ra0z7I6KaEaZmpJ7AC)
    *   [Teste 2](https://drive.google.com/drive/folders/1NEtO5cV2g4OmbpnWomx4vVxtf31LWi4F)
    *   [Teste 3](https://drive.google.com/drive/folders/1qFRugz_imj_4OREKe8vk2tEFOWZA2y8n)
    *   [Teste 4](https://drive.google.com/drive/folders/11f5OxxEstGACml4aoPmt-0oZAfGVxhzF)
```
