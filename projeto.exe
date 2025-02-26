#include <stdio.h>
#include <string.h> // Para usar snprintf
#include "pico/stdlib.h"
#include "hardware/adc.h"
#include "hardware/i2c.h"
#include "inc/ssd1306.h" // Sua biblioteca SSD1306

// Definições de pinos e constantes
#define I2C_BUS i2c1
#define I2C_DATA_PIN 14
#define I2C_CLOCK_PIN 15
#define OLED_ADDRESS 0x3C
#define RED_LED_PIN 13
#define GREEN_LED_PIN 11
#define BLUE_LED_PIN 12
#define BUZZER_PIN 10

// Variáveis globais
ssd1306_t oled_display;

// Função para configurar o GPIO
void setup_gpio() {
    gpio_init(RED_LED_PIN);
    gpio_init(GREEN_LED_PIN);
    gpio_init(BLUE_LED_PIN);
    gpio_init(BUZZER_PIN);
    gpio_set_dir(RED_LED_PIN, GPIO_OUT);
    gpio_set_dir(GREEN_LED_PIN, GPIO_OUT);
    gpio_set_dir(BLUE_LED_PIN, GPIO_OUT);
    gpio_set_dir(BUZZER_PIN, GPIO_OUT);
}

// Função para configurar o I2C
void setup_i2c() {
    i2c_init(I2C_BUS, 400000); // Inicializa I2C com frequência de 400 kHz
    gpio_set_function(I2C_DATA_PIN, GPIO_FUNC_I2C);
    gpio_set_function(I2C_CLOCK_PIN, GPIO_FUNC_I2C);
    gpio_pull_up(I2C_DATA_PIN);
    gpio_pull_up(I2C_CLOCK_PIN);
}

// Função para atualizar o display SSD1306
void update_display(uint16_t soil_moisture, uint16_t light_level, const char* status) {
    char buffer[50]; // Buffer para armazenar as strings

    // Limpa o display (preenche com 0)
    ssd1306_fill(&oled_display, 0);

    // Exibe a umidade
    snprintf(buffer, sizeof(buffer), "UMIDADE: %d", soil_moisture);
    ssd1306_draw_string(&oled_display, buffer, 0, 0);

    // Exibe a luminosidade
    snprintf(buffer, sizeof(buffer), "LUZ: %d", light_level);
    ssd1306_draw_string(&oled_display, buffer, 0, 16);

    // Exibe o status
    snprintf(buffer, sizeof(buffer), "STATUS: %s", status);
    ssd1306_draw_string(&oled_display, buffer, 0, 32);

    // Atualiza o display
    ssd1306_send_data(&oled_display);
}

// Função para gerar um som de campainha alto
void buzzer_alert() {
    for (int i = 0; i < 5; i++) { // Repete o som 5 vezes
        for (int j = 0; j < 50; j++) { // Gera um tom agudo
            gpio_put(BUZZER_PIN, 1); // Liga o buzzer
            sleep_us(500); // Mantém ligado por 500 microssegundos
            gpio_put(BUZZER_PIN, 0); // Desliga o buzzer
            sleep_us(500); // Mantém desligado por 500 microssegundos
        }
        sleep_ms(200); // Pausa entre os tons
    }
}

int main() {
    stdio_init_all();
    setup_gpio();
    setup_i2c();

    // Inicializa o display SSD1306
    ssd1306_init(&oled_display, WIDTH, HEIGHT, false, OLED_ADDRESS, I2C_BUS);
    ssd1306_config(&oled_display); // Configura o display
    ssd1306_fill(&oled_display, 0); // Limpa o display

    while (true) {
        // Gera valores aleatórios para umidade e luminosidade
        uint16_t simulated_soil_moisture = rand() % 4096; // Valor entre 0 e 4095 (12 bits ADC)
        uint16_t simulated_light_level = rand() % 4096;   // Valor entre 0 e 4095

        // Exibe os valores no console (para depuração)
        printf("UMIDADE SIMULADA: %d, LUMINOSIDADE SIMULADA: %d\n", simulated_soil_moisture, simulated_light_level);

        // Lógica de feedback visual com LEDs
        const char* status;
        if (simulated_soil_moisture < 1365) { // Solo seco (valor simulado baixo)
            gpio_put(RED_LED_PIN, 1);
            gpio_put(BLUE_LED_PIN, 0);
            gpio_put(GREEN_LED_PIN, 0);
            status = "SECO";
            buzzer_alert(); // Ativa o buzzer com som de campainha
        } else if (simulated_soil_moisture >= 1365 && simulated_soil_moisture < 2730) { // Atenção (valor simulado médio)
            gpio_put(RED_LED_PIN, 0);
            gpio_put(BLUE_LED_PIN, 1);
            gpio_put(GREEN_LED_PIN, 0);
            status = "ATENCAO";
        } else { // Condições ideais (valor simulado alto)
            gpio_put(RED_LED_PIN, 0);
            gpio_put(BLUE_LED_PIN, 0);
            gpio_put(GREEN_LED_PIN, 1);
            status = "IDEAL";
        }

        // Atualiza o display com os valores e o status
        update_display(simulated_soil_moisture, simulated_light_level, status);

        // Pausa de 5 segundos antes da próxima leitura
        sleep_ms(8000);
    }
}