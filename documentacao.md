# Semáforo com Arduino

## Descrição do Projeto
Este projeto tem como objetivo criar um **sistema de semáforo automatizado** utilizando **Arduino UNO**, **LEDs** e **resistores**, simulando o funcionamento de um cruzamento real.  
O sistema alterna automaticamente entre as três fases do semáforo, garantindo a segurança de pedestres e veículos:

- 🔴 **Vermelho:** 6 segundos  
- 🟢 **Verde:** 4 segundos  
- 🟡 **Amarelo:** 2 segundos  

Além disso, **foi implementado um recurso extra opcional**: um **display LCD I2C 16x2** que exibe dinamicamente o **tempo restante de cada fase**.  


---

## Objetivos Educacionais
- Aplicar conceitos de **eletrônica básica e controle de sinais luminosos**.  
- Entender a **lógica sequencial e a temporização de eventos**.  
- Desenvolver habilidades em **programação estruturada para Arduino (C/C++)**.  
- Integrar **hardware e software** em um protótipo funcional.  
- (Extra) Demonstrar o uso de **dispositivos I2C e exibição de dados em tempo real**.

---

## Componentes Utilizados

| Componente | Quantidade | Especificação / Observação |
|-------------|-------------|-----------------------------|
| LED vermelho | 1 | Representa o sinal de **Pare** |
| LED amarelo | 1 | Representa o sinal de **Atenção** |
| LED verde | 1 | Representa o sinal de **Siga** |
| Resistores | 3 | 220 Ω — um para cada LED |
| Display I2C 16x2 | 1 | **Extra:** para mostrar contagem regressiva |
| Protoboard | 1 | Padrão 830 pontos |
| Fios jumper | 5 | Macho–macho |
| Fios jumper | 4 | Macho–fêmea |
| Arduino UNO | 1 | Microcontrolador principal |
| Cabo USB | 1 | Alimentação e upload do código |

---

## Esquema de Ligação

### 🔌 Conexões dos LEDs
| LED | Pino Arduino | Ligação |
|------|----------------|----------|
| Vermelho | + resistor → 8 | GND |
| Amarelo | + resistor → 9 | GND |
| Verde | + resistor → 10 | GND |

### Conexões do Display LCD I2C (extra)
| Pino do LCD | Pino do Arduino | Função |
|--------------|----------------|----------|
| VCC | 5V | Alimentação |
| GND | GND | Terra |
| SDA | A4 | Comunicação de dados |
| SCL | A5 | Clock do barramento I2C |

---

## Código Fonte (Arduino IDE)

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// --- Definições dos pinos dos LEDs ---
#define LED_RED 8
#define LED_YELLOW 9
#define LED_GREEN 10

// --- Configuração do display I2C ---
// Endereço padrão 0x27 (alterar para 0x3F se necessário)
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  // Configuração dos LEDs
  pinMode(LED_RED, OUTPUT);
  pinMode(LED_YELLOW, OUTPUT);
  pinMode(LED_GREEN, OUTPUT);

  // Inicialização do display LCD
  lcd.init();
  lcd.backlight();
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("  SEMAFORO V1.0");
  lcd.setCursor(0, 1);
  lcd.print("Iniciando...");
  delay(2000);
  lcd.clear();
}

void loop() {
  // --- Fase Vermelha (6s) ---
  digitalWrite(LED_RED, HIGH);
  digitalWrite(LED_YELLOW, LOW);
  digitalWrite(LED_GREEN, LOW);
  mostrarContagem("Vermelho", 6);

  // --- Fase Verde (4s) ---
  digitalWrite(LED_RED, LOW);
  digitalWrite(LED_YELLOW, LOW);
  digitalWrite(LED_GREEN, HIGH);
  mostrarContagem("Verde", 4);

  // --- Fase Amarela (2s) ---
  digitalWrite(LED_RED, LOW);
  digitalWrite(LED_YELLOW, HIGH);
  digitalWrite(LED_GREEN, LOW);
  mostrarContagem("Amarelo", 2);
}

// --- Função auxiliar: exibe contagem no LCD ---
void mostrarContagem(String cor, int tempo) {
  for (int i = tempo; i > 0; i--) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Sinal: " + cor);
    lcd.setCursor(0, 1);
    lcd.print("Tempo: " + String(i) + "s");
    delay(1000);
  }
}
```

## Tutorial de Montagem

### 1° Passo
<div align="center">
<sub>Figura 1 - Requisitos iniciais</sub>
<br>
<img src="assets/figura1.jpeg" alt='figura 1' width="70%">
<br>
</div>

<div align="center">
<sub>Figura 2 - Primeiras ligações</sub>
<br>
<img src="assets/figura2.png" alt='figura 2' width="70%">
<br>
</div>

<div align="center">
<sub>Figura 3 - Implantação dos LEDs</sub>
<br>
<img src="assets/figura3.png" alt='figura 3' width="70%">
<br>
</div>

<div align="center">
<sub>Figura 4 - Implantação dos resistores</sub>
<br>
<img src="assets/figura4.png" alt='figura 4' width="70%">
<br>
</div>

<div align="center">
<sub>Figura 5 - Implantação dos jumpers</sub>
<br>
<img src="assets/figura5.png" alt='figura 5' width="70%">
<br>
</div>

<div align="center">
<sub>Figura 6 - Extra: Display</sub>
<br>
<img src="assets/figura6.png" alt='figura 6' width="70%">
<br>
</div>

[Vídeo do Funcionamento](https://drive.google.com/file/d/1In0ACiZk_tQ2dKcayyV4njXACwC2q1oC/view?usp=sharing)
