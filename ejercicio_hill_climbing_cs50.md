# Ejercicio: Optimización de Ubicación de Ambulancias con Hill Climbing

## Contexto

Una ciudad está representada por una cuadrícula de tamaño `10 x 10`. En ella existen varias colonias (casas) distribuidas en diferentes posiciones. El gobierno desea colocar **3 ambulancias** de forma que la distancia total entre cada colonia y la ambulancia más cercana sea mínima.

Para resolver el problema utilizarás el algoritmo de **Hill Climbing** descrito en la clase de CS50 sobre búsqueda local y optimización.

---

# Resumen teórico (CS50 AI)

## Optimización

Los problemas de optimización buscan encontrar la mejor solución posible entre un conjunto de soluciones candidatas.

Ejemplos:

- Ubicación de hospitales.
- Ubicación de estaciones de ambulancias.
- Problema del viajante (Traveling Salesman Problem).
- Planeación de producción.
- Asignación de horarios.

Generalmente se define:

- Una función objetivo (maximizar).
- Una función de costo (minimizar).

## Búsqueda Local

A diferencia de algoritmos como:

- Breadth First Search (BFS)
- Depth First Search (DFS)
- A*

La búsqueda local mantiene únicamente un estado actual.

No interesa el camino para llegar a la solución.

Solo interesa la solución final.

## Conceptos fundamentales

### Estado

Representa una configuración completa del problema.

Ejemplo:

```python
ambulances = [(0,0), (3,9), (9,0)]
```

### Vecino

Un estado vecino es una pequeña modificación del estado actual.

Ejemplo:

Mover una ambulancia:

```python
(5,5)
```

a:

```python
(5,6)
```

### Función de costo

Permite evaluar qué tan buena es una solución.

Mientras menor sea el costo, mejor será la solución.

### Distancia Manhattan

```text
|x1 - x2| + |y1 - y2|
```

Ejemplo:

```python
(1,1) -> (4,3)
```

Costo:

```text
|1-4| + |1-3| = 5
```

---

# Hill Climbing

## Idea general

1. Comenzar con una solución inicial.
2. Generar todos los vecinos.
3. Seleccionar el mejor vecino.
4. Si mejora la solución actual, moverse allí.
5. Repetir.
6. Detenerse cuando no exista mejora.

### Pseudocódigo

```text
current = estado_inicial

while True:

    neighbor = mejor_vecino(current)

    if costo(neighbor) >= costo(current):
        return current

    current = neighbor
```

---

# Limitaciones de Hill Climbing

## Mínimo local

Puede encontrar una solución mejor que sus vecinos inmediatos pero que no sea la mejor solución global.

```text
Global Minimum
      *

   *
 Local Minimum
```

## Plateau

Región plana donde muchos estados tienen exactamente el mismo costo.

## Shoulder

Región plana que sí tiene una mejora posible, pero difícil de encontrar.

---

# Variantes vistas en CS50

## Steepest-Ascent Hill Climbing

Selecciona siempre el mejor vecino disponible.

## Stochastic Hill Climbing

Selecciona aleatoriamente uno de los vecinos mejores.

## First-Choice Hill Climbing

Selecciona el primer vecino que mejora la situación.

## Random-Restart Hill Climbing

Ejecuta Hill Climbing múltiples veces desde estados iniciales diferentes.

## Local Beam Search

Mantiene varios estados simultáneamente.

---

# Simulated Annealing

Permite realizar movimientos que empeoran temporalmente la solución.

Objetivo:

Evitar quedarse atrapado en mínimos locales.

### Idea

Al inicio:

- Alta temperatura.
- Mucha exploración.

Al final:

- Baja temperatura.
- Más explotación.

### Pseudocódigo

```text
current = estado_inicial

for t in tiempo:

    T = temperatura(t)

    vecino = vecino_aleatorio()

    ΔE = calidad(vecino) - calidad(actual)

    if ΔE > 0:
        aceptar
    else:
        aceptar con probabilidad e^(ΔE/T)
```

---

# Ejercicio práctico

## Objetivos de aprendizaje

- Modelar un problema como espacio de estados.
- Implementar una función de costo.
- Generar estados vecinos.
- Aplicar Hill Climbing.
- Comprender mínimos locales.

---

## Datos iniciales

```python
houses = [
    (1, 1),
    (2, 7),
    (4, 4),
    (5, 8),
    (7, 2),
    (8, 6),
    (9, 9)
]
```

```python
ambulances = [
    (0, 0),
    (3, 9),
    (9, 0)
]
```

---

## Parte 1: Distancia Manhattan

Implementa:

```python
def distance(p1, p2):
    pass
```

Ejemplo:

```python
distance((1,1), (4,3))
```

Resultado:

```python
5
```

---

## Parte 2: Función de costo

Implementa:

```python
def cost(houses, ambulances):
    pass
```

La función debe:

1. Encontrar la ambulancia más cercana.
2. Calcular la distancia Manhattan.
3. Sumar las distancias.

---

## Parte 3: Generación de vecinos

Implementa:

```python
def get_neighbors(ambulances):
    pass
```

Un vecino consiste en mover una ambulancia:

- Arriba
- Abajo
- Izquierda
- Derecha

---

## Parte 4: Hill Climbing

Implementa:

```python
def hill_climbing(houses, ambulances):
    pass
```

Algoritmo:

```text
1. Calcular costo actual.
2. Generar vecinos.
3. Escoger el vecino con menor costo.
4. Si no mejora:
      terminar.
5. Si mejora:
      movernos.
6. Repetir.
```

---

## Parte 5: Mostrar evolución

Ejemplo:

```text
Paso 0: costo = 34
Paso 1: costo = 29
Paso 2: costo = 25
Paso 3: costo = 22
```

---

# Código base

```python
def distance(p1, p2):
    pass


def cost(houses, ambulances):
    pass


def get_neighbors(ambulances):
    pass


def hill_climbing(houses, ambulances):
    pass


houses = [
    (1, 1),
    (2, 7),
    (4, 4),
    (5, 8),
    (7, 2),
    (8, 6),
    (9, 9)
]

ambulances = [
    (0, 0),
    (3, 9),
    (9, 0)
]

solution = hill_climbing(houses, ambulances)

print("\nSolución encontrada:")
print(solution)
```

---

# Preguntas de análisis

1. ¿Qué representa un estado en este problema?
2. ¿Qué representa la función de costo?
3. ¿Cómo se define un vecino?
4. ¿Por qué Hill Climbing puede quedarse atrapado en un mínimo local?
5. ¿Qué ventaja ofrece Random Restart Hill Climbing?
6. ¿Cómo puede Simulated Annealing mejorar la búsqueda?
7. ¿Qué sucede si aumentamos la cantidad de ambulancias?
8. ¿Cuál es la complejidad de explorar todos los estados posibles?
9. ¿Qué diferencias existen entre Hill Climbing y A*?
10. ¿Por qué este problema es de optimización y no de búsqueda clásica?

---

# Reto adicional (Nivel CS50)

Implementa una versión de Random Restart Hill Climbing:

1. Ejecutar Hill Climbing 20 veces.
2. Iniciar cada ejecución con posiciones aleatorias.
3. Guardar la mejor solución encontrada.

Salida esperada:

```text
Intento 1 -> costo 18
Intento 2 -> costo 14
Intento 3 -> costo 16
...
Mejor solución encontrada: costo 12
```

## Desafío extra

Implementa una versión usando Simulated Annealing y compara:

- Costo final.
- Tiempo de ejecución.
- Capacidad para escapar de mínimos locales.
