# Laboratorio 2: Programación Robot SCARA T3 - Robot EPSON T6
Nombre: Fernando Cardenas Acosta & Raúl Ignacio Marín Medina

## Descripción del problema: 
Se debe programar los robots SCARA T3 y EPSON T6 por medio del software EPSON RC + 7.0 para realizar movimiento de paletizado en Z, s (paletizado por default) y exterior (alrededor de una matriz 3x3). Esta práctica busca visualizar los sconceptos de instalación física y eléctrica del robot (requerimientos para encender, recomendaciones de uso, espacio del controlador dentro del robot y paradas de emergencia), de igual manera, utilizar y editar el Robot Manager para usar comandos Go, Move, Jump, Pallet y Pallet Outside dentro de las rutinas de movimiento. 

![image](https://github.com/fcardenasa/Robotica-Lab-2/assets/124843458/64eaa355-e2cb-42ae-b290-5bc2ba5fb59a)
![image](https://github.com/fcardenasa/Robotica-Lab-2/assets/124843458/0c5d5f1b-2120-4576-9d47-e58d5402e995)

## Solución:

A continuación, se presenta los pasos realizados para programar la rutina de paletizado en el robot EPSON T6.
* Abrir el software EPSON RC+7.0 para crear una conexión virtual con el robot EPSON T6/SCARA T6 en la pestaña "Configuración del sistema".
* Se debe donfigurar una posición de Home con el Robot Manager, para este caso, la posición de home será uno de los puntos que generan el plano donde el paletizado se realiza.
* Mediante la herramienta JOG&TEACH se crean tres puntos, los cuales son necesarios para definir el plano del paletizado, estos puntos se llama Origen, Ejex y Ejey en código.
* Se crea la función paletizado_z, la cual requiere como input los 3 puntos del plano y el tamaño de la matriz para paletizar, en este caso 3x3 (Origen,Ejex,Ejey,3,3).
* Se recorren las posiciones del pallet mediante un ciclo FOR que va desde la posición 1 hasta la 9 con el comando Jump/Go
* Se crea la función paletizado_s, la cual recorre la matriz como 1,2,3,6,5,4,7,8,9. Se utiliza el mismo comando Jump/Go, sin embargo, el ciclo FOR se recorre desde 1:3:1, 6:4:-1 y 7:9:1.
* Se crea la función paletizado_externo, la cual recorre una matriz 3x3 de manera externa, es decir, recorre desde afuera hacia los elementos interiores añadiendo una fila y columna, por lo tanto, se realiza un FOR anidado, desde i=1:4 para filas y j=1:4 para columnas, para pasar de un estado a otro se usa el comando next.

## Videos de demostración: 

Demostración paletizado en Z y S.
https://youtu.be/Mm6FZLsKNC0

Demostración paletizado en Z, S y externo.
https://youtu.be/Mm6FZLsKNC0

## Código:

	Function main
	
	Motor On
	Power High
	Speed 50
	Accel 50, 50
	Do
		Call paletizado_z
		Call paletizado_s
	
		If MemSw(512) Then
			Call paletizado_externo
		EndIf
	Loop
	
	Fend
	Function paletizado_s
	
	Integer i
	
	#define estado_paletizado_s 12
	
	Pallet 1, Origen, EjeX, EjeY, 3, 3
	
	On estado_paletizado_s
	
	For i = 1 To 3
		Go Pallet(1, i) :Z(837)
		Go Pallet(1, i)
		Go Pallet(1, i) :Z(837)
	Next
	
	For i = 6 To 4 Step -1
		Go Pallet(1, i) :Z(837)
		Go Pallet(1, i)
		Go Pallet(1, i) :Z(837)
	Next
	
	For i = 7 To 9
		Go Pallet(1, i) :Z(837)
		Go Pallet(1, i)
		Go Pallet(1, i) :Z(837)
	Next
	
	Off estado_paletizado_s
	
	Fend
	Function paletizado_z
	
	Integer i
	#define estado_paletizado_z 11
	
	Pallet 1, Origen, EjeX, EjeY, 3, 3
	
	On estado_paletizado_z
	
	For i = 1 To 9
		Go Pallet(1, i) :Z(837)
		Go Pallet(1, i)
		Go Pallet(1, i) :Z(837)
	Next
	
	Off estado_paletizado_z
	
	Fend
	Function paletizado_externo
	
	Integer i
	
	#define estado_paletizado_externo 12
	Pallet Outside, 2, Origen, EjeX, EjeY, 3, 3
	
	On estado_paletizado_externo
	
	For i = 1 To 4
		Go Pallet(2, i) :Z(837)
		Go Pallet(2, i)
		Go Pallet(2, i) :Z(837)
	Next
	
	Off estado_paletizado_externo
	
	Fend
