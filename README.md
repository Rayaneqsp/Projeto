
Instruções de uso
Alerta e – Sistema de Monitoramento de Solo e Luminosidade

Autora: Rayane Queiroz dos Santos Passos

Descrição

Este programa monitora a umidade do solo e o nível de luminosidade, exibindo os valores em um display OLED SSD1306 e fornecendo alertas visuais (LEDs) e sonoros (buzzer) quando necessário. O código simula os valores da umidade e da luz, mas pode ser adaptado para utilizar sensores reais.

Requisitos

Hardware

Raspberry Pi Pico W

Display OLED SSD1306 (I2C)

LEDs (vermelho, azul e verde)

Buzzer

Fios e resistores apropriados


Conexões

Instalação e Execução

1. Configurar o ambiente de desenvolvimento:

Instale o compilador GCC para ARM (se ainda não tiver).

Utilize o Visual Studio Code com a extensão para Raspberry Pi Pico.



2. Compilar e carregar o código:

Salve o código como main.c.

Compile e gere o arquivo .uf2.

Conecte a Raspberry Pi Pico W no modo de bootloader e transfira o arquivo .uf2.



3. Funcionamento do programa:

O sistema inicia e configura os dispositivos.

Gera valores simulados de umidade e luminosidade.

Exibe os valores no display OLED.

Aciona os LEDs de acordo com a umidade do solo:

Vermelho: Solo seco (alerta sonoro ativado).

Azul: Nível intermediário de umidade.

Verde: Solo ideal.


Repete o processo a cada 8 segundos.




Personalização

Para utilizar sensores reais, substitua os valores simulados por leituras do ADC da Raspberry Pi Pico.

Ajuste os limites de umidade para diferentes tipos de solo.


Conclusão

Este projeto oferece uma base funcional para monitoramento de umidade e luminosidade, podendo ser expandido para aplicações agrícolas reais.

