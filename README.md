# Microcontroladores PIC16F877A

Este repositorio contiene proyectos y ejemplos prácticos para el uso de microcontroladores PIC16F877A.

## Diagrama del Circuito

![Diagrama del Circuito](circuit_diagram.png)

## Código del Proyectos

### Código PIC18F4550 Parte 1

```c
// Aquí se encuentra el código para el PIC18F4550
#include <xc.h>

void main() {
    // Configuración inicial
    TRISB = 0x00; // Puerto B como salida
    while(1) {
        PORTB = 0xFF; // Enciende todos los pines del puerto B
        __delay_ms(500);
        PORTB = 0x00; // Apaga todos los pines del puerto B
        __delay_ms(500);
    }
}
