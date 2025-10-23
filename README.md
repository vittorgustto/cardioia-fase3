# FIAP - Faculdade de Informática e Administração Paulista

<p align="center">
  <a href="https://www.fiap.com.br/">
    <img src="./assets/logo-fiap.png" alt="FIAP - Faculdade de Informática e Administração Paulista" style="border:0; width:40%; height:40%;">
  </a>
</p>

<br>


## Grupo 38

## 👨‍🎓 Integrantes: 
- <a href="https://www.linkedin.com/in/vittor-augusto/">Vitor Augusto Gomes</a>
- <a href="https://www.linkedin.com/in/jo%C3%A3o-vitor-lopes-beiro-59a007248/">João Vitor Lopes Beiro</a>

## 👩‍🏫 Professores:
### Tutor(a) 
- <a href="https://www.linkedin.com/in/leonardoorabona/">Leonardo Ruiz Orabona</a>
### Coordenador(a)
- <a href="https://www.linkedin.com/in/profandregodoi/">André Godoi Chiovato</a>


## 📜 Descrição

# 🩺 CardioIA — Fase 2

**Diagnóstico Automatizado: IA no Estetoscópio Digital**

Este repositório contém a Fase 2 do projeto CardioIA, dividida em duas partes:

- Parte 1: Extração de sintomas e sugestão de diagnóstico.

- Parte 2: Classificação de risco em frases clínicas com Machine Learning.

---

## 📌 Objetivo

O propósito desta fase é simular a automatização do diagnóstico com IA, mostrando como algoritmos simples aliados a dados bem estruturados podem apoiar médicos em processos de triagem e decisão clínica.

---

## 🔹 Parte 1 — Diagnóstico Automático

A Parte 1 simula um sistema especialista baseado em regras, no qual sintomas mencionados por pacientes em frases livres são detectados automaticamente e relacionados a possíveis diagnósticos.

- **Entrada:** frases de pacientes (ex.: “Estou com dor no peito e falta de ar”).
- **Processo:** identificação de sintomas com base no mapa_conhecimento.csv.
- **Saída:** lista de sintomas detectados e diagnósticos sugeridos.

💡 Essa etapa mostra como sistemas simples de correspondência podem apoiar triagens médicas iniciais.

---

## 🚀 Como Executar

1. Abra a pasta "Parte 1" no VS Code.

2. Execute o script no terminal:

```
python diagnostico.py
```

3. A saída será gerada no arquivo:

```
resultados_diagnostico.csv
```

4. O arquivo contém:

- A frase original.
- Sintomas detectados.
- Diagnósticos sugeridos.

💡 Exemplo de resultado esperado:
| frase                          | sintomas_detectados | diagnosticos_sugeridos |
| ------------------------------ | ------------------- | ---------------------- |
| "Sinto dor no peito há 2 dias" | dor no peito        | Infarto                |

---

## 🔹 Parte 2 — Classificador de Risco

A Parte 2 amplia o projeto para o uso de Machine Learning supervisionado, onde um modelo é treinado para classificar frases clínicas em alto risco ou baixo risco.

- **Entrada:** dataset rotulado (frases_risco.csv).
- **Processo:** pré-processamento com TF-IDF, treinamento com algoritmos do Scikit-learn e avaliação do desempenho.
- **Saída:** predição de risco para novas frases (ex.: “Falta de ar intensa” → Alto risco).

💡 Essa etapa evidencia o uso prático de IA para apoio à tomada de decisão, priorizando pacientes em situações críticas.

---

## 🚀 Como Executar

1. Abra o notebook classificador_risco.ipynb.
2. Carregue o dataset frases_risco.csv.
3. Execute todas as células na ordem do notebook:

- Pré-processamento com TF-IDF.
- Treinamento do modelo (ex.: Regressão Logística ou Naive Bayes).
- Avaliação (acurácia, matriz de confusão, exemplos de predição).

💡 Exemplo de uso no final do notebook:
```
Frase: "Estou com falta de ar e dor no peito"
Predição: Alto Risco
```

---

## 📊 Tecnologias Utilizadas

- Python 3
- Pandas — manipulação de dados
- Scikit-learn — vetorização TF-IDF, treino e avaliação de modelos
- Matplotlib — gráficos e matriz de confusão
- Jupyter Notebook / Google Colab

---

## ▶️ Demonstração em Vídeo

📹 [Clique aqui para assistir no YouTube](https://www.youtube.com/watch?v=CAedP-GF2Mo)  

---

## 🔎 Conclusão

A **Fase 2 do CardioIA** demonstra duas abordagens complementares para diagnóstico automatizado:

1. **Regras baseadas em sintomas (Parte 1)** — úteis para triagens rápidas.
2. **Machine Learning supervisionado (Parte 2)** — capaz de aprender padrões e generalizar para novos casos.

Essas técnicas reforçam como a **IA pode apoiar a medicina** ao oferecer ferramentas de análise inicial, organização da informação clínica e suporte à decisão médica, sem substituir a avaliação profissional.

---

## 🗂 Estrutura dos Arquivos (Parte 1 e 2)

```
cardioia-fase2/
├─ assets/
├─ docs/
│  ├─ Parte1/
│  │  ├─ diagnostico.py              # script que analisa frases e sugere diagnósticos
│  │  ├─ sintomas.txt                # 10 frases simuladas de pacientes
│  │  ├─ mapa_conhecimento.csv       # mapa de sintomas → doenças
│  │  └─ resultados_diagnostico.csv  # saída gerada
│  ├─ Parte2/
│  │  ├─ classificador.ipynb         # notebook com TF-IDF, treino e avaliação do modelo
│  │  └─ frases_risco.csv            # dataset com frases e rótulos (alto/baixo risco)
└─ README
```

---

# 🫀 CardioIA – Fase 2: Ir Além 1 – Interface do CardioIA

O objetivo é construir a interface do **CardioIA** em **React + Vite**, simulando um portal de cardiologia com autenticação fake, listagem de pacientes, agendamento de consultas e um dashboard com métricas.

---

## 📌 Funcionalidades

- 🔑 **Autenticação simulada** via Context API (login fake, armazenado em estado).
- 👨‍⚕️ **Listagem de pacientes** consumindo dados de uma API fake (JSONPlaceholder).
- 📅 **Formulário de agendamento de consultas** usando `useState` e `useReducer`.
- 📊 **Dashboard simples** com:
  - Número total de pacientes.
  - Número total de consultas agendadas.
  - Gráfico ilustrativo com **Recharts**.
- 🔒 **Proteção de rotas**: apenas usuários logados conseguem acessar pacientes, agendamentos e dashboard.
- 🎨 **Estilização responsiva** utilizando CSS Modules.

---

## 🚀 Como executar o projeto

1️. **Download dos arquivos**

```bash
Faça o download do arquivo "ir_alem1_frontend.zip" e extraia ele. O resultado será a pasta "ir_alem1_frontend" contendo todos os arquivos do portal.
```

2. **Abrir no VS Code**
```
Com o VS Code aberto abra a pasta "ir_alem1_frontend" no VS Code.
```

3. **Instalar as dependências**
```
npm install
```

4. **Instalar a biblioteca de gráficos (Recharts)**
```
npm install recharts
```

5. **Rodar a aplicação**
```
npm run dev
```

A aplicação estará disponível em:
👉 http://localhost:5173

---

## 🧪 Login Simulado

Para acessar o portal, use qualquer e-mail e senha no login.
Exemplo:

```
email: teste@teste.com
senha: 123456
```

---

## ▶️ Demonstração em Vídeo

📹 [Clique aqui para assistir no YouTube](https://www.youtube.com/watch?v=cAYX2YwVrxs)  

---

## 📑 Observações

Este projeto não possui back-end real. Todos os dados são simulados via JSONPlaceholder e estados internos do React. O objetivo é demonstrar boas práticas de Front-End:


  - Componentização
  - Hooks (useState, useEffect, useContext, useReducer)
  - Context API
  - Roteamento protegido

---

## 📂 Estrutura dos Arquivos (Ir Além 1)

```
cardioia-fase2/
├─ assets/
├─ docs/
│  ├─ Ir Além 1
│  │  └─ ir_alem1_frontend.zip
└─ README
```

---

# 🫀 CardioIA – Fase 2: Ir Além 2 – Diagnóstico visual em cardiologia com MLP

Este projeto aplica uma **Rede Neural Artificial (MLP – Perceptron Multicamadas)** para classificar imagens médicas de **eletrocardiogramas (ECG)** em **normal** ou **anormal**.  

Ele faz parte do desafio *CardioIA*, ampliando o uso da Inteligência Artificial para diagnósticos visuais e reforçando o papel da IA no apoio à decisão médica.

---

## 📊 Dataset

- **Fonte:** [Kaggle – Heartbeat Dataset](https://www.kaggle.com/datasets/shayanfazeli/heartbeat)  
- Classes:  
  - **Normal** → ECGs saudáveis  
  - **Anormal** → ECGs com irregularidades  

O dataset foi balanceado para conter o mesmo número de amostras normais e anormais.

---

## ⚙️ Etapas do Projeto

1. **Pré-processamento das imagens**
   - Conversão para tons de cinza
   - Redimensionamento para 128x128 pixels
   - Normalização para valores entre 0 e 1  

2. **Construção do modelo MLP (Keras)**
   - Camada de entrada (Flatten)  
   - Camadas densas ocultas com ReLU e Dropout  
   - Camada de saída com ativação Sigmoid  

3. **Treinamento**
   - Função de perda: `binary_crossentropy`  
   - Otimizador: `adam`  
   - Early Stopping para evitar overfitting  

4. **Avaliação**
   - Métricas: Acurácia, Precisão, Recall, F1-score  
   - Matriz de confusão  

---

## 🚀 Como Executar

1️. Faça o download do notebook "rede_neural_ecg.ipynb" e do arquivo "kaggle.json"
   
2. Abra o notebook no Google Colab ou Jupyter.

3. Faça upload do arquivo kaggle.json na seção "Arquivos" do Colab

4. Execute todas as células na ordem.

---

## 📈 Resultados

- **Acurácia no conjunto de teste:** ~91%  
- **Relatório de classificação:**

```
          precision    recall  f1-score   support

  normal       0.88      0.95      0.92      1012
 anormal       0.95      0.87      0.91      1011

accuracy                           0.91      2023
macro avg      0.92      0.91      0.91      2023
weighted avg   0.92      0.91      0.91      2023

```

**Matriz de Confusão:**

|               | Pred Normal | Pred Anormal |
|---------------|-------------|--------------|
| **True Normal**   | 964         | 48           |
| **True Anormal**  | 127         | 884          |

![Gráfico Matriz de Confusão](./assets/Gráfico-Matriz-de-Confusão.png)


---

## ▶️ Demonstração em Vídeo

📹 [Clique aqui para assistir no YouTube](https://www.youtube.com/watch?v=LlpKeJxpuuE)  

---

## 🏆 Conclusão

O modelo MLP foi capaz de alcançar 91% de acurácia, mostrando que mesmo arquiteturas simples podem apoiar tarefas de triagem médica em ECGs.

Este resultado reforça a importância da IA na área da saúde, auxiliando profissionais na detecção precoce de anomalias cardíacas.

---

## Estrutura dos Arquivos (Ir Além 2)

```
cardioia-fase2/
├─ assets/
├─ docs/
│  ├─ Ir Além 2
│  │  ├─ kaggle.json
│  │  └─ rede_neural_ecg.ipynb
└─ README
```

---

## 🗂 Estrutura Completa do Repositório

```
cardioia-fase2/
├─ assets/
├─ docs/
│  ├─ Parte1/
│  │  ├─ diagnostico.py
│  │  ├─ frases.txt
│  │  ├─ mapa_conhecimento.csv
│  │  └─ resultados_diagnostico.csv
│  ├─ Parte2/
│  │  ├─ classificador.ipynb
│  │  └─ frases_risco.csv
│  ├─ Ir Além 1
│  │  └─ ir_alem1_frontend.zip
│  ├─ Ir Além 2
│  │  ├─ kaggle.json
│  │  └─ rede_neural_ecg.ipynb
└─ README.md
```

---

## 📁 Estrutura de pastas

Dentre os arquivos e pastas presentes na raiz do projeto, definem-se:

- <b>.github</b>: Nesta pasta ficarão os arquivos de configuração específicos do GitHub que ajudam a gerenciar e automatizar processos no repositório.

- <b>assets</b>: aqui estão os arquivos relacionados a elementos não-estruturados deste repositório, como imagens.

- <b>config</b>: Posicione aqui arquivos de configuração que são usados para definir parâmetros e ajustes do projeto.

- <b>docs</b>: aqui estão todos os documentos do projeto que as atividades poderão pedir. Na subpasta "other", adicione documentos complementares e menos importantes.

- <b>scripts</b>: Posicione aqui scripts auxiliares para tarefas específicas do seu projeto. Exemplo: deploy, migrações de banco de dados, backups.

- <b>src</b>: Todo o código fonte criado para o desenvolvimento do projeto ao longo das 7 fases.

- <b>README.md</b>: arquivo que serve como guia e explicação geral sobre o projeto (o mesmo que você está lendo agora).



## 📋 Licença

<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/agodoi/template">MODELO GIT FIAP</a> por <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://fiap.com.br">Fiap</a> está licenciado sobre <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International</a>.</p>




