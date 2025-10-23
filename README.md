# FIAP - Faculdade de Informática e Administração Paulista

<p align="center">
  <a href="https://www.fiap.com.br/">
    <img src="./assets/logo-fiap.png" alt="FIAP - Faculdade de Informática e Administração Paulista" style="border:0; width:40%; height:40%;">
  </a>
</p>

<br>


## Grupo 37

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

## 🔹 Parte 2 — Comunicação MQTT e Dashboard no Node-RED

Esta segunda etapa do projeto CardioIA tem como objetivo estabelecer a comunicação MQTT entre o dispositivo ESP32 (ou simulação via Node-RED) e o painel de monitoramento (Dashboard) desenvolvido no Node-RED, permitindo acompanhar em tempo real os parâmetros vitais de um paciente simulado — temperatura corporal, umidade e batimentos cardíacos (BPM).

O projeto representa uma arquitetura IoT simples, segura e escalável para aplicações de monitoramento remoto de saúde, utilizando o protocolo MQTT e o broker em nuvem HiveMQ Cloud.

---

## 🧩 Arquitetura do Sistema

O fluxo de comunicação segue o modelo Publish/Subscribe, típico do protocolo MQTT:

```
ESP32 (Publisher) → HiveMQ Cloud (Broker MQTT) → Node-RED (Subscriber)
```

1. O ESP32 (ou simulação) coleta os dados e publica no tópico:

```
cardioIA/vitor/telemetry
```

2.  O HiveMQ Cloud atua como broker, intermediando a comunicação.

3.  O Node-RED, configurado como assinante (subscriber), recebe os dados, processa e exibe as informações no Dashboard CardioIA.

---

## ⚙️ Configuração do HiveMQ Cloud

1. [Acesse HiveMQ Cloud Console](https://console.hivemq.cloud/)

2. Crie um Serverless Cluster gratuito.

3. Na aba Connection Details, copie:

- Broker hostname: seu_cluster_id.s1.eu.hivemq.cloud
- Porta MQTT TLS: 8883

4. Na aba Access Management, crie um usuário e senha para autenticação MQTT.

Essas credenciais serão usadas no Node-RED nos nós ``` mqtt in ``` e ``` mqtt out ```.

---

## 💡 Fluxo no Node-RED

O fluxo é composto pelos seguintes nós:

| Tipo de Nó       | Função       |
|----------------|----------------|
| Inject        | Gera dados simulados periodicamente (caso o ESP32 não esteja conectado)        |
| Function        | Converte os dados simulados em formato JSON        |
| MQTT Out        | Publica os dados no tópico cardioIA/vitor/telemetry        |
| MQTT In        | Recebe os dados do broker MQTT        |
| JSON        | Converte a string recebida para objeto JSON        |
| Gauge        | Exibe o valor atual de BPM        |
| Chart        | Mostra a variação de temperatura e umidade em tempo real        |
| Text        | (Alerta)	Exibe mensagens de alerta automáticas        |

**📍 Observação:**

Os nós MQTT devem estar configurados com:

- Servidor: seu_cluster_id.s1.eu.hivemq.cloud
- Porta: 8883
- Usar TLS: ✔️ Marque esta opção
- Usuário e Senha: conforme criados no HiveMQ Cloud

---

## 🖥️ Interface (Dashboard)

A interface foi desenvolvida no Node-RED Dashboard e pode ser acessada em:

👉 http://127.0.0.1:1880/ui

Estrutura do painel:
- Título: CardioIA Monitor
- Gauge (BPM): indicador analógico de batimentos cardíacos
- Chart: gráfico de temperatura e umidade
- Texto de Alerta: exibe mensagens como:
  - ✅ Tudo normal
  - ⚠️ Temperatura alta: 38.6°C
  - ⚠️ BPM elevado: 110

---

## 🧠 Como o sistema funciona

1. O ESP32 (ou simulador) gera leituras a cada intervalo configurado.
2. Os dados são publicados no broker MQTT.
3. O Node-RED assina o mesmo tópico e recebe as mensagens JSON.
4. As leituras são processadas, exibidas em tempo real e comparadas com limites predefinidos.
5. Caso a temperatura > 38 °C ou o BPM > 100, um alerta automático é mostrado no painel.

---

## 📸 Evidências (prints)

As imagens abaixo mostram o funcionamento do painel e do fluxo:

- Fluxo MQTT configurado no Node-RED
- Dashboard em execução com atualização em tempo real
- Alertas automáticos sendo disparados

![Gráfico Node-RED](./assets/Gráfico-Matriz-de-Confusão.png)

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




