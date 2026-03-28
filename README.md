# Sistema de Gestión de Exámenes (EdR) - Trabajo Práctico II

## Descripción del Proyecto
Este proyecto es la implementación en Java de un sistema para gestionar exámenes, estudiantes y detectar posibles casos de copia en un aula. Fue desarrollado como parte de la materia **Algoritmos y Estructuras de Datos II** (2do cuatrimestre de 2025) en la Universidad de Buenos Aires (UBA).

El sistema modela un aula con una distribución específica de asientos, donde los estudiantes, una vez que tienen su examen, pueden resolver ejercicios, copiarse de sus vecinos, entregar exámenes y ser evaluados. Las funcionalidades principales incluyen la detección de copia según un criterio porcentual y la corrección de exámenes, generando un ranking de notas finales.

El objetivo principal fue implementar un **Tipo Abstracto de Datos (TAD) EdR**, esperando que se cumpla la complejidad temporal en cada caso, usando abstracción, encapsulamiento y modularidad.

## Integrantes del Equipo

- Lucas Akerman
- Agustina Campardo
- Alejo Martinez Evans

## Conceptos 
Este proyecto utiliza los conceptos vistos durante la cursada de la materia:

- **TAD (Tipo Abstracto de Datos):** Se diseñó e implementó el TAD EdR, donde definimos su interfaz (las operaciones) y ocultamos los detalles de su representación interna.
- **Estructuras de Datos:** Implementadas a modo de cumplir con la complejidad:
  - **Heap (Cola de Prioridad):** Se implementó un `HeapMin` sobre un arreglo para mantener a los estudiantes ordenados por su nota y estado de entrega, permitiendo operaciones `O(log E)`.
  - **Handles:** Se utilizaron Handles (clase interna `HeapMin.Handle`) para mantener referencia a cada estudiante dentro del heap. Nos permitía actualizar la posición de un estudiante en `O(log E)` después de un cambio en su nota o estado de entrega.
- **Encapsulamiento:** La solución se dividió en varias clases (`EdR`, `Estudiante`, `Examen`, `HeapMin`, `NotaFinal`), cada una con su responsabilidad y así garantizar el encapsulamiento.
- **Complejidad:** Cada operación fue hecha para cumplir con la complejidad especificada en el enunciado (justificada en los comentarios de código).


## Funcionalidades

El sistema EdR implementa las siguientes operaciones:

1. **`nuevoEdr(...)`**: Inicializa el sistema. Crea una estructura de estudiantes y los ubica en aula con un espacio de por medio. Complejidad: O(E * R). 

2. **`copiarse(estudiante)`**: Un estudiante se copia de su mejor vecino. El "mejor vecino" es aquel que tiene la mayor cantidad de respuestas que el estudiante no tiene. Solo se copia la primera respuesta faltante. *Desempata por ID mayor.* Complejidad: O(R + log(E)). 

3. **`resolver(estudiante, nroEjercicio, respuesta)`**: El estudiante resuelve un ejercicio. Su nota se actualiza en `O(1)` gracias a que mantenemos un contador de respuestas correctas, y su posición en el heap se ordena en `O(log E)`. Complejidad: O(log(E)).

4. **`consultarDarkWeb(k, examenDW)`**: Los k estudiantes con peor puntaje reemplazan completamente su examen por el `examenDW`. *Desempata por menor ID.* Complejidad: O(k (R + log(E)). 

5. **`notas()`**: Devuelve una secuencia de notas de todos los estudiantes, ordenada por ID. Complejidad: O(E).

6. **`entregar(estudiante)`**: Marca el examen del estudiante como entregado. Esto afecta su prioridad en el heap (los que entregaron tienen prioridad más baja para ser los "peores"). Complejidad: O(log(E)). 

7. **`chequearCopias()`**: Detecta qué estudiantes son sospechosos de haberse copiado según el criterio *"Un/a estudiante sospechoso de haberse copiado es alguien que tiene cada respuesta de su examen igual a al menos el 25 % de todos los estudiantes"*. Devuelve una lista ordenada de IDs. Complejidad: O(E ∗ R). 

8. **`corregir()`**: Devuelve las notas de los exámenes de los estudiantes que **no** se copiaron, ordenadas de mayor a menor nota. *Desempata por mayor ID.* Complejidad: O(E ∗ log(E)). 

## Estructura del proyecto 
```text
src
└── aed
    ├── EdR.java              # Clase principal que implementa el TAD. Aquí están implementadas las operación del enunciado. 
    ├── Estudiante.java       # Clase Estudiante
    ├── Examen.java           # Clase Examen
    ├── HeapMin.java          # Clase HeapMin
    │   └── Handle            # Clase interna que encapsula una referencia a un nodo del heap
    └── NotaFinal.java        # Clase para el resultado de la corrección (aportada por la cátedra)
```
