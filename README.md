
# **IoT Livestream Sistema de Recompensa*

Este projeto simula um sistema de recompensas para uma livestream usando dispositivos IoT. Toda vez que o usuário aperta um botão no *SimulIDE* (representando uma ação de assistir a uma live), um LED acende e o sistema adiciona pontos. Esses pontos são enviados para o WhatsApp através do Node-RED e também são exibidos em um dashboard com gráficos.

## **Arquitetura do Sistema**

A arquitetura proposta envolve os seguintes componentes:

1. **Arduino Uno (SimulIDE)**: Responsável pela simulação física do botão e do LED.
2. **com0com**: Utilizado para criar uma porta serial virtual que conecta o SimulIDE ao Node-RED.
3. **Node-RED**: Processa a comunicação com o Arduino via serial, envia mensagens WhatsApp e atualiza o dashboard.
   - **Nós usados**: `serial in`, `send message` (da biblioteca *whatsapp-crm*), `gauge` e `chart`.

### **Fluxo de Funcionamento**:
- Ao pressionar o botão no SimulIDE, o LED acende e o código no Arduino adiciona um ponto.
- O valor do ponto é enviado ao Node-RED via porta serial (*com0com*).
- O Node-RED envia uma mensagem de notificação pelo WhatsApp (usando o nó `send message`).
- O dashboard no Node-RED exibe os pontos acumulados em tempo real usando os nós `gauge` e `chart`.

## **Tecnologias Utilizadas**
- **SimulIDE**: Simulador para o Arduino Uno e componentes eletrônicos.
- **com0com**: Emulador de portas seriais virtuais.
- **Node-RED**: Plataforma de desenvolvimento de fluxos de integração IoT.
- **Arduino IDE**: Para escrever o código do Arduino.
- **Biblioteca `whatsapp-crm` para Node-RED**: Para enviar notificações via WhatsApp.
  
## **Como Instalar e Configurar**

### **1. Configuração do Ambiente**
- **SimulIDE**: Baixe e instale o SimulIDE. Monte o circuito com um botão e um LED.
- **Arduino IDE**: Escreva o código Arduino para contar os pontos e acender o LED.
  
  ```cpp
  int buttonPin = 2;
  int ledPin = 13;
  int points = 0;

  void setup() {
    pinMode(buttonPin, INPUT);
    pinMode(ledPin, OUTPUT);
    Serial.begin(9600);
  }

  void loop() {
    if (digitalRead(buttonPin) == HIGH) {
      digitalWrite(ledPin, HIGH);
      points++;
      Serial.println(points);
      delay(1000);
      digitalWrite(ledPin, LOW);
    }
  }
  ```

### **2. Configuração da Porta Serial Virtual (com0com)**
- Instale o `com0com` para criar portas seriais virtuais e conectar o SimulIDE ao Node-RED.
- Configure uma porta virtual (ex.: COM3 ↔ COM4).

### **3. Configuração do Node-RED**
- Instale o Node-RED.
- Adicione a biblioteca `whatsapp-crm` para enviar mensagens via WhatsApp.
- Crie um fluxo no Node-RED com os seguintes nós:
  
  - **`serial in`**: Para receber dados da porta serial.
  - **`send message`**: Para enviar uma mensagem via WhatsApp (biblioteca `whatsapp-crm`).
  - **`gauge` e `chart`**: Para exibir os pontos em tempo real no dashboard.

  #### **Exemplo de Fluxo Node-RED:**
  ```json
  [
      {
          "id": "serial in node",
          "type": "serial in",
          "serial": "com4",
          "wires": [["gauge", "chart", "whatsapp"]]
      },
      {
          "id": "gauge",
          "type": "ui_gauge",
          "wires": []
      },
      {
          "id": "chart",
          "type": "ui_chart",
          "wires": []
      },
      {
          "id": "whatsapp",
          "type": "send message",
          "to": "whatsapp_number",
          "message": "Pontos acumulados: {{payload}}"
      }
  ]
  ```

### **4. Executando a Aplicação**
- Abra o SimulIDE e inicie a simulação do Arduino com o circuito montado.
- No Node-RED, inicie o fluxo. A cada vez que o botão for pressionado no SimulIDE:
  - O LED acenderá.
  - O valor do ponto será enviado ao Node-RED.
  - Uma mensagem será enviada via WhatsApp.
  - O dashboard será atualizado com os novos pontos.

## **Uso**
- Pressione o botão no SimulIDE para simular a interação com uma livestream.
- Verifique o dashboard para acompanhar a contagem de pontos em tempo real.
- Receba uma notificação via WhatsApp sempre que um ponto for adicionado.

## **Recursos Necessários**
- **Hardware**:
  - Arduino Uno (SimulIDE).
  - Botão e LED.
- **Software**:
  - SimulIDE.
  - com0com (para portas seriais virtuais).
  - Node-RED.
  - Arduino IDE.

## **Dependências**
- Node-RED com a biblioteca `whatsapp-crm`.
- Porta serial virtual configurada via com0com.

## Integrantes do grupo

- **Augusto Rocha Silva (RM556316)**


- **Nicolas Lorenzo Ferreira da Silva (RM557962)**


- **Pedro Henrique Faim dos Santos(RM557440)**