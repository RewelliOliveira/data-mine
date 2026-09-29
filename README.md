# Mineração de Dados — Ambiente Jupyter Notebook com Docker

Este repositório contém as atividades práticas, notebooks e conjuntos de dados desenvolvidos para a disciplina de **Mineração de Dados**. 

Para garantir reprodutibilidade e isolamento de dependências (Python, bibliotecas científicas e JupyterLab), o ambiente de desenvolvimento é executado via **Docker** utilizando a imagem oficial [`quay.io/jupyter/scipy-notebook`](https://quay.io/repository/jupyter/scipy-notebook).

---

## 📁 Estrutura do Repositório

```text
data_mine/
├── datasets/                                 # Datasets utilizados nas atividades
│   ├── DTW_prec.csv
│   ├── hotel_bookings.csv
│   ├── Perfil de Escuta Musical ... .csv
│   └── respostas_questionario_musical_... .csv
├── exploratory analysis/                     # Atividade 1 & Revisão
│   ├── exame_review.ipynb
│   ├── Exercicio_Pratico_Hotel_Booking_... .pdf
│   └── preparação_dados_bookings.ipynb
├── pre-processing of data/                   # Atividade 2
│   └── mineracao_de_dados_bookings.ipynb
├── KNN and classification/                   # Atividade 3
│   └── Exercicio_KNN_Perfis_Musicais.ipynb
└── decision tree/                            # Atividade 4
    └── Atividade_Arvore_Decisao_Bank_Marketing.ipynb
```

---

## 📋 Pré-requisitos

1. Ter o **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** instalado no computador.
2. Certifique-se de que o Docker Desktop esteja aberto e em execução.
3. Para validar a instalação, abra o terminal (PowerShell ou CMD) e digite:
   ```bash
   docker --version
   ```
   *(Deverá retornar algo como `Docker version 28.x.x`)*

---

## 🚀 Como Executar o Projeto

Há duas formas principais de trabalhar com o Docker neste projeto:
1. **Criar e executar um novo container do zero** (compartilhando os arquivos da sua máquina via volume).
2. **Reaproveitar um container já criado** para continuar de onde parou.

---

### Opção 1: Criando um Novo Container do Zero (Recomendado)

Ao montar a pasta do projeto como um volume (`-v`), todas as alterações feitas no JupyterLab são salvas diretamente nos arquivos locais do seu computador.

#### 1. Abra o terminal na raiz do projeto (`data_mine`)

#### 2. Execute o comando de inicialização

> **Dica:** O parâmetro `--name jupyter-mine` dá um nome fixo ao container para facilitar iniciá-lo ou pará-lo depois.

* **No PowerShell (usando a pasta atual automaticamente):**
  ```powershell
  docker run -it -p 8888:8888 --name jupyter-mine -v "${PWD}:/home/jovyan/work" quay.io/jupyter/scipy-notebook
  ```

* **No Prompt de Comando (CMD):**
  ```cmd
  docker run -it -p 8888:8888 --name jupyter-mine -v "%cd%:/home/jovyan/work" quay.io/jupyter/scipy-notebook
  ```

* **Ou especificando o caminho absoluto do seu computador:**
  ```bash
  docker run -it -p 8888:8888 --name jupyter-mine -v "C:\Users\SEU_USUARIO\Documents\data_mine:/home/jovyan/work" quay.io/jupyter/scipy-notebook
  ```
  *(Substitua `C:\Users\SEU_USUARIO\Documents\data_mine` pelo caminho real da sua pasta).*

#### Entendendo os parâmetros:
| Parâmetro | Finalidade |
| :--- | :--- |
| `docker run` | Cria e inicia um novo container. |
| `-it` | Modo interativo: conecta o terminal ao container para exibir logs e permitir comandos. |
| `-p 8888:8888` | Mapeia a porta `8888` da sua máquina física para a porta `8888` do container. |
| `--name jupyter-mine` | *(Opcional)* Atribui um nome amigável ao container. |
| `-v "LOCAL:CONTAINER"` | Compartilha a pasta do seu computador com a pasta interna `/home/jovyan/work`. |
| `quay.io/jupyter/scipy-notebook` | Imagem que contém Python, JupyterLab, Pandas, NumPy, Scikit-Learn, Matplotlib, Seaborn, etc. |

> **Nota:** Na primeira vez, o Docker fará o download da imagem (`~3-4 GB`), o que pode demorar alguns minutos dependendo da sua conexão.

---

### Opção 2: Reutilizando um Container Existente

Se você já executou o container anteriormente com o nome `jupyter-mine` (sem o parâmetro `--rm`), ele permanece salvo no Docker e não precisa ser criado novamente.

1. **Listar containers existentes:**
   ```bash
   docker ps -a
   ```
2. **Iniciar o container novamente:**
   ```bash
   docker start -ai jupyter-mine
   ```
   *(A flag `-ai` anexa a saída do terminal para que você veja a URL com o token de login).*

---

## 🌐 Acessando o JupyterLab

1. Após iniciar o container, observe o terminal. Várias linhas de log serão exibidas até aparecer um link com token, parecido com:
   ```text
   http://127.0.0.1:8888/lab?token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```
2. Copie a URL completa (com o `token`) e cole no seu navegador web.
3. No painel lateral esquerdo do JupyterLab, abra a pasta **`work`**.
4. Todos os arquivos e diretórios do repositório (`datasets`, `exploratory analysis`, `KNN and classification`, `decision tree`, etc.) estarão disponíveis para edição e execução.

---

## ⏹️ Encerrando e Gerenciando o Container

* **Parar a execução:**
  - No terminal onde o container está rodando, pressione `Ctrl + C`. O terminal perguntará se deseja encerrar (`y/N`) ou parará automaticamente.
  - Ou, em outro terminal:
    ```bash
    docker stop jupyter-mine
    ```
* **Remover o container (quando quiser criar outro do zero):**
  ```bash
  docker rm jupyter-mine
  ```
  *(Seus arquivos na máquina local **não** serão excluídos, apenas o ambiente do container).*

---

## 🧪 Verificando o Compartilhamento de Arquivos

Para confirmar que as alterações feitas no container persistem na sua máquina:
1. Pelo JupyterLab, entre na pasta `work` e crie um arquivo simples (ex: `teste.txt`).
2. Abra a pasta do repositório no explorador de arquivos do seu computador e confirme a presença do arquivo.
3. Qualquer alteração ou salvamento em notebooks `.ipynb` refletirá instantaneamente nos dois lados.
