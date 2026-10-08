# 🌱 Monitoramento de Ambiente com ESP32 e IoT

Projeto desenvolvido utilizando **ESP32** para monitoramento de condições ambientais, realizando a leitura de **temperatura, umidade e luminosidade**. Os dados são apresentados em um display LCD, utilizados para identificar situações de alerta e enviados para uma plataforma de monitoramento em nuvem.

O projeto foi desenvolvido e simulado utilizando o **Wokwi**, permitindo testar o funcionamento do circuito sem a necessidade de componentes físicos.

---

## 📌 Sobre o Projeto

O sistema tem como objetivo realizar o monitoramento de um ambiente utilizando sensores conectados a um ESP32.

A partir das informações coletadas, o sistema verifica se os valores estão dentro dos limites estabelecidos. Caso alguma condição esteja fora do intervalo esperado, um **LED de alerta é acionado e um buzzer emite um sinal sonoro**.

Além disso, os dados coletados são enviados periodicamente para o **ThingSpeak**, possibilitando o acompanhamento das informações por meio da nuvem.

### Informações monitoradas

* 🌡️ Temperatura
* 💧 Umidade
* 💡 Luminosidade

---

## ⚙️ Funcionamento

O ESP32 realiza continuamente a leitura dos sensores conectados ao circuito.

Os valores são comparados com os limites definidos no programa:

| Parâmetro       | Limite        |
| --------------- | ------------- |
| 🌡️ Temperatura | 18°C a 30°C   |
| 💧 Umidade      | 40% a 80%     |
| 💡 Luminosidade | Mínimo de 20% |

Quando todos os valores estão dentro dos limites:

* LED verde é acionado;
* LCD informa que as condições estão **OK**.

Quando algum valor ultrapassa os limites:

* LED vermelho é acionado;
* buzzer emite um alerta sonoro;
* LCD informa a situação de **ALERTA**.

---

## 🔌 Componentes Utilizados

* ESP32 DevKit
* Sensor de temperatura e umidade **DHT22**
* Sensor de luminosidade **LDR**
* Display LCD 16x2 com interface I2C
* LED verde
* LED vermelho
* Buzzer
* Resistores de 220 Ω
* Conexões virtuais do Wokwi

---

## 🧰 Tecnologias e Ferramentas

* **C++ / Arduino**
* **ESP32**
* **Wokwi**
* **PlatformIO**
* **Wi-Fi**
* **HTTP**
* **ThingSpeak**
* **DHT22**
* **LiquidCrystal I2C**

### Bibliotecas utilizadas

```text
Adafruit DHT sensor library
Adafruit Unified Sensor
LiquidCrystal_I2C
WiFi
HTTPClient
```

---

## 🗺️ Arquitetura do Projeto

O funcionamento pode ser representado da seguinte forma:

```text
┌───────────────┐
│    Sensores   │
│ DHT22 + LDR   │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│     ESP32     │
│ Processamento │
└───────┬───────┘
        │
   ┌────┴─────┐
   ▼          ▼
┌───────┐  ┌────────────┐
│  LCD  │  │ LED + Buzzer│
└───────┘  └────────────┘
        │
        ▼
┌───────────────┐
│     Wi-Fi      │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   ThingSpeak  │
│     Nuvem     │
└───────────────┘
```

---

## 📟 Display LCD

O display LCD apresenta as principais informações coletadas pelos sensores.

Exemplo:

```text
T:25.4C H:65%
Luz:75% OK
```

Em uma situação de alerta:

```text
T:32.1C H:70%
Luz:80% ALERTA
```

---

## 🚨 Sistema de Alertas

O sistema utiliza dois LEDs para indicar o estado do ambiente.

### 🟢 LED Verde

Indica que:

* Temperatura está adequada;
* Umidade está adequada;
* Luminosidade está acima do limite mínimo.

### 🔴 LED Vermelho

É acionado quando:

* Temperatura está abaixo de 18°C;
* Temperatura está acima de 30°C;
* Umidade está abaixo de 40%;
* Umidade está acima de 80%;
* Luminosidade está abaixo de 20%.

O buzzer também é acionado quando uma situação de alerta é identificada.

---

## ☁️ Envio dos Dados para a Nuvem

O projeto utiliza o **ThingSpeak** para armazenar os dados coletados pelo ESP32.

Os valores são enviados utilizando uma requisição HTTP:

```text
Temperatura → Field 1
Umidade     → Field 2
Luminosidade → Field 3
```

O envio ocorre aproximadamente a cada **20 segundos**, respeitando o intervalo mínimo utilizado pelo serviço gratuito do ThingSpeak.

> ⚠️ Para utilizar o envio para o ThingSpeak, é necessário substituir `SUA_WRITE_API_KEY` no código pela chave de escrita do seu canal.

---

## ▶️ Como Executar

### 1. Clonar o projeto

```bash
git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
```

### 2. Abrir o projeto

Abra a pasta do projeto no **Visual Studio Code** com a extensão **PlatformIO** instalada.

### 3. Executar no Wokwi

O projeto contém a configuração necessária para simulação do ESP32.

No Wokwi, o circuito pode ser executado utilizando os componentes definidos no arquivo:

```text
diagram.json
```

### 4. Configurar o ThingSpeak

No arquivo principal:

```cpp
const char* API_KEY = "SUA_WRITE_API_KEY";
```

Substitua pela sua chave de escrita do ThingSpeak.

---

## 📁 Estrutura do Projeto

```text
Atividade_wokwi/
│
├── .pio/
│
├── include/
│
├── lib/
│
├── src/
│   └── main.cpp
│
├── test/
│
├── diagram.json
├── platformio.ini
├── wokwi.toml
└── README.md
```

### Principais arquivos

**`src/main.cpp`**

Contém a lógica principal do sistema, incluindo:

* Inicialização do ESP32;
* Leitura dos sensores;
* Controle dos LEDs;
* Controle do buzzer;
* Atualização do LCD;
* Conexão Wi-Fi;
* Envio dos dados para o ThingSpeak.

**`diagram.json`**

Define os componentes e as conexões utilizadas na simulação do circuito no Wokwi.

**`platformio.ini`**

Contém as configurações do projeto no PlatformIO, incluindo a placa ESP32, framework Arduino e bibliotecas utilizadas.

**`wokwi.toml`**

Define as configurações necessárias para executar a simulação do projeto no Wokwi.

---

## 🎯 Objetivos do Projeto

* Desenvolver uma aplicação básica de **Internet das Coisas (IoT)**;
* Aprender a utilizar o ESP32;
* Realizar leitura de sensores;
* Processar dados ambientais;
* Implementar um sistema de alertas;
* Exibir informações em um display LCD;
* Utilizar comunicação Wi-Fi;
* Enviar informações para uma plataforma de nuvem;
* Praticar programação utilizando C++ e Arduino.

---

## 🚀 Possíveis Melhorias

Algumas funcionalidades podem ser adicionadas futuramente:

* 📊 Criação de gráficos em tempo real;
* 📱 Desenvolvimento de um aplicativo para acompanhamento dos dados;
* 🔔 Envio de notificações quando houver uma situação de alerta;
* 💾 Armazenamento local dos dados;
* 🌐 Desenvolvimento de um dashboard próprio;
* ⚙️ Configuração dos limites diretamente pelo usuário;
* 🔐 Implementação de autenticação;
* 📈 Histórico das medições;
* 🌱 Adição de outros sensores para monitoramento ambiental.

---

## 👨‍💻 Autor

**Kauan Vilas Boas Barbosa**

Estudante de Desenvolvimento de Sistemas.

### Tecnologias de interesse

`HTML` `CSS` `JavaScript` `Python` `SQL` `C++` `IoT` `ESP32`

---

## 📚 Contexto Acadêmico

Projeto desenvolvido com finalidade **acadêmica e educacional**, com foco na aplicação prática dos conceitos de programação, sistemas embarcados, sensores, comunicação de dados e Internet das Coisas (IoT).

---

## ⭐ Status

🟢 **Concluído — Projeto acadêmico**

O projeto pode receber novas funcionalidades e melhorias conforme o avanço dos estudos em IoT e sistemas embarcados.
