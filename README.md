# Robotica-Lab-2
## bxsedigcedog2code
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
