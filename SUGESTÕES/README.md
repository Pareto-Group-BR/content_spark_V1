# Agente de Criação de Conteúdo (SUGESTÕES) - Manual de Operação

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
9.  [**Passo a Passo para Replicar o Fluxo SUGESTÕES**](#9-passo-a-passo-para-replicar-o-fluxo-SUGESTÕES)

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
*   **Acesso de Editor** à [Planilha de Controle](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit?gid=0#gid=0) e Conta Google para acessar a planilha e o Google Drive.

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

Confira mais detalhes sobre o Fluxo do N8N, com todas as suas etapas, nós, APIs utilizadas e Credenciais [nesta outra página da documentação.](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Fluxo_N8N.md)

<br>

### **6. Agentes de IA Utilizados**
Este fluxo utiliza múltiplos agentes da plataforma Tess AI, encadeados para realizar tarefas específicas:

*   **Agente de Pesquisa Aprofundada (ID: 32754):** Responsável por pesquisar e coletar informações detalhadas sobre o tema proposto.
*   **Criador de Roteiro de Post Instagram (ID: 32061):** Transforma o tema e a pesquisa em um roteiro completo para o carrossel.
*   **Agente Criador Carrosséis Instagram [FUNDO] (ID: 32060):** Gera as imagens de fundo para o carrossel com base no roteiro.
*   **Criar anúncios de Imagem em HTML (ID: 32059):** Monta os slides do carrossel combinando textos e imagens em formato HTML.

Confira mais dedatlhes sobre os agentes de IA, prompts e LLMs usados na [seção específica aqui na documentação.](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Agentes_de_IA.md)

<br>


## 7. Exemplos de Saída da Automação

## Exemplo de Carrossel Gerado

Abaixo, um exemplo da saída completa da automação, desde a escolha do tema até a legenda final para o Instagram.

| Campo | Descrição |
| :--- | :--- |
| **Tema *(input do usuário)*** | `Diferença entre IA tradicional e IA Generativa` |
| **Motivo da Escolha *(input do usuário)*** | Ajudar profissionais de qualquer área a saber como trabalhar melhor com a grande revolução tecnológica do momento, a IA GENERATIVA. |
| **Explicacao do Tema *(input do usuário)*** | Essa é uma excelente pergunta e fundamental para entender o atual momento tecnológico. Como a Pareto atua fortemente na integração dessas duas frentes, posso explicar não apenas a teoria, mas como elas coexistem no mercado.<br><br>A principal distinção é que a **IA Tradicional (ou Analítica/Discriminativa)** foca em **analisar** dados existentes para encontrar padrões e fazer previsões, enquanto a **IA Generativa** foca em **criar** novos dados e conteúdos a partir do que aprendeu.<br><br>Abaixo, apresento a tabela comparativa detalhada:<br><br>### Tabela Comparativa: IA Tradicional vs. IA Generativa<br><br>\| Aspecto \| IA Tradicional (Analítica/Discriminativa) \| IA Generativa (GenAI) \|<br>\| :--- \| :--- \| :--- \|<br>\| **Objetivo Principal** \| Analisar, classificar, otimizar e prever com base em dados históricos. \| Criar, sintetizar e gerar novos conteúdos originais (texto, imagem, código, som). \|<br>\| **Funcionamento** \| Distingue classes de dados (ex: "Isso é um gato ou um cachorro?"). Segue regras e padrões lógicos definidos. \| Aprende a distribuição e padrões dos dados para gerar algo novo (ex: "Crie uma imagem de um gato"). \|<br>\| **Dependência de Dados** \| Geralmente requer dados estruturados e rotulados para treinamento supervisionado. \| Treinada em volumes massivos de dados não estruturados (internet, livros, bancos de imagens). \|<br>\| **Resultado (Saída)** \| Um número (previsão), uma classe (categoria), uma recomendação ou um alerta. \| Um texto, uma imagem, um vídeo, uma linha de código ou áudio. \|<br>\| **Flexibilidade** \| Específica para a tarefa (Narrow AI). Um modelo de xadrez não sabe jogar damas. \| Mais generalista e adaptável. O mesmo modelo (ex: GPT-4) pode traduzir, resumir ou programar. \|<br>\| **Principais Limitações** \| Depende da qualidade/limpeza dos dados; não inventa nada novo; rigidez no escopo. \| Alucinações (inventar fatos), viés criativo, alto custo computacional, direitos autorais. \|<br>\| **Exemplos Práticos** \| • Filtros de Spam em e-mails.<br>• Recomendação da Netflix/Amazon.<br>• Detecção de fraude bancária.<br>• Previsão do tempo. \| • ChatGPT / Claude (Texto).<br>• Tess AI / Midjourney (Imagem).<br>• Copilot (Geração de Código).<br>• Suno AI (Criação de música). \|<br><br>---<br><br>### Aprofundando as Diferenças<br><br>Para fixar o conceito, vamos explorar as características e limitações com um exemplo do dia a dia da **Pareto e da Tess AI**:<br><br>#### 1. IA Tradicional (O "Cérebro Lógico")<br>É a tecnologia que sustenta a maior parte da automação industrial e análise de negócios hoje.<br>* **Exemplo na Pareto:** Quando nosso time de *Cientistas de Dados* cria um workflow para analisar planilhas financeiras de um cliente, identificar padrões de gastos e prever o orçamento do mês seguinte, estamos usando IA Tradicional.<br>* **Limitação:** Se pedirmos a essa IA para "escrever um e-mail explicando o gasto para o diretor", ela falhará, pois ela só entende números e classificações, não a semântica da linguagem humana.<br><br>#### 2. IA Generativa (O "Cérebro Criativo")<br>É a revolução recente baseada em LLMs (Large Language Models) e modelos de difusão.<br>* **Exemplo na Pareto:** Quando um *Agent Operator* utiliza a **Tess AI** para ler o relatório financeiro gerado pela IA Tradicional e redigir um comunicado elegante para os acionistas, ou criar uma imagem para uma campanha de marketing baseada no perfil do público, estamos usando IA Generativa.<br>* **Limitação:** Ela pode ser muito convincente, mas imprecisa. Se perguntarmos a uma IA Generativa "qual será o preço exato da ação da empresa amanhã", ela pode "alucinar" (inventar) um número, pois ela é probabilística na linguagem, não determinística na matemática como a tradicional.<br><br>### Conclusão: O Poder da Integração<br><br>No mercado atual, o "pulo do gato" não é escolher uma ou outra, mas **usá-las em conjunto**.<br><br>Na nossa metodologia de *Squads* na Pareto, frequentemente combinamos as duas:<br>1. Usamos **IA Tradicional (RPA e Análise de Dados)** para coletar informações, limpar dados e garantir a precisão dos fatos.<br>2. Enviamos esses dados para a **IA Generativa (via Tess AI)** para que ela transforme esses dados frios em relatórios, atendimentos humanizados ou conteúdo estratégico.<br><br>Isso garante a precisão da máquina tradicional com a versatilidade e criatividade da máquina generativa. |
| **Artes geradas** | [Link para a pasta com as imagens](https://drive.google.com/drive/folders/1-6YN6FGYvfckCf28Xne7lPjIvFQ8GTbO) |
| **Legenda gerada** | "Nem toda inteligência artificial funciona do mesmo jeito. 📡 Algumas seguem regras. Outras criam o inesperado. Saber a diferença pode mudar sua forma de pensar tecnologia. Você sabia disso? Qual das duas você usaria agora? 💡 #inteligenciaartificial #tecnologia #inovacao #reflexão" |
| **Data de Criação** | 17-12-2025 |


### Artes Geradas no Exemplo

<img width="360" height="360" alt="52335fb7-b847-4839-ba32-74b9e8566a5b" src="https://github.com/user-attachments/assets/08dd6b2a-1408-4b1b-b99c-72b226a811a9" />
<img width="360" height="360" alt="b0277926-c838-41b3-97bf-0b2c33ed871b" src="https://github.com/user-attachments/assets/2e610e4d-0c6a-47e8-a056-a64fb4a6f489" />
<img width="360" height="360" alt="3abb3aa4-f2fe-41c2-a432-9763b7f2e507" src="https://github.com/user-attachments/assets/20731601-d12c-4344-ba50-45dfa958e311" />
<img width="360" height="360" alt="572bf9d1-16a7-4b31-88ca-c16b8c2255a1" src="https://github.com/user-attachments/assets/e8d4dbb8-21f2-47eb-ba37-4fe8f9ae1591" />
<img width="360" height="360" alt="380e4525-70e6-4adf-bb6d-25b315459e51" src="https://github.com/user-attachments/assets/3745e2fa-d61c-4661-84b0-24a5fbcb8351" />
<img width="360" height="360" alt="7085469e-5482-49dd-80c1-e087623926e3" src="https://github.com/user-attachments/assets/71072adb-60ec-4a6e-bf08-7a00d1204d7c" />
<img width="360" height="360" alt="317479e3-aa36-4d4f-b598-cf61043648d9" src="https://github.com/user-attachments/assets/c0b7d50c-e2f8-44e2-8e83-236cf139b9ad" />
<img width="360" height="360" alt="a9ce7276-833a-4949-975e-c80f85faa53a" src="https://github.com/user-attachments/assets/738ed51c-a036-4d45-9ca6-c1a8d89b7483" />
<img width="360" height="360" alt="a722dd3f-eec0-4ee8-a7b0-7082adcfc676" src="https://github.com/user-attachments/assets/0db48c44-50aa-4ee3-ac4f-2f4ed946bb86" />


### **8. Links e Recursos**
*   **Planilha de Controle (template):** [`[Pareto AI Content Hub] Registro dos Conteúdos Criados - Pareto`](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit?gid=0#gid=0)
*   **Repositório das Artes:** Google Drive (Crie uma pasta "mãe" para armazenamento das artes criadas e substitua o OD no fluxo N8N)
*   **Arquivo JSON com o Fluxo N8N:** [Arquivo JSON com o fluxo SUGESTÕES](https://tess-workflows-files.storage.googleapis.com/b9b4cd1fcfd61ee840c2a00b6d3d467a9edf6ed9/n8n_workflow_sanitized.json)



## 9. Passo a Passo para Replicar o Fluxo SUGESTÕES

Para replicar este fluxo em seu próprio ambiente, siga as etapas abaixo.

> Antes de replicar o fluxo, leia atentamente as seções de [Documentação Técnica - Fluxo N8N: Agente de Criação de Conteúdo (SUGESTÕES)](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Fluxo_N8N.md) e [Agentes de IA - Fluxo SUGESTÕES](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Agentes_de_IA.md).

### **Etapa 1: Preparar Ativos (Credenciais, Agentes e Pasta)**

Antes de importar o fluxo, você precisa preparar todos os recursos externos.

1.  **Credenciais no N8N:** Acesse sua instância do N8N e, na seção **Credentials**, crie as credenciais essenciais para este fluxo (Google Sheets, Google Drive, Tess AI, Htmlcsstoimg API, etc.).
2.  **Agentes na Tess AI:** Os IDs dos agentes são únicos por workspace. Você precisa recriá-los:
    *   Consulte a seção **[Agentes de IA - Fluxo SUGESTÕES](https://github.com/Pareto-Group-BR/content_spark_V1/blob/main/SUGEST%C3%95ES/Agentes_de_IA.md)** deste repositório para ver a lista de agentes e seus prompts detalhados.
    *   Em seu próprio workspace da Tess AI, **crie ou duplique cada agente**, utilizando os mesmos prompts e configurações do fluxo original.
    *   Anote os **novos IDs** de cada um dos seus agentes.
3.  **Pasta no Drive:** Crie uma pasta principal no seu Google Drive onde as artes serão salvas e copie o **ID da pasta** (a parte final do URL).

### **Etapa 2: Replicar a Planilha de Controle**

1.  **Faça uma cópia** do template da planilha: [**Template - Planilha de Controle**](https://docs.google.com/spreadsheets/d/18jAJI2m42CHGPKLJkozDQVHs3cH1msQZuvJHef3G3NY/edit).
2.  Em sua nova planilha, acesse **`Extensões > Apps Script`** e conceda as permissões de execução do script.

### **Etapa 3: Importar e Configurar o Fluxo no N8N**

1.  **Importe o arquivo JSON** deste fluxo (`SUGESTÕES`) para a sua instância do N8N. [Link para Download](https://tess-workflows-files.storage.googleapis.com/b9b4cd1fcfd61ee840c2a00b6d3d467a9edf6ed9/n8n_workflow_sanitized.json) e **substitua todas as variáveis (credenciais, IDs de planilhas, agentes e similares)**.
2.  **Copie o URL do seu novo Webhook** abra cada um dos nós de `Webhook` do fluxo "[PARETO] Gerenciamento do fluxo de criação de conteúdo" e copie a URL de  "Production" específico deles [Link para Download do arquivo JSON](https://cdn.tess.im/assets/uploads/a3812340-f54f-4953-8a3e-ff1d4c998d3b.json).
3.  **Cole o Webhook na sua Planilha** no Apps Script, na variável `WEBHOOK_URL_SUGESTOES`, e salve.
4.  **Atualize os IDs no N8N:**
    *   **Pasta do Drive:** No nó `Create folder1`, cole o **ID da sua pasta** no campo "Parent Folder ID".
    *   **Agentes de IA:** Nos nós que fazem chamadas para a Tess AI (ex: `TESS - Agente de Pesquisa Aprofundada`), **substitua os IDs dos agentes antigos pelos novos IDs** que você criou.
5.  **Verifique os Nós Manualmente:** Percorra os demais nós para confirmar se suas credenciais foram associadas corretamente.

> IMPORTANTE: É necessário substituir as variáveis presentes no fluxo do N8N pelas suas específicas. Exemplos de variáveis: {{GOOGLE_SHEET_ID}} e {{TESS_API_TOKEN }}.
