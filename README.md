Este proyecto es la implementación en Java de un sistema para gestionar exámenes, estudiantes y detectar posibles casos de copia en un aula. 
Fue desarrollado como parte de la materia Algorítmos y Estructura de Datos II (2do cuatrimestre de 2025) en la Universidad de Buenos Aires (UBA). 
El sistema modela un aula con una distribución específica de asientos, donde los estudiantes, una vez que tienen su exámen, pueden resolver ejercicios, copiarse de sus vecinos, entregar exámenes y ser evaluados. Las funcionalidades principales incluyten la detección de copia según un criterio porcetual y la corrección de exámenes, generando un ranking de notas finales. 
El objetivo principal fue el implementar un Tipo Abstracto de Datos (TAD) Edr, esperando que se cumpla la complejidad temporal en cada caso, usando abstracción, encapsulamiento y modularidad. 
Integrantes del Equipo: Lucas Akerman, Agustina Campardo y Alejo Mendes. 
Conceptos 
Este proyecto se apoya en los conceptos fundamentales de la materia. 
- TAD (Tipo Abstracto de Datos): Se diseño e implementó el TAD EdR, donde definimos su interfaz (las operaciones) y ocultamos los detalles de su representación interna.
- Estructuras de Datos: implementadas a modo de cumplir con la complejidad
  - Heap (Cola de Prioridad): Se implementó un HeapMin sobre un arreglo para mantener a los estudiantes ordenados por su nota y estado de entrega, permitiendo operaciones O(log E)
  - Handles: Se utilizaron Handles (clase interna HeapMin.Handle) para mantener referencia a cada estudiante dentro de heap. Nos permitía actualizar la posición de un estudiante en O(log E) después de un cambio en su nota o estado de estrega.
- Encapsulamiento: La solución se dividió en varias clases (EdR, Estudiante, Examen, HeapMin, NotaFinal), cada una con su responsabilidad y así garantizar el encapsulamiento.
- Complejidad: Cada operación fue hecha para cumplir con la complejidad especificada en el enunciado (justificada en los comentarios de código).

Funcionalidades 
El sistema EdR implementa las siguientes operaciones: 
1. nuevoEdr(...): Inicializa el sistema. Crea una estructura de estudiantes y los ubica en aula con un espacio de por medio.
2. copiarse(estudiante): Un estudiante se copia de su mejor vecino. El "mejor vecino" es aquel que tiene la mayor cantidad de respuestas que el estudiante no tiene. Solo se copia la primera respuesta faltante. Desempata por ID mayor.
3. resolver(estudiante, nroEjercicio, respuesta): El estudiante resuelva un ejercicio. Su nota se actualiza en O(1) gracias a que mantenemos un contador de respuestas correctas, y si posición en el heap se ordena en O(log E).
4. consultarDarkWeb(k, examenDW): Los k estudientes con peor punatje reemplazan completamente su examen por el examenDW. Desempata por menor ID.
5. notas(): Devuelve una secuencia de notas de todos los estudiantes, ordenada por ID.
6. entregar(estudiante): Marca el examen del estudiante como entregado. Esto afecto su priotidad en el heap (los que entregaron tienen prioridad más baja para ser los "peores").
7. chequearCopias(): Detecta qué estudiantes con sospechosos de haberse copiado según el criterio "Un/a estudiante sospechoso de haberse copiado es alguien que tiene cada respuesta de su examen igual a al menos el 25 % de todos los estudiantes". Devuelve una lista ordenada de IDs.
8. corregir(): Devuelve las notas de los exámenes de los estudiantes que no se copiaron, ordenadas de mayor a menor nota. Desempata por mayor ID.

Estructura del proyecto 
src/
├── aed/
│   ├── EdR.java               # Clase principal que implementa el TAD. Contiene el flujo de trabajo.
│   ├── Estudiante.java        # Modela a un estudiante (ID, posición, nota, respuestas, estado).
│   ├── Examen.java            # Modela el examen de un estudiante (arreglo de respuestas).
│   ├── HeapMin.java           # Implementación de una cola de prioridad (heap mínimo) sobre un arreglo.
│   │   └── Handle             # Clase interna que encapsula una referencia a un nodo del heap.
│   └── NotaFinal.java         # Clase auxiliar para el resultado de la corrección (nota, ID).


