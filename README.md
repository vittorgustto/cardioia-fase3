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

# 🩺 CardioIA — Fase 3: Monitoramento Contínuo de Sinais Vitais com IoT

**📘 Introdução**

O projeto CardioIA foi desenvolvido no contexto da disciplina IoT e Sistemas Embarcados da FIAP, com o propósito de aplicar conceitos práticos de Internet das Coisas (IoT) voltados à saúde digital.

O sistema simula um monitor de sinais vitais cardíacos, capaz de capturar dados fisiológicos, processá-los localmente e enviá-los para a nuvem, demonstrando a integração entre Edge Computing, Fog Computing e Cloud Computing.

**Observação: Por instabilidades da plataforma GitHub, se não estiver conseguindo visualizar ou acessar os links das imagens e relatórios da Parte 1 e 2 por este README.MD [acesse a pasta de documentos aqui no repositório](docs/) ou [acesse a pasta de documentos hospedada no OneDrive](https://1drv.ms/f/c/4140def327662c57/EtANW4QQimVLvyKv6U9gTp8Bx5m8fb_pBEWC7Vty167qhA?e=tF8ObI) aonde se encontram estes arquivos e faça o download dos mesmos ou os visualize online.**


**🔹 Parte 1 — Edge Computing (Armazenamento e Processamento Local)**

Nesta etapa, foi desenvolvido no simulador Wokwi um sistema com ESP32 e sensores DHT22 (temperatura e umidade) e sensor de batimentos cardíacos simulado, que:
- Captura dados periodicamente.
- Armazena as leituras no sistema de arquivos SPIFFS.
- Garante resiliência offline: mesmo sem conexão Wi-Fi, os dados continuam sendo gravados localmente até que a conectividade seja restabelecida, momento em que as informações são enviadas e o armazenamento local é limpo.

**🔹 Parte 2 — Fog/Cloud Computing (Transmissão MQTT e Dashboard Node-RED)**

Nesta etapa, os dados são transmitidos via protocolo MQTT para a nuvem (HiveMQ Cloud) e visualizados em tempo real no Node-RED Dashboard.
O sistema exibe gráficos, medidores e alertas automáticos, ilustrando a aplicação de monitoramento remoto de saúde com base em tecnologias de IoT médico.

---

## 🎯 Objetivo

- 📡 Capture sinais vitais simulados (temperatura, umidade, batimentos).
- 💾 Armazene localmente os dados, assegurando resiliência em caso de desconexão (Edge Computing).
- ☁️ Transmita informações para a nuvem via MQTT (Fog/Cloud).
- 📊 Exiba resultados em dashboards interativos, com alertas automáticos.
- 🔒 Promova reflexões sobre segurança, eficiência e boas práticas no contexto da IoT aplicada à saúde.e:

---

## 🔹 Parte 1 — Armazenamento e Processamento Local (Edge Computing)

Nesta primeira parte do projeto CardioIA, o foco foi desenvolver um sistema embarcado simulado com ESP32 no Wokwi, capaz de:

- Capturar sinais vitais de forma simulada (temperatura, umidade e batimentos cardíacos).
- Armazenar os dados localmente no SPIFFS.
- Garantir resiliência offline, continuando a coleta mesmo sem conectividade, e enviar os dados quando a conexão for restabelecida.

Essa etapa demonstra o papel do Edge Computing em aplicações de saúde críticas, onde a continuidade da coleta de dados é essencial.

---

## 🧩 Fluxo de Funcionamento

```
Coleta de dados do DHT22 + batimentos simulados
            │
            ▼
Armazenamento local no SPIFFS
            │
            ▼
Verifica conexão Wi-Fi
 ┌───────────────┴───────────────┐
 │                               │
Offline: continua armazenando    Online: envia via Serial.println() e limpa SPIFFS
```

---

## 💻 Códigos ESP32 (Arduino/Wokwi) e como usar no Wokwi

**0. [Acesse o simulador Wokwi com o Projeto CardioIA Fase 3](https://wokwi.com/projects/445458674379080705)**

ou

**1. Cole esse código no 'skect.ino' no Wokwi (este é o código C++ comentado):**

```
// ============================================================
// Projeto: CardioIA - Fase 3 - Parte 1
// Tema: Armazenamento e Processamento Local (Edge Computing)
// Simulação no Wokwi com ESP32
// Autor: (Seu Nome)
// ============================================================

// --- Bibliotecas necessárias ---
#include <Arduino.h>     // Biblioteca base do Arduino
#include "config.h"      // Arquivo de configuração do projeto

// Se o sensor DHT for habilitado em config.h, inclui a biblioteca DHT
#if USE_DHT
  #include "DHT.h"
  DHT dht(DHT_PIN, DHT_TYPE);   // Inicializa o sensor DHT com o pino e tipo definidos
#endif

// --- Variáveis globais ---
bool wifiConnected = false;          // Simula o estado da conexão Wi-Fi
unsigned long lastSampleMillis = 0;  // Armazena o tempo da última coleta de dados
unsigned long lastBeatWindowStart = 0; // Marca o início da janela de contagem de batimentos
int beatCountWindow = 0;             // Armazena os batimentos dentro da janela
unsigned long totalSamplesStored = 0; // Contador de amostras armazenadas (simulado)

// ============================================================
// FUNÇÕES DE LEITURA DE SENSORES
// ============================================================

// --- Função para leitura de temperatura ---
float readTemperature() {
  #if USE_DHT
    float t = dht.readTemperature(); // Lê a temperatura do DHT22
    if (isnan(t)) return 36.0 + random(-50,50)/100.0; // Gera valor aleatório caso a leitura falhe
    return t;
  #else
    // Caso o DHT não esteja sendo usado, gera valor simulado entre 35°C e 38°C
    return 36.5 + random(-150,150)/100.0;
  #endif
}

// --- Função para leitura de umidade ---
float readHumidity() {
  #if USE_DHT
    float h = dht.readHumidity(); // Lê a umidade do DHT22
    if (isnan(h)) return 50.0 + random(-200,200)/100.0; // Valor simulado caso falhe
    return h;
  #else
    // Caso sem DHT: retorna valor simulado entre 42% e 48%
    return 45.0 + random(-300,300)/100.0;
  #endif
}

// ============================================================
// FUNÇÃO DE LEITURA DE BATIMENTOS CARDÍACOS (BPM)
// ============================================================

// Essa função retorna o número de batimentos por minuto.
// Pode usar um botão (modo real) ou valores simulados.
int getBeatsPerMinute() {
  unsigned long now = millis(); // Tempo atual desde o início do programa

  if (USE_BUTTON_FOR_BEATS) {
    // Caso o modo botão esteja ativo, conta quantos cliques em 10 segundos
    if (now - lastBeatWindowStart >= 10000) { // Janela de 10 segundos
      int bpm = (beatCountWindow * 60000) / 10000; // Converte batidas em BPM
      beatCountWindow = 0; // Reinicia contagem
      lastBeatWindowStart = now;
      return bpm;
    } else {
      return -1; // Indica que ainda está coletando dados
    }
  } else {
    // Caso esteja em modo simulado, gera valor aleatório entre 45 e 105 BPM
    return 60 + random(-15,45);
  }
}

// ============================================================
// INTERRUPÇÃO DO BOTÃO (para simular batimentos reais)
// ============================================================
#if USE_BUTTON_FOR_BEATS
void IRAM_ATTR onBeatButton() {
  beatCountWindow++; // Incrementa batida ao pressionar o botão
}
#endif

// ============================================================
// FUNÇÃO PARA PROCESSAR COMANDOS RECEBIDOS VIA SERIAL
// ============================================================
// Exemplo: enviar pelo terminal do Wokwi "c" para conectar, "d" para desconectar
void processSerialCommands() {
  if (Serial.available()) {
    String cmd = Serial.readStringUntil('\n'); // Lê o comando
    cmd.trim();
    if (cmd.length() == 0) return;

    if (cmd.equalsIgnoreCase("c") || cmd.equalsIgnoreCase("connect")) {
      wifiConnected = true; // Simula conexão Wi-Fi
      Serial.println("[CMD] Simulação: CONNECTED");
    } 
    else if (cmd.equalsIgnoreCase("d") || cmd.equalsIgnoreCase("disconnect")) {
      wifiConnected = false; // Simula desconexão Wi-Fi
      Serial.println("[CMD] Simulação: DISCONNECTED");
    } 
    else if (cmd.equalsIgnoreCase("sync")) {
      Serial.println("[CMD] Sincronização não implementada na simulação.");
    } 
    else if (cmd.equalsIgnoreCase("status")) {
      // Mostra status de conexão e quantidade de amostras armazenadas
      Serial.printf("[STATUS] conectado=%d samples=%lu\n", wifiConnected ? 1 : 0, totalSamplesStored);
    } 
    else {
      Serial.printf("[CMD] Comando desconhecido: %s\n", cmd.c_str());
    }
  }
}

// ============================================================
// FUNÇÃO DE ARMAZENAMENTO SIMULADO (sem SPIFFS real)
// ============================================================
// Aqui simulamos a gravação de dados localmente (Edge Computing)
bool appendSampleSimulated(const String &line) {
  Serial.print("[SIMULATED_SAVE] ");  // Mostra no terminal o dado "armazenado"
  Serial.println(line);
  totalSamplesStored++;               // Incrementa contador de amostras
  Serial.printf("[STORE] Amostra simulada armazenada (total=%lu).\n", totalSamplesStored);
  return true;
}

// ============================================================
// FUNÇÃO DE CONFIGURAÇÃO INICIAL (setup)
// ============================================================
void setup() {
  Serial.begin(115200);    // Inicializa comunicação serial
  delay(2000);             // Aguarda estabilização

  Serial.println("CardioIA - Fase 3 Parte 1 (Simulação Wokwi)");
  Serial.println("----------------------------------------------------");
  Serial.println("[INFO] Modo simulado de armazenamento (sem SPIFFS real no Wokwi).");

  #if USE_DHT
    dht.begin(); // Inicializa o sensor DHT22
    Serial.println("DHT22 inicializado com sucesso.");
  #endif

  #if USE_BUTTON_FOR_BEATS
    pinMode(BUTTON_PIN, INPUT_PULLUP); // Configura botão com resistor interno
    attachInterrupt(digitalPinToInterrupt(BUTTON_PIN), onBeatButton, FALLING); // Interrupção para batimento
    lastBeatWindowStart = millis();
    Serial.println("Botão de batimentos configurado.");
  #endif

  Serial.println("Use 'c' (connect) ou 'd' (disconnect) via Serial para simular conexão.");
  Serial.println("----------------------------------------------------");
}

// ============================================================
// LOOP PRINCIPAL DO PROGRAMA
// ============================================================
// Este loop executa leituras periódicas dos sensores e "armazena" localmente
void loop() {
  processSerialCommands();  // Verifica comandos recebidos via Serial

  unsigned long now = millis(); // Tempo atual

  // Executa a cada intervalo de amostragem definido em config.h
  if (now - lastSampleMillis >= SAMPLE_INTERVAL_MS) {
    lastSampleMillis = now;

    float temp = readTemperature();  // Lê temperatura
    float hum = readHumidity();      // Lê umidade
    int bpm = getBeatsPerMinute();   // Lê batimentos (simulados ou reais)

    // Se estiver usando botão e janela ainda não terminou, apenas aguarda
    if (USE_BUTTON_FOR_BEATS && bpm == -1) {
      Serial.println("[SAMPLE] Aguardando completar janela de batimentos...");
    } 
    else {
      // Monta o JSON com as medições
      String sample = "{";
      sample += "\"timestamp\":" + String(now);
      sample += ",\"temp\":" + String(temp,2);
      sample += ",\"hum\":" + String(hum,2);
      sample += ",\"bpm\":" + String(bpm);
      sample += "}";

      // Mostra o dado coletado e simula armazenamento local
      Serial.print("[SAMPLE] ");
      Serial.println(sample);
      appendSampleSimulated(sample);
    }
  }

  delay(10); // Pequeno atraso para estabilidade
}

```

**2. Cole este código em 'diagram.json':**

```
{
  "version": 1,
  "author": "Vitor Gomes",
  "editor": "wokwi",
  "parts": [
    { "type": "board-esp32-devkit-c-v4", "id": "esp", "top": -48, "left": 33.64, "attrs": {} },
    {
      "type": "wokwi-pushbutton",
      "id": "btn",
      "top": 73.4,
      "left": -144,
      "attrs": { "color": "green", "xray": "1" }
    },
    { "type": "wokwi-dht22", "id": "dht", "top": -191.7, "left": -53.4, "attrs": {} }
  ],
  "connections": [
    [ "esp:TX", "$serialMonitor:RX", "", [] ],
    [ "esp:RX", "$serialMonitor:TX", "", [] ],
    [ "dht:VCC", "esp:3V3", "red", [] ],
    [ "dht:GND", "esp:GND.1", "black", [] ],
    [ "dht:SDA", "esp:4", "green", [] ],
    [ "btn:1.l", "esp:14", "green", [] ],
    [ "btn:2.r", "esp:GND.1", "green", [] ]
  ],
  "dependencies": {}
}
```

**3. Crie um novo arquivo chamado 'config.h' no ESP32 do Wokwi e cole este código dentro dele:**

```
// config.h - parâmetros configuráveis para testes rápidos

#pragma once

// ----- Sensores -----
#define USE_DHT  true            // true = usar DHT22 real no Wokwi; false = simula temperatura/umidade
#define DHT_PIN  4                // pino DHT (se usar)
#define DHT_TYPE DHT22

// Segundo sensor: botão pra simular batimentos
#define USE_BUTTON_FOR_BEATS true // true = usa um botão virtual no Wokwi para contar batimentos
#define BUTTON_PIN 14             // pino do botão (se usar)
#define BEAT_WINDOW_MS 10000      // janela para contagem de batimentos (aqui 10s para teste rápido)

// ----- Amostragem -----
#define SAMPLE_INTERVAL_MS 2000   // intervalo entre amostras (2s para teste)
#define SYNC_INTERVAL_MS 5000     // tenta sincronizar a cada 5s quando conectado (teste)

// ----- Resiliência / SPIFFS -----
#define MAX_SAMPLES_STORED 20     // limite pequeno para testes (20 amostras)

```

**4. Instale a biblioteca 'DHT Sensor Library' no mesmo ambiente do ESP32 do Wokwi**

**5. Rode a simulação**

---

## 📸 Evidências (prints)

A imagem abaixo mostra:

- Interface do Wokwi simulando dados do ESP32
- Batimentos cardíacos simulados por cliques
- Dados armazenados e enviados via Serial Monitor
- Armazenamento local (SPIFFS).
- Lógica de resiliência offline/online.

![Print do ESP32 no Wokwi](./docs/Parte%201/Print-ESP32-Wokwi.png)

**Caso não esteja conseguindo visualizer [entre neste link, entre na pasta Parte 1 e visualize a imagem](https://1drv.ms/f/c/4140def327662c57/EtANW4QQimVLvyKv6U9gTp8Bx5m8fb_pBEWC7Vty167qhA?e=tF8ObI)**


---

## 🧾 Relatório Técnico

O relatório da Parte 1 descreve:

- Coleta de dados e simulação de sensores.
- Armazenamento local (SPIFFS).
- Lógica de resiliência offline/online.

📄 [Relatório CardioIA Fase 3 - Parte 1](docs/Parte%201/Relatório-CardioIA-Fase-3-Parte-1.docx)

**Caso não esteja conseguindo visualizer [entre neste link, entre na pasta Parte 1 e visualize o documento](https://1drv.ms/f/c/4140def327662c57/EtANW4QQimVLvyKv6U9gTp8Bx5m8fb_pBEWC7Vty167qhA?e=tF8ObI)**


---

## 🔹 Parte 2 — Comunicação MQTT e Dashboard no Node-RED

Esta segunda etapa do projeto CardioIA tem como objetivo estabelecer a comunicação MQTT entre o dispositivo ESP32 (ou simulação via Node-RED) e o painel de monitoramento (Dashboard) desenvolvido no Node-RED, permitindo acompanhar em tempo real os parâmetros vitais de um paciente simulado — temperatura corporal, umidade e batimentos cardíacos (BPM).

O projeto representa uma arquitetura IoT simples, segura e escalável para aplicações de monitoramento remoto de saúde, utilizando o protocolo MQTT e o broker em nuvem HiveMQ Cloud.

---

🧱 Requisitos

- Node.js e Node-RED instalados
- Conexão MQTT (HiveMQ Cloud)
- Dashboard Node-RED habilitado (node-red-dashboard)

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

![Gráfico Node-RED](./docs/Parte%202/Print-Node-RED.png)

![Dashboard Node-RED](./docs/Parte%202/Print-Node-RED-Dashboard.png)

**Caso não esteja conseguindo visualizer [entre neste link, entre na pasta Parte 2 e visualize as imagens](https://1drv.ms/f/c/4140def327662c57/EtANW4QQimVLvyKv6U9gTp8Bx5m8fb_pBEWC7Vty167qhA?e=tF8ObI)**

---

## 🧾 Relatório Técnico

O relatório detalhado sobre o fluxo MQTT e a configuração do dashboard encontra-se no arquivo:

📄 [Relatório CardioIA Fase 3 - Parte 2](docs/Parte%202/Relatório-CardioIA-Fase-3-Parte-2.docx)

**Caso não esteja conseguindo visualizer [entre neste link, entre na pasta Parte 2 e visualize o documento](https://1drv.ms/f/c/4140def327662c57/EtANW4QQimVLvyKv6U9gTp8Bx5m8fb_pBEWC7Vty167qhA?e=tF8ObI)**

---

## 🏆 Conclusão Geral

O projeto CardioIA demonstrou de forma prática e integrada como as tecnologias de IoT, Edge Computing, Fog Computing e Cloud Computing podem ser aplicadas no contexto da saúde digital.

A Parte 1 evidenciou o papel da resiliência offline e do armazenamento local, enquanto a Parte 2 mostrou a transmissão de dados e visualização em tempo real.

Com isso, o sistema oferece uma base sólida para futuras implementações reais, como:

- Integração com banco de dados em nuvem.
- Notificações via aplicativo móvel.
- Análise preditiva com IA para detecção precoce de anomalias.

---

## 🗂 Estrutura Completa do Repositório

```
cardioia-fase3/
├─ assets/
├─ docs/
│  ├─ Parte1/
│  │  ├─ Print-ESP32-Wokwi.png
│  │  └─ Relatório-CardioIA-Fase-3-Parte-1.docx
│  ├─ Parte2/
│  │  ├─ Print-Node-RED.png
│  │  ├─ Print-Node-RED-Dashboard.png
│  │  └─ Relatório-CardioIA-Fase-3-Parte-2.docx
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




