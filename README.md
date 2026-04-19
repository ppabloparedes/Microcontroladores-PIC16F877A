# Microcontroladores PIC18F4550 - Parte 1

## Descripción

Este repositorio contiene ejemplos de código y esquemáticos para trabajar con microcontroladores PIC16F877A y PIC18F4550. En esta **Parte 1** encontrarás la configuración inicial, el esquemático del circuito y el código fuente necesario para comenzar a programar estos microcontroladores.

---

## 📸 Esquemático - Diagrama de Conexión

![Diagrama de conexión P1_1](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/54ecaef033b274b7b9815eee68acd3f49e2a8aa6/Diagrama%20de%20conexi%C3%B3n%20P1_1.png?raw=true)

---

## 💻 Código Fuente - Parte 1

```c
#include <xc.h>

// CONFIGURACIÓN
#pragma config FOSC = HS
#pragma config WDT = OFF
#pragma config LVP = OFF
#pragma config PBADEN = OFF

#define _XTAL_FREQ 8000000  // 8 MHz

void main(void) {
    
    // Configurar puertos
    TRISBbits.TRISB0 = 1; // RB0 como entrada (pulsador)
    TRISBbits.TRISB1 = 0; // RB1 como salida (LED)
    
    LATBbits.LATB1 = 0; // LED apagado inicialmente

    while(1) {
        if(PORTBbits.RB0 == 1) {  // Si se presiona el pulsador
            LATBbits.LATB1 = 1;  // Encender LED
        } else {
            LATBbits.LATB1 = 0;  // Apagar LED
        }
    }
}
