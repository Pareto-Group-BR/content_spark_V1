# Agente de Criação de Conteúdo (Fluxo de Sugestões) - Manual de Operação

> Este repositório contém a documentação completa do Agente de Criação de Conteúdo - SUGESTÕES ("Content Spark - SUGESTÕES"), uma automação projetada para gerar Carrosséis com **foco motivacional** para Instagram com alto potencial de engajamento, baseados em tendências do momento. Com isso, as artes e a comunicação serão mais voltadas para esse objetivo.

> Cabe destacar que esse foco motivacional foi pré-definido devido ao objetivo original deste fluxo, com base nas boas práticas de mercaso. **Ao duplicar o fluxo, é possível  alterar esse foco Motivacional diretamente nos prompts dos agentes de IA, em caso de de desejar fazer uma mudança de objetivo.**

## Índice
1.  [**Visão Geral e Objetivo**](#1-visão-geral-e-objetivo)
2.  [**Estratégia Principal**](#2-estratégia-principal)
3.  [**Manual de Operação (Para Usuários)**](#3-manual-de-operação-para-usuários)
    *   [Pré-requisitos](#pré-requisitos)
    *   [A Planilha de Controle: O Centro de Comando](#a-planilha-de-controle-o-centro-de-comando)
    *   [Modos de Operação](#modos-de-operação)
    *   [Solução de Problemas (Troubleshooting)](#solução-de-problemas-troubleshooting)
4.  [**Arquitetura e Ferramentas**](#4-arquitetura-e-ferramentas)
5.  [**Fluxo de Trabalho no N8N (Execução Técnica)**](#5-fluxo-de-trabalho-no-n8n-execução-técnica)
6.  [**Agentes de IA Utilizados**](#6-agentes-de-ia-utilizados)
7.  [**Exemplos de Saída da Automação**](#7-exemplos-de-saída-da-automação)
8.  [**Links e Recursos**](#8-links-e-recursos)

<br>

### **1. Visão Geral e Objetivo**
O fluxo **Agente de Criação de Conteúdo (Sugestões)** é uma automação projetada para transformar temas e ideias, inseridos manualmente em uma planilha, em posts completos e prontos para o Instagram. A ferramenta foi criada para agilizar a produção de conteúdo, permitindo que o time de marketing forneça um contexto inicial e receba, de forma automatizada, um carrossel com artes e legenda.

O objetivo principal é gerar conteúdo relevante e de alta qualidade sob demanda, a partir de temas específicos fornecidos pelo usuário, garantindo agilidade e consistência na comunicação.

<br>

### **2. Estratégia Principal**
A estratégia do fluxo de **Sugestões** se baseia em um processo de enriquecimento de dados em etapas, utilizando uma sequência de agentes de IA para construir o post final:

1.  **Entrada Manual:** O processo é iniciado quando um usuário preenche uma nova linha na aba "Sugestão de Temas" da planilha de controle, fornecendo o tema, o motivo da escolha e uma breve explicação.
2.  **Pesquisa e Aprofundamento:** A automação coleta esses dados e os utiliza como base para uma pesquisa aprofundada, buscando informações adicionais para enriquecer o conteúdo.
3.  **Criação do Roteiro:** Com o tema e a pesquisa em mãos, um agente de IA especializado cria um roteiro detalhado para um post em formato de carrossel.
4.  **Geração de Imagens:** Outro agente de IA gera as imagens de fundo para cada slide do carrossel, com base nas especificações do roteiro.
5.  **Montagem do Carrossel:** As imagens e os textos são combinados em arquivos HTML, que são convertidos em imagens finais (PNG).
6.  **Entrega Final:** As imagens geradas são salvas em uma pasta no Google Drive, e os links, juntamente com a legenda, são atualizados na planilha original. Uma notificação é enviada via GChat e e-mail para informar a conclusão.

<br>

### **3. Manual de Operação (Para Usuários)**

#### **Pré-requisitos**
*   **Acesso de Editor** à [Planilha de Controle](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=0#gid=0) e Conta Google para acessar a planilha e o Google Drive.

Também é necessário liberar as permissões do AppScript, o que pode ser feito via menu da planilha **"Permissões do Script"**. Essa liberação é feita uma única vez, ao utilizar a planilha pela primeira vez.

<img width="319" height="83" alt="image" src="https://github.com/user-attachments/assets/9b342bb8-9a77-48d3-8e21-97906da01e8b" />
  
*   Fazer parte do canal do Google Chat para receber as notificações (Opcional para receber as notificações).


> **Importante:** Não é necessário ter acesso direto ao ambiente do n8n. Toda a interação é feita através desta planilha.
  

#### **A Planilha de Controle: O Centro de Comando**
A planilha é a interface principal para operar o fluxo e visualizar os resultados.

*   **Abas de Resultados:** Nela você encontrará os conteúdos gerados, separados nas abas: `SUGESTÕES`, `BRASILIDADES` e `SUGESTÃO`. Para o fluxo atual, concentre-se na aba SUGESTÕES. Lá, você irá inserir o tema desejado, o motivo da escolha, uma explicação e um link de referência (opcional). Quando o Carrossel for criado, as artes e texto serão adicionados nesta mesma linha da planilha, nas colunas à direita.
*   **Config**: Contém configurações técnicas da automação. **Não altere esta aba** sem orientação.
*   **Controles Manuais:** A planilha contém botões para controlar as automações, localizados no menu superior **"Pareto AI"**.

<img width="1346" height="638" alt="image" src="https://github.com/user-attachments/assets/945a746c-df90-455a-a42c-81375356c71f" />


#### **Modos de Operação**
A execução do fluxo de sugestões é acionada **manualmente** ao fornecer um tema específico na planilha (o usuário deve preencher a linha e depois clicar no botão do fluxo).

**Execução Manual por Tema:**

1.  **Acesse a Aba SUGESTÕES:** Vá para a aba **`SUGESTÕES`** na planilha de controle.
2.  **Preencha o Tema:** Em uma linha vazia onde a coluna "Post sugerido?" esteja marcada como `false`, preencha as colunas:
    *   `Tema`
    *   `Motivo da Escolha`
    *   `Explicacao do Tema`
    *   `Referência (opcional)`
3.  **Acione a Automação:** A automação é acionada por um webhook que detecta a nova linha preenchida.   Para isso, selecione o menu: selecione `Pareto AI` -> `ORIGINAIS` -> `Executar fluxo`.
   <img width="631" height="200" alt="image" src="https://github.com/user-attachments/assets/658c4548-056e-4884-8b54-fd6e2b4af812" />

5.   **Aguarde:** Uma mensagem de aviso informará que o fluxo foi iniciado (ele poderá levar de 20 a 25 minutos). Você poderá fechar o pop up (inclusive, a planilha pode ser fechada sem nenhum problema);
6.  **Verifique o Resultado:** Após a execução (que pode levar alguns minutos), o conteúdo gerado aparecerá nas colunas **`Artes`** (com o link para a pasta no Google Drive) e **`Legenda`**. A coluna "Post sugerido?" será atualizada para `true`.

#### **Solução de Problemas (Troubleshooting)**
*   **A automação não iniciou?** Verifique se as colunas `Tema`, `Motivo da Escolha` e `Explicacao do Tema` foram preenchidas corretamente na planilha. Certifique-se também de que a coluna "Post sugerido?" está como `false`.
*   **Problema persiste?** Corie uma Issue aqui na documentação do agente na Github.

<br>

### **4. Arquitetura e Ferramentas**
*   **Orquestração:** **N8N** é o motor que conecta todas as etapas do fluxo de trabalho.
*   **Interface de Usuário:** **Google Sheets** serve como a principal interface para entrada de dados e visualização dos resultados.
*   **Armazenamento de Arquivos:** **Google Drive** é utilizado para armazenar as imagens (artes) geradas para cada post.
*   **Notificações:** **Gmail** e **Google Chat** são usados para notificar os stakeholders sobre a conclusão do processo.
*   **Inteligência Artificial:** Uma série de agentes de IA da **Tess AI** é utilizada para pesquisa, roteirização, e geração de imagens.

<br>

### **5. Fluxo de Trabalho no N8N (Execução Técnica)**
O fluxo de trabalho no N8N é ativado por um **Webhook** e segue as seguintes etapas principais:

1.  **`Webhook`**: Aguarda um chamado (trigger) para iniciar o fluxo.
2.  **`Get row(s) in sheet`**: Lê a planilha "Sugestão de Temas" e filtra pelas linhas onde "Post sugerido?" é `false`.
3.  **`Filter1`**: Garante que a linha tenha pelo menos o campo "Explicacao do Tema" preenchido.
4.  **`Pré Pesquisa Aprofundada1`**: Prepara os dados para o primeiro agente de IA.
5.  **`TESS - Agente de Pesquisa Aprofundada`**: Executa os agentes de pesquisa para aprofundar no tema. Inclui nós para iniciar a execução, aguardar e obter o resultado (`TESS - Agente de Pesquisa Aprofundada - Aprofundar1`, `Wait 60s1`, `GET | TESS - Agente Identificador de Tendências2`).
6.  **`Pós Pesquisa Aprofundada`**: Formata a saída da pesquisa para a próxima etapa.
7.  **`TESS - Criador de Roteiro de Post Instagram`**: Envia o tema e a pesquisa para o agente de IA que cria o roteiro do carrossel.
8.  **`Pós criação de roteiro`**: Prepara o roteiro para o agente de geração de imagens de fundo.
9.  **`TESS - Agente Criador Carrosséis Instagram [FUNDO]`**: Gera as imagens de fundo para o carrossel.
10. **`JSON Parse3`**: Prepara o roteiro e as imagens de fundo para o agente que montará o HTML.
11. **`TESS - Criar anúncios de Imagem em HTML`**: Gera o código HTML para cada slide do carrossel.
12. **`Dados do conteúdo1`**: Extrai a legenda e outras informações do roteiro.
13. **`Create folder1`**: Cria uma pasta no Google Drive para armazenar as artes.
14. **`Dividir saída em arquivos HTML diferentes1`**: Separa a saída HTML em arquivos individuais para cada imagem.
15. **`Loop Over Items2`**: Inicia um loop para processar cada arquivo HTML.
16. **`Gerar imagem pelo HTML1`**: Converte cada HTML em uma imagem PNG.
17. **`Baixar imagem`** e **`Subir imagem no drive`**: Faz o download da imagem gerada e a envia para a pasta criada no Google Drive.
18. **`Atualizar planilha`**: Atualiza a linha original na planilha com o link da pasta do Google Drive e a legenda do post.
19. **`Send a message1`** e **`Enviar Mensagem Gchat`**: Envia notificações por e-mail e Google Chat informando sobre a conclusão.

Confira mais detalhes sobre o Fluxo do N8N [nesta outra página da documentação.](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Fluxo_N8N.md)

<br>

### **6. Agentes de IA Utilizados**
Este fluxo utiliza múltiplos agentes da plataforma Tess AI, encadeados para realizar tarefas específicas:

*   **Agente de Pesquisa Aprofundada (ID: 32754):** Responsável por pesquisar e coletar informações detalhadas sobre o tema proposto.
*   **Criador de Roteiro de Post Instagram (ID: 32061):** Transforma o tema e a pesquisa em um roteiro completo para o carrossel.
*   **Agente Criador Carrosséis Instagram [FUNDO] (ID: 32060):** Gera as imagens de fundo para o carrossel com base no roteiro.
*   **Criar anúncios de Imagem em HTML (ID: 32059):** Monta os slides do carrossel combinando textos e imagens em formato HTML.

Confira mais dedatlhes sobre os agents de IA na [seção específica aqui na documentação.](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Agentes_de_IA.md)

<br>


## 7. Exemplos de Saída da Automação

## Exemplo de Carrossel Gerado

Abaixo, um exemplo da saída completa da automação, desde a escolha do tema até a legenda final para o Instagram.

| Campo | Descrição |
| :--- | :--- |
| **Tema *(input do usuário)*** | `Diferença entre IA tradicional e IA Generativa` |
| **Motivo da Escolha *(input do usuário)*** | Ajudar profissionais de qualquer área a saber como trabalhar melhor com a grande revolução tecnológica do momento, a IA GENERATIVA.  |
| **Explicacao do Tema *(input do usuário)*** | Essa é uma excelente pergunta e fundamental para entender o atual momento tecnológico. Como a Pareto atua fortemente na integração dessas duas frentes, posso explicar não apenas a teoria, mas como elas coexistem no mercado.

A principal distinção é que a **IA Tradicional (ou Analítica/Discriminativa)** foca em **analisar** dados existentes para encontrar padrões e fazer previsões, enquanto a **IA Generativa** foca em **criar** novos dados e conteúdos a partir do que aprendeu.  |
| **Artes** | [Link para a pasta com as imagens](https://drive.google.com/drive/folders/1-6YN6FGYvfckCf28Xne7lPjIvFQ8GTbO) |
| **Legenda** | "Nem toda inteligência artificial funciona do mesmo jeito. 📡 Algumas seguem regras. Outras criam o inesperado. Saber a diferença pode mudar sua forma de pensar tecnologia. Você sabia disso? Qual das duas você usaria agora? 💡 #inteligenciaartificial #tecnologia #inovacao #reflexão" |
| **Data de Criação** | 17-12-2025 |

### Artes Geradas no Exemplo



### **8. Links e Recursos**
*   **Planilha de Controle:** [`[Pareto AI Content Hub] Registro dos Conteúdos Criados - Pareto`](https://docs.google.com/spreadsheets/d/1V3A3ClTlg4waudwwiP1lHlrqNv-I96fNmcYilR_5RUY/edit?gid=200832694#gid=200832694)
*   **Repositório das Artes:** Google Drive (As pastas são criadas dinamicamente e os links inseridos na planilha)
*   **Arquivo JSON com o Fluxo N8N:** [Arquivo JSON com o fluxo SUGESTÕES](https://cdn.tess.im/assets/uploads/a28b15f4-b907-4c13-959a-73a303b206c2.json?_gl=1*oiy62f*_gcl_au*MTg4Nzg4OTA0My4xNzY5Njk3NTg4LjIyNDgwMTQ1My4xNzY5ODA1MjY4LjE3Njk4MDg3NjY.*_ga*OTM0Mzg4NjAxLjE3Njk2OTc1ODg.*_ga_K1Q8FJY3BS*czE3Njk4MDA1NjAkbzYkZzEkdDE3Njk4MDg3NjYkajYwJGwwJGgw*_ga_9D17W435GL*czE3Njk4MDA1NjAkbzI2JGcxJHQxNzY5ODA4NzY2JGo2MCRsMCRoMA..)
