# Content Spark: Suíte de Automação para Criação de Conteúdo no Instagram

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

Toda a interação com a suíte Content Spark é centralizada na **[Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit)**.

Cada fluxo possui a sua documentaçãoe specífica no GitHub:
[ORIGINAIS](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/ORIGINAIS/README.md).
[BRASILIDADES](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/BRASILIDADES/README.md).
[SUGESTÕES](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/README.md).


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
