	List p=16F84A
	#include <p16f84A.inc>

	; Fusibles: Cristal XT, Watchdog apagado, Power-up Timer encendido, sin protección de código
	__CONFIG _CP_OFF & _WDT_OFF & _PWRTE_ON & _XT_OSC

 	org 00h
 	goto inicio

	org 05h             ; El vector de interrupción está en 04h, el código libre inicia en 05h
inicio
;configuracion de entradas y salidas
	bsf 0x03, 5         ; Cambia al Banco 1 (RP0 = 1)
 	bcf 0x05, 1         ; RA1 como salida (bit 1 de TRISA en 0)
 	bcf 0x05, 3         ;RA3 sera
 	bsf 0x06, 0         ; RB0 como entrada (bit 0 de TRISB en 1)
 	bcf 0x01, 7         ; Habilitar pullups para poder conectar
    
bcf 0x03, 5       ;Regresa al Banco 0

	bcf 0x05, 1         ; Asegura que el LED en RA1 inicie apagado

;-------- Bucle principal --------
Test1
	btfss 0x06, 0       ; Revisa RB0 (PORTB). Si es 1 (presionado), salta la siguiente línea
	goto apago          ; Si RB0 es 0, va a apagar el LED
	goto prend        ; Si RB0 es 1, enciende el LED en RA1 (PORTA)
    
    
apago
	bcf 0x05, 1         ; Apaga el LED en RA1
	bsf 0x05, 3          ; apaga el led en RA3 pero inverso
	goto Test1		 ; 

	End