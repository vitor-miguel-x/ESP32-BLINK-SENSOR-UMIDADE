# Projeto: Monitor de Umidade de Solo com ESP32 

Este repositório contém o projeto de um sistema de monitoramento de umidade de solo desenvolvido para a plataforma **ESP32**. O sistema utiliza um sensor analógico para medir o nível de hidratação e aciona um alerta visual (LED) quando o solo está seco.

---

## 📝 Descrição do Projeto
O objetivo desta atividade é realizar a leitura de dados analógicos através do **ADC (Analog-to-Digital Converter)** do ESP32. 
- O sensor envia um valor variável que corresponde ao nível de umidade.
- Se o valor lido ultrapassar o **limiar de 2000**, o sistema identifica que o solo precisa de água e acende o LED de alerta.
- O status e os valores lidos são exibidos em tempo real no **Monitor Serial**.

## 🛠️ Hardware Necessário
| Componente | Quantidade |
| :--- | :---: |
| Placa ESP32 (DevKit V1) | 1 |
| Sensor de Umidade de Solo (Analógico) | 1 |
| LED Vermelho | 1 |
| Resistor (220Ω ou 330Ω) | 1 |
| Protoboard | 1 |
| Jumpers (Macho-Macho e Macho-Fêmea) | 12 |

## 🔌 Esquema de Ligação
As conexões foram configuradas seguindo a lógica do código fonte:

1. **Sensor de Umidade:**
   - **VCC:** Conectado ao pino **3.3V** do ESP32.
   - **GND:** Conectado ao pino **GND**.
   - **AO (Saída Analógica):** Conectado à **GPIO 34**.
2. **LED de Alerta:**
   - **Anodo (perna longa):** Conectado à **GPIO 4** (com resistor em série).
   - **Catodo (perna curta):** Conectado ao **GND**.

## 💻 Configuração do Ambiente
1. Instale a **Arduino IDE**.
2. No menu `Preferências`, adicione a URL: 
   `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
3. Instale o pacote **esp32** no `Gerenciador de Placas`.
4. Selecione o modelo correto da sua placa em `Ferramentas > Placa`.

## 📜 Código Fonte
```cpp
/*
 * Projeto: Monitor de Umidade de Solo
 * Autores: Vitor Miguel, Kauan, Davi
 */

const int PINO_SENSOR_UMIDADE = 34; // Entrada analógica
const int PINO_LED_SECO = 4;        // Saída digital para o LED

const int LIMIAR_SECO = 2000; // Valor de referência para solo seco

void setup() {
  Serial.begin(115200);
  pinMode(PINO_LED_SECO, OUTPUT);
}

void loop() {
  // Realiza a leitura analógica (0 - 4095)
  int valor_umidade = analogRead(PINO_SENSOR_UMIDADE);

  Serial.print("Valor do sensor: ");
  Serial.println(valor_umidade);

  // Verifica se o solo está seco
  if (valor_umidade > LIMIAR_SECO) {
    Serial.println("ALERTA: Solo seco! É hora de regar.");
    digitalWrite(PINO_LED_SECO, HIGH);
  } else {
    Serial.println("Solo úmido. Tudo certo!");
    digitalWrite(PINO_LED_SECO, LOW);
  }

  delay(1000); // Intervalo de 1 segundo entre leituras
}
