## Descripción

Este repositorio contiene ejemplos de código y esquemáticos para trabajar con microcontroladores PIC16F877A y PIC18F4550. Encontrarás la configuración inicial, el esquemático del circuito y el código fuente necesario para comenzar a programar estos microcontroladores.

---
# Parte 1: Microcontroladores PIC18F4550 - LED y Pulsador
En la parte 1 realizaremos un ejemplo práctico de la simulación del encendido de un LED utilizando el PIC18F4550 y un pulsador.

---
## 📸 Esquemático - Diagrama de Conexión en Proteus (Parte 1)

![Diagrama de conexión P1_1](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/54ecaef033b274b7b9815eee68acd3f49e2a8aa6/Diagrama%20de%20conexi%C3%B3n%20P1_1.png?raw=true)

## 📸 Simulación en Ejecución (Parte 1)

![Diagrama de conexión P1_1 ON](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/8027131575de4235fe233ec2361e82dea26b90e7/Diagrama%20de%20conexi%C3%B3n%20P1_1%20ON.png?raw=true)

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

## 🔧 Especificaciones Técnicas (Parte 1)

| Especificación | Detalle |
|---|---|
| **Microcontrolador** | PIC18F4550 |
| **Voltaje de operación** | 5V |
| **Oscilador** | 8 MHz |
| **Puerto utilizado** | PORTB (RB0 y RB1) |

---

## 🚀 Cómo Usar (Parte 1)

1. Revisa el esquemático y realiza las conexiones del circuito
2. Copia el código fuente en MPLAB X IDE
3. Compila el proyecto con el compilador XC8
4. Ubica el archivo .hex y carga el código a tu microcontrolador en proteus
5. Inicia la simulación del circuito
6. Prueba el encendido del LED al presionar el pulsador

---

## 📌 Funcionalidad (Parte 1)

- **RB0**: Entrada digital (Pulsador)
- **RB1**: Salida digital (LED)
- El LED se enciende cuando se presiona el pulsador y se apaga cuando se suelta

---

# Parte 2_1: Microcontroladores PIC16F877A - Contador Multiplexado en 4 Displays de 7 Segmentos

En la parte 2_1 realizaremos un contador de 0 a 9999 utilizando el PIC16F877A con 4 displays de 7 segmentos multiplexados y decodificadores BCD (Código Binario Decimal).

---

## 📸 Esquemático - Diagrama de Conexión en el Módulo de Microcontroladores (Parte 2_1)
![Diagrama de conexión P2_1](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/2168f4156d3d9d463da531c059c56398263faf54/Diagrama%20de%20conexi%C3%B3n%20P2_1.png?raw=true)

![Diagrama de conexión P2_1](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/36f915ee0828095de10cacc9988829b669b20634/Diagrama%20de%20conexi%C3%B3n%20P2_1.png?raw=true)

https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/2168f4156d3d9d463da531c059c56398263faf54/Diagrama%20de%20conexi%C3%B3n%20P2_1.png
---

## 💻 Código Fuente en MPLAB X IDE - Parte 2_1

```c
#include <xc.h> //Librería del compilador XC8

// ================= CONFIGURACIÓN =================
#pragma config FOSC = HS       // Oscilador externo de alta velocidad
#pragma config WDTE = OFF      // Watchdog desactivado
#pragma config PWRTE = ON      // Power-up timer activado
#pragma config BOREN = OFF     // Brown-out reset menor a 4V desactivado
#pragma config LVP = OFF       // Programación en bajo voltaje desactivada
#pragma config CPD = OFF       // Protección de datos desactivada
#pragma config WRT = OFF       // Escritura protegida desactivada
#pragma config CP = OFF        // Protección de código desactivada

#define _XTAL_FREQ 8000000     // Frecuencia de 8 MHz

// ================= FUNCIÓN BCD =================
void enviar_bcd(unsigned char num){ // Convierte número a binario
    // Enviar cada bit del número a RB0?RB3
    PORTBbits.RB0 = (num >> 0) & 1; // bit 0
    PORTBbits.RB1 = (num >> 1) & 1; // bit 1
    PORTBbits.RB2 = (num >> 2) & 1; // bit 2
    PORTBbits.RB3 = (num >> 3) & 1; // bit 3
} 

// ================= MOSTRAR NÚMERO =================
void mostrar_numero(int numero){ //Recibe, separa y muestra datos

    unsigned char d1, d2, d3, d4;

    // Separar el número en dígitos
    d1 = numero % 10;           // unidades
    d2 = (numero / 10) % 10;    // decenas
    d3 = (numero / 100) % 10;   // centenas
    d4 = (numero / 1000) % 10;  // millares

    // Multiplexado: encender un display a la vez

    PORTD = 0x01;   // Activar display 1 (millares)
    enviar_bcd(d4); // Enviar número
    __delay_ms(1);  // Pequeño retardo

    PORTD = 0x02;   // Activar display 2 (centenas)
    enviar_bcd(d3);
    __delay_ms(1);

    PORTD = 0x04;   // Activar display 3 (decenas)
    enviar_bcd(d2);
    __delay_ms(1);

    PORTD = 0x08;   // Activar display 4 (unidades)
    enviar_bcd(d1);
    __delay_ms(1);
}

// ================= PROGRAMA PRINCIPAL =================
void main(){
    int contador = 0;
    int i;

    TRISB = 0x00; // PORTB como salida (BCD)
    TRISD = 0x00; // PORTD como salida (control displays)
    
    PORTB = 0x00; //Apaga antes de iniciar todo
    PORTD = 0x00;

    while(1){
        // Refrescar varias veces para evitar parpadeo
        for(i = 0; i < 100; i++){
            mostrar_numero(contador);
        }
        contador++; // Incrementar número
        // Reiniciar si llega a 9999
        if(contador > 9999){
            contador = 0;
        }
    }
}
```

---

## 🔧 Especificaciones Técnicas (Parte 2_1)

| Especificación | Detalle |
|---|---|
| **Microcontrolador** | PIC16F877A |
| **Voltaje de operación** | 5V |
| **Oscilador** | 8 MHz |
| **Puertos utilizados** | PORTB (RB0-RB3) y PORTD (RD0-RD3) |
| **Decodificadores** | CD4511 BCD a 7 Segmentos |
| **Displays** | 4 Displays de 7 Segmentos multiplexados |
| **Rango de conteo** | 0 a 9999 |

---

## 🚀 Cómo Usar (Parte 2_1)

1. Revisa el esquemático y realiza las conexiones del circuito
2. Conecta el módulo de 4 displays de 7 segmentos según el diagrama
3. Copia el código fuente en MPLAB X IDE
4. Compila el proyecto con el compilador XC8
5. Ubica el archivo .hex y carga el código a tu microcontrolador con MPLAB IPE
6. Realiza el reseteo por software en MPLAB IPE
7. Observa cómo el contador cuenta de 0 a 9999 en el módulo display de 4 digitos

---

## 📌 Funcionalidad (Parte 2_1)

- **PORTB (RB0-RB3)**: Salidas digitales (Líneas BCD: A, B, C, D) para los decodificadores
- **PORTD (RD0-RD3)**: Salidas digitales (Control de habilitación de cada display)
- El microcontrolador genera un contador que incrementa cada segundo
- Utiliza multiplexado para activar un display a la vez, evitando parpadeo
- El contador separa el número en 4 dígitos (unidades, decenas, centenas, millares)
- Cada decodificador BCD a 7 segmentos convierte los datos a los segmentos del display
- El contador reinicia a 0 después de llegar a 9999

---

# Parte 2_2: Microcontroladores PIC16F877A - Contador BCD en Display de 7 Segmentos

En la parte 2_2 realizaremos un contador de 0 a 9 utilizando el PIC16F877A y mostraremos el resultado en un decodificador BCD a 7 segmentos.

---

## 📸 Esquemático - Diagrama de Conexión en Proteus (Parte 2_2)

![Diagrama de conexión P2_2](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/69628c927790dd4c2cac06aceac46918d26ca12f/Diagrama%20de%20conexi%C3%B3n%20P2_2.png?raw=true)

## 📸 Simulación en Ejecución (Parte 2_2)

![Diagrama de conexión P2_2 ON](https://github.com/ppabloparedes/Microcontroladores-PIC16F877A/blob/69628c927790dd4c2cac06aceac46918d26ca12f/Diagrama%20de%20conexi%C3%B3n%20P2_2%20ON.png?raw=true)

---

## 💻 Código Fuente en MPLAB X IDE - Parte 2_2

```c
#include <xc.h>

// CONFIGURACIÓN
#pragma config FOSC = HS
#pragma config WDTE = OFF
#pragma config PWRTE = ON
#pragma config BOREN = OFF
#pragma config LVP = OFF
#pragma config CPD = OFF
#pragma config WRT = OFF
#pragma config CP = OFF

#define _XTAL_FREQ 8000000

void enviar_bcd(unsigned char num){
    
    PORTBbits.RB0 = (num >> 0) & 1; // A
    PORTBbits.RB1 = (num >> 1) & 1; // B
    PORTBbits.RB2 = (num >> 2) & 1; // C
    PORTBbits.RB3 = (num >> 3) & 1; // D
}

void main(){

    unsigned char contador = 0;

    TRISB = 0x00; // RB0-RB3 como salida
    PORTB = 0x00;

    while(1){

        enviar_bcd(contador);  // Mostrar número
        __delay_ms(1000);      // Esperar 1 segundo

        contador++;

        if(contador > 9){
            contador = 0;
        }
    }
}
```

---

## 🔧 Especificaciones Técnicas (Parte 2_2)

| Especificación | Detalle |
|---|---|
| **Microcontrolador** | PIC16F877A |
| **Voltaje de operación** | 5V |
| **Oscilador** | 8 MHz |
| **Puertos utilizados** | PORTB (RB0-RB3) |
| **Decodificador** | BCD a 7 Segmentos |
| **Rango de conteo** | 0 a 9 |

---

## 🚀 Cómo Usar (Parte 2_2)

1. Revisa el esquemático y realiza las conexiones del circuito
2. Copia el código fuente en MPLAB X IDE
3. Compila el proyecto con el compilador XC8
4. Ubica el archivo .hex y carga el código a tu microcontrolador en Proteus
5. Inicia la simulación del circuito
6. Observa cómo el contador cuenta de 0 a 9 cada segundo en el display de 7 segmentos

---

## 📌 Funcionalidad (Parte 2_2)

- **RB0-RB3**: Salidas digitales (Líneas BCD: A, B, C, D)
- El microcontrolador genera un contador que incrementa cada segundo
- El contador envía los datos en formato BCD (Binary Coded Decimal)
- El decodificador BCD a 7 segmentos convierte los datos BCD a los segmentos del display
- El contador reinicia a 0 después de llegar a 9

---


