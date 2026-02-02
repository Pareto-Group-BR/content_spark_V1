# Content Spark: Automação para Criação de Conteúdo no Instagram

# Índice

- [1. Visão Geral e Objetivo](#1-visão-geral-e-objetivo)
- [2. A Estratégia: Três Ramos de Criação de Conteúdo](#2-a-estratégia-três-ramos-de-criação-de-conteúdo)
  - [2.1. Content Spark - ORIGINAIS](#21-content-spark---originais)
  - [2.2. Content Spark - BRASILIDADES](#22-content-spark---brasilidades)
  - [2.3. Content Spark - SUGESTÕES](#23-content-spark---sugestões)
- [3. Arquitetura e Ferramentas Comuns](#3-arquitetura-e-ferramentas-comuns)
- [4. Manual de Operação Geral](#4-manual-de-operação-geral)
  - [Modos de Execução](#modos-de-execução)
  - [Gerenciamento](#gerenciamento)
- [5. Passo a Passo para Replicar](#5-passo-a-passo-para-replicar)
  - [Etapa 1: Preparar as Credenciais no N8N](#etapa-1-preparar-as-credenciais-no-n8n)
  - [Etapa 2: Replicar a Planilha de Controle](#etapa-2-replicar-a-planilha-de-controle)
  - [Etapa 3: Importar o Fluxo e Conectar as Ferramentas](#etapa-3-importar-o-fluxo-e-conectar-as-ferramentas)


## 1. Visão Geral e Objetivo

O **Content Spark** é uma suíte de automação projetada para otimizar e escalar a criação de conteúdo para o Instagram. A solução transforma temas, tendências e referências em posts de alto engajamento (carrosséis ou imagens únicas) de forma ágil e inteligente, minimizando a intervenção humana e o tempo de produção.

O objetivo principal é fornecer um fluxo constante de conteúdo relevante e de alta qualidade, pronto para publicação, permitindo que a equipe de marketing e conteúdo se concentre em estratégia, aprovação e análise de resultados.

A suíte é orquestrada no N8N e utiliza o Google Sheets como interface central de controle, tornando a operação acessível a todos os usuários, sem a necessidade de conhecimento técnico na plataforma de automação.

## 2. A Estratégia: Três Ramos de Criação de Conteúdo

O Content Spark opera através de três ramos complementares, cada um com um objetivo estratégico distinto para diversificar a produção de conteúdo:

| Ramo | Nome do Fluxo | Objetivo Principal | Fonte da Ideia |
| :--- | :--- | :--- | :--- |
| **ORIGINAIS** | `Content Spark - Originais` | Criar conteúdo 100% original com base em tendências e temas de alta performance no momento. | Análise de dados (Google Trends, Twitter, etc.) |
| **BRASILIDADES**| `Content Spark - Brasilidades` | Adaptar posts internacionais de sucesso para o público brasileiro, traduzindo e contextualizando o conteúdo. | Posts de alto engajamento de perfis de referência internacionais. |
| **SUGESTÕES** | `Content Spark - Sugestões`| Produzir conteúdo sob demanda a partir de ideias e temas específicos fornecidos pela equipe. | Input manual do usuário em uma planilha. |

---

### **2.1. Content Spark - ORIGINAIS**

Este fluxo é o motor de criação proativa da suíte. Ele foi projetado para gerar carrosséis com foco motivacional, identificando os temas com maior potencial de engajamento antes que saturem.

**Como funciona:**
1.  **Análise de Tendências:** A automação monitora fontes como Google Trends, Twitter e APIs de tendências para identificar temas em alta.
2.  **Seleção Inteligente:** Uma IA analisa os dados coletados, filtra ruídos e seleciona o tema com o maior potencial viral.
3.  **Pesquisa e Roteirização:** Agentes de IA aprofundam a pesquisa no tema escolhido e estruturam um roteiro completo para o carrossel (de 6 a 10 slides), incluindo textos e a legenda final.
4.  **Geração Visual:** As descrições de texto são traduzidas em composições visuais, gerando as imagens do carrossel.
5.  **Entrega:** As artes são salvas no Google Drive e o link, juntamente com a legenda, é registrado na planilha de controle.

Confira mais detalhes do fluxo ORIGINAIS [nesta documentação específica](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/ORIGINAIS/README.md).

### **2.2. Content Spark - BRASILIDADES**

Este fluxo foca em capitalizar sobre o sucesso comprovado de conteúdos internacionais, adaptando-os de forma rápida e eficiente para o mercado brasileiro.

**Como funciona:**
1.  **Coleta de Referências:** A automação extrai os posts mais recentes de uma lista de perfis internacionais de referência.
2.  **Seleção por Engajamento:** O sistema seleciona o post (imagem única ou carrossel) com a maior taxa de engajamento proporcional.
3.  **Tradução e Adaptação:** A legenda e o texto na imagem são processados por uma IA que traduz, descreve e reformata o conteúdo para o português, mantendo a mensagem central. O crédito ao post original é sempre incluído.
4.  **Geração e Entrega:** As novas artes e a legenda traduzida são geradas, salvas no Google Drive e registradas na planilha de controle.

Confira mais detalhes do fluxo BRASILIDADES [nesta documentação específica](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/BRASILIDADES/README.md).

### **2.3. Content Spark - SUGESTÕES**

Este é o fluxo sob demanda, permitindo que a equipe transforme rapidamente uma ideia específica em um post completo e bem estruturado.

**Como funciona:**
1.  **Input Manual:** O usuário preenche uma linha em uma aba específica da planilha, fornecendo o `Tema`, o `Motivo da Escolha` e uma `Explicação`.
2.  **Enriquecimento e Pesquisa:** A automa'ção utiliza o input inicial como base para uma pesquisa aprofundada, enriquecendo o conteúdo com dados e contexto.
3.  **Criação de Roteiro e Imagens:** Assim como no fluxo de "Originais", agentes de IA criam o roteiro detalhado, geram as imagens de fundo e montam o carrossel.
4.  **Entrega Final:** As artes são salvas em uma nova pasta no Google Drive, e o link, junto com a legenda, é inserido na mesma linha da planilha que originou a sugestão.

Confira mais detalhes do fluxo SUGESTÕES [nesta documentação específica](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/README.md).


## 3. Arquitetura e Ferramentas Comuns

Todos os fluxos do Content Spark compartilham uma arquitetura robusta e integrada, garantindo consistência e eficiência:

*   **Orquestrador**: **N8N** como motor central que conecta todas as etapas.
*   **Centro de Controle**: **Google Sheets** como a interface principal para interação do usuário, controle de agendamentos e visualização de resultados.
*   **Armazenamento de Mídia**: **Google Drive** para armazenar todas as artes geradas de forma organizada.
*   **Inteligência Artificial**: **Tess AI** e outros modelos de linguagem para análise, pesquisa, roteirização, tradução e criação de conteúdo.
*   **Coleta de Dados e Geração de Imagem**: Ferramentas como **Apify** (scraping) e **Htmlcsstoimg API** (conversão de HTML para imagem) são utilizadas em diferentes fluxos.
*   **Notificações**: **E-mail** e **Google Chat** para notificar a equipe sobre a conclusão dos processos.

## 4. Manual de Operação Geral

Cada fluxo possui a sua documentaçãoe specífica no GitHub:
[ORIGINAIS](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/ORIGINAIS/README.md).
[BRASILIDADES](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/BRASILIDADES/README.md).
[SUGESTÕES](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/README.md).

Toda a interação do usuário (utilizador da automação) com a suíte Content Spark é centralizada em uma Planilha de Google Sheets, a qual possui o seguinte **[modelo para duplicação Planilha de Controle](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit?gid=0#gid=0)**.


### Modos de Execução:
*   **Agendada:** Os fluxos `ORIGINAIS` e `BRASILIDADES` podem ser configurados para rodar automaticamente em dias e horários específicos, garantindo um suprimento constante de conteúdo.
*   **Manual (Sob Demanda):**
    *   Para os fluxos `ORIGINAIS` e `BRASILIDADES`, o usuário pode acionar uma execução imediata através do menu `Pareto AI` na planilha.
    *   Para o fluxo `SUGESTÕES`, a execução é iniciada ao preencher as informações do tema na aba correspondente e acionar o fluxo pelo mesmo menu.

### Gerenciamento:
Através do menu **"Pareto AI"** na planilha, o usuário pode:
*   **Executar** fluxos manualmente.
*   **Pausar** e **Reativar** as execuções agendadas.
*   **Verificar o Status** (ativo ou inativo) de cada fluxo.
*   **Selecionar os Dias de Execução** para os fluxos agendados.

## 5. Passo a Passo para Replicar

Para replicar a suíte Content Spark em seu próprio ambiente, siga as etapas abaixo. Este guia assume que você possui uma instância do N8N e uma Conta Google.

### **Etapa 1: Preparar as Credenciais no N8N**

Antes de importar os fluxos, é crucial configurar suas credenciais. Isso garantirá que o N8N mapeie as contas corretamente durante a importação.

1.  Consulte a documentação específica de cada fluxo (`ORIGINAIS`, `BRASILIDADES`, `SUGESTÕES`), especialmente o documento **"Fluxo_N8N.md"** de cada um, para identificar as credenciais necessárias (ex: Google Sheets, Google Drive, Apify, Tess AI, etc.).
2.  Em sua instância do N8N, acesse a seção de **Credentials** e adicione uma a uma, utilizando suas próprias chaves de API, tokens e informações de conta.

> **Dica:** É altamente recomendável criar as credenciais **antes** de importar o JSON. Assim, o N8N preencherá os campos automaticamente.

### **Etapa 2: Replicar a Planilha de Controle**

Você precisará de sua própria versão da planilha que serve como centro de comando.

1.  **Faça uma cópia** da planilha de template a partir deste link:
    *   [**Template - Planilha de Controle Content Spark**](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit)
2.  Abra sua nova planilha e acesse o menu **`Extensões > Apps Script`**.
3.  Na tela do Apps Script, clique em `Execuções` e depois no botão para **conceder as permissões necessárias**. Esta ação é obrigatória e precisa ser feita apenas uma vez.

### **Etapa 3: Importar o Fluxo e Conectar as Ferramentas**

Com as credenciais e a planilha prontas, o passo final é conectar tudo.

1.  **Importe o arquivo JSON** de um dos fluxos para a sua instância do N8N.
2.  **Copie o URL do seu novo Webhook:**
    *   Dentro do fluxo recém-importado no N8N, clique no primeiro nó, chamado **`Webhook`**.
    *   No painel à direita, na seção "Webhook URLs", copie o URL da aba **"Production"**.
3.  **Cole o Webhook na sua Planilha:**
    *   Volte para o **Apps Script** da sua planilha (menu `Extensões > Apps Script`).
    *   No código, localize as **URL de cada Webhook** (tem uma para ATIVAR, uma para PAUSAR e outra para RODAR MANUALMENTE o fluxo)
   <img width="976" height="252" alt="image" src="https://github.com/user-attachments/assets/299d431a-4ba0-43bd-b4f2-771e1475f88a" />
   <img width="977" height="241" alt="image" src="https://github.com/user-attachments/assets/4a492da7-4af6-4e16-bc9a-ec9f2186ecf4" />
   <img width="938" height="333" alt="image" src="https://github.com/user-attachments/assets/749844a9-ddbf-46b2-b05a-94cbe1a2a09e" />


    *   **Substitua o link antigo** pelo novo URL que você copiou do seu N8N.
    *   Clique no ícone de "Salvar projeto" (disquete).
4.  **Verifique os Nós Manualmente:**
    *   Percorra os nós do fluxo de trabalho no N8N para confirmar se suas credenciais foram associadas corretamente.
    *   Fique atento a nós de **Requisição HTTP (HTTP Request)**, que podem exigir a inclusão manual de um Token ou API Key diretamente no nó, caso a associação automática falhe.

Repita a **Etapa 3** para cada um dos fluxos que desejar replicar. Após isso, sua suíte Content Spark estará pronta para ser executada em seu ambiente.

> Para cada um dos fluxos de Criação de Conteúdo (BRASILIDADES, ORIGINAIS e SUGESTÕES), recomenda-se configurara um  "Error Workflow" no N8N, de modo a ser alertado em casos de erro nos fluxos. Pode ser um workflow que ative uma mensagem em canais de mensageria (como Google Chat), envie um E-mail, ou outra ação desejada. Segue um exemplo:
>
> <img width="224.5" height="133.25" alt="image" src="https://github.com/user-attachments/assets/491c4f49-e4a8-40b6-8a77-d279f93bbf6d" />


