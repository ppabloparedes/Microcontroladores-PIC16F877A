## Descripción

Este repositorio contiene ejemplos de código y esquemáticos para trabajar con microcontroladores PIC16F877A y PIC18F4550. Encontrarás la configuración inicial, el esquemático del circuito y el código fuente necesario para comenzar a programar estos microcontroladores.

---
# Microcontroladores PIC18F4550 - Parte 1
En la parte 1 realizaremos un ejemplo práctico de la simulación del encendido de un LED utilizando el PIC18F4550 y un pulsador.

---
## 📸 Esquemático - Diagrama de Conexión en Proteus

![Diagrama de conexión P1_1](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/54ecaef033b274b7b9815eee68acd3f49e2a8aa6/Diagrama%20de%20conexi%C3%B3n%20P1_1.png?raw=true)

---

## 💻 Código Fuente en MPLAB X IDE - Parte 1

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
```

---

## 🔧 Especificaciones Técnicas

| Especificación | Detalle |
|---|---|
| **Microcontrolador** | PIC18F4550 |
| **Voltaje de operación** | 5V |
| **Oscilador** | 8 MHz |
| **Puerto utilizado** | PORTB (RB0 y RB1) |

---

## 🚀 Cómo Usar

1. Revisa el esquemático y realiza las conexiones del circuito
2. Copia el código fuente en MPLAB X IDE
3. Compila el proyecto con el compilador XC8
4. Ubica el archivo .hex y carga el código a tu microcontrolador en proteus
5. Inicia la simulación del circuito
6. Prueba el encendido del LED al presionar el pulsador

---

## 📌 Funcionalidad

- **RB0**: Entrada digital (Pulsador)
- **RB1**: Salida digital (LED)
- El LED se enciende cuando se presiona el pulsador y se apaga cuando se suelta

---
