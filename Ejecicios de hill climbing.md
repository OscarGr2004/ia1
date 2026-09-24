
 EJERCICIO 1: Hill Climbing Clásico

    def distance(p1, p2):
        return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    
    def cost(houses, ambulances):
        total_cost = 0
        for h in houses:
            min_dist = min(distance(h, a) for a in ambulances)
            total_cost += min_dist
        return total_cost
    
    def get_neighbors(ambulances):
        neighbors = []
        moves = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    
      for i, (x, y) in enumerate(ambulances):
            for dx, dy in moves:
                nx, ny = x + dx, y + dy
                if 0 <= nx < 10 and 0 <= ny < 10:
                    new_ambulances = ambulances.copy()
                    new_ambulances[i] = (nx, ny)
                    neighbors.append(new_ambulances)
        return neighbors
    
    def hill_climbing(houses, ambulances):
        current = ambulances
        current_cost = cost(houses, current)
        step = 0
    
      print(f"Paso {step}: costo = {current_cost}")
    
     while True:
            neighbors = get_neighbors(current)
            best_neighbor = None
            best_neighbor_cost = current_cost
    
      for neighbor in neighbors:
                c = cost(houses, neighbor)
                if c < best_neighbor_cost:
                    best_neighbor_cost = c
                    best_neighbor = neighbor
    
      if best_neighbor is None:
                return current, current_cost
    
     current = best_neighbor
            current_cost = best_neighbor_cost
            step += 1
            print(f"Paso {step}: costo = {current_cost}")
    
    if __name__ == "__main__":
        houses = [(1, 1), (2, 7), (4, 4), (5, 8), (7, 2), (8, 6), (9, 9)]
        ambulances = [(0, 0), (3, 9), (9, 0)]
    
    print("--- INICIANDO HILL CLIMBING CLÁSICO ---")
        solution, final_cost = hill_climbing(houses, ambulances)
        print("\nSolución encontrada:")
        print(f"Posiciones: {solution} | Costo final: {final_cost}")
    
 EJECUCCION 1: 
    
    tarea/ $ python hill_climbing.py
    --- INICIANDO HILL CLIMBING CLÁSICO ---
    Paso 0: costo = 31
    Paso 1: costo = 29
    Paso 2: costo = 26
    Paso 3: costo = 25
    Paso 4: costo = 24
    Paso 5: costo = 23
    Paso 6: costo = 22
    Paso 7: costo = 21
    Paso 8: costo = 20
    Paso 9: costo = 19
    Paso 10: costo = 18
    
    Solución encontrada:
    Posiciones: [(1, 1), (5, 7), (7, 2)] | Costo final: 18

<img width="432" height="263" alt="image" src="https://github.com/user-attachments/assets/28710af2-0aa8-4509-aaeb-ab27c67c552d" />

EJERCICIO 2: Random Restart Hill Climbing

    import random
    
    def distance(p1, p2):
        return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    
    def cost(houses, ambulances):
        total_cost = 0
        for h in houses:
            min_dist = min(distance(h, a) for a in ambulances)
            total_cost += min_dist
        return total_cost
    
    def get_neighbors(ambulances):
        neighbors = []
        moves = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        
      for i, (x, y) in enumerate(ambulances):
            for dx, dy in moves:
                nx, ny = x + dx, y + dy
                if 0 <= nx < 10 and 0 <= ny < 10:
                    new_ambulances = ambulances.copy()
                    new_ambulances[i] = (nx, ny)
                    neighbors.append(new_ambulances)
        return neighbors
    
    def random_restart_hill_climbing(houses, restarts=20):
        best_overall_solution = None
        best_overall_cost = float('inf')
        
    for i in range(restarts):
            random_ambulances = [(random.randint(0, 9), random.randint(0, 9)) for _ in range(3)]
            current = random_ambulances
            current_cost = cost(houses, current)
            
       while True:
                neighbors = get_neighbors(current)
                best_neighbor = None
                best_cost = current_cost
                
      for neighbor in neighbors:
                    c = cost(houses, neighbor)
                    if c < best_cost:
                        best_cost = c
                        best_neighbor = neighbor
                        
      if best_neighbor is None:
                    break
                    
     current = best_neighbor
                current_cost = best_cost
                
    print(f"Intento {i+1} -> costo {current_cost}")
            
     if current_cost < best_overall_cost:
                best_overall_cost = current_cost
                best_overall_solution = current
                
     print(f"\nMejor solución encontrada: {best_overall_solution} con costo {best_overall_cost}")
        return best_overall_solution
    if __name__ == "__main__":
        houses = [(1, 1), (2, 7), (4, 4), (5, 8), (7, 2), (8, 6), (9, 9)]
         print("--- INICIANDO RANDOM RESTART HILL CLIMBING ---")
        random_restart_hill_climbing(houses)

  EJECUCICON 2: 
    
    tarea/ $ python random_restart_hill_climbing.py 
    --- INICIANDO RANDOM RESTART HILL CLIMBING ---
    Intento 1 -> costo 20
    Intento 2 -> costo 19
    Intento 3 -> costo 19
    Intento 4 -> costo 19
    Intento 5 -> costo 16
    Intento 6 -> costo 16
    Intento 7 -> costo 20
    Intento 8 -> costo 18
    Intento 9 -> costo 18
    Intento 10 -> costo 19
    Intento 11 -> costo 19
    Intento 12 -> costo 19
    Intento 13 -> costo 19
    Intento 14 -> costo 23
    Intento 15 -> costo 20
    Intento 16 -> costo 17
    Intento 17 -> costo 20
    Intento 18 -> costo 17
    Intento 19 -> costo 21
    Intento 20 -> costo 21
    
    Mejor solución encontrada: [(8, 8), (4, 2), (2, 7)] con costo 16
    tarea/ $ 
<img width="446" height="353" alt="image" src="https://github.com/user-attachments/assets/d68b0229-9716-4199-94fc-503600c7a49c" />

  EJERCICIO 3: Simulated Annealing
    import math
    import random
    
    def distance(p1, p2):
        return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    
    def cost(houses, ambulances):
        total_cost = 0
        for h in houses:
            min_dist = min(distance(h, a) for a in ambulances)
            total_cost += min_dist
        return total_cost
    
    def get_neighbors(ambulances):
        neighbors = []
        moves = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    
      for i, (x, y) in enumerate(ambulances):
            for dx, dy in moves:
                nx, ny = x + dx, y + dy
                if 0 <= nx < 10 and 0 <= ny < 10:
                    new_ambulances = ambulances.copy()
                    new_ambulances[i] = (nx, ny)
                    neighbors.append(new_ambulances)
        return neighbors
    
    def simulated_annealing(houses, ambulances, initial_temp=100.0, cooling_rate=0.99):
        current = ambulances
        current_cost = cost(houses, current)
    
     best_solution = current
        best_cost = current_cost
    
      T = initial_temp
        step = 0
    
       while T > 0.1:
            neighbors = get_neighbors(current)
            neighbor = random.choice(neighbors)
            neighbor_cost = cost(houses, neighbor)
    
      delta = current_cost - neighbor_cost
    
    if delta > 0 or random.random() < math.exp(delta / T):
                current = neighbor
                current_cost = neighbor_cost
    
       if current_cost < best_cost:
                    best_cost = current_cost
                    best_solution = current
    
     T *= cooling_rate
            step += 1
    
    print(f"Simulated Annealing terminó en {step} pasos.")
        print(f"Solución: {best_solution} | Costo final: {best_cost}")
        return best_solution, best_cost
    
    if __name__ == "__main__":
        houses = [(1, 1), (2, 7), (4, 4), (5, 8), (7, 2), (8, 6), (9, 9)]
        ambulances = [(0, 0), (3, 9), (9, 0)]
    
     print("--- INICIANDO SIMULATED ANNEALING ---")
        simulated_annealing(houses, ambulances)

  EJECUCION 3:
  
    tarea/ $ python simulated_annealing.py 
    --- INICIANDO SIMULATED ANNEALING ---
    Simulated Annealing terminó en 688 pasos.
    Solución: [(4, 2), (8, 8), (2, 7)] | Costo final: 16
    tarea/ $ 
<img width="371" height="144" alt="image" src="https://github.com/user-attachments/assets/3541c622-ff86-44a4-8f12-6e78180f12b9" />

    Preguntas de análisis
# Preguntas y respuestas

### 1. ¿Qué representa un estado en este problema?

Es la posición actual de las 3 ambulancias en el mapa de 10x10. Básicamente es una lista con las coordenadas `(x, y)` de dónde está parada cada una en ese momento.

### 2. ¿Qué representa la función de costo?

Es la suma de las distancias desde cada casa hasta su ambulancia más cercana. Entre menor sea este número, significa que las ambulancias están mejor repartidas y van a llegar más rápido.

### 3. ¿Cómo se define un vecino?

Es un estado donde mueves una sola ambulancia un cuadrito hacia arriba, abajo, izquierda o derecha, dejando las otras dos exactamente en donde estaban.

### 4. ¿Por qué Hill Climbing puede quedarse atrapado en un mínimo local?

Porque solo revisa los pasos que tiene cerca, es decir, sus vecinos. Si todos los movimientos de alrededor aumentan el costo, el algoritmo cree que ya llegó a la mejor opción y se detiene, aunque unas casillas más allá haya una posición mucho mejor.

### 5. ¿Qué ventaja ofrece Random Restart Hill Climbing?

Que al colocar las ambulancias en lugares aleatorios varias veces, no te quedas atascado siempre en el mismo mínimo local. Así tienes más probabilidades de encontrar una solución cercana a la mejor solución global.

### 6. ¿Cómo puede Simulated Annealing mejorar la búsqueda?

Porque al principio, cuando la temperatura es alta, se permite aceptar cambios que empeoran el costo. Esto ayuda a dar "saltos" para salir de un mínimo local y seguir explorando antes de asentarse en una solución.

### 7. ¿Qué sucede si aumentamos la cantidad de ambulancias?

El costo puede bajar porque habrá ambulancias más cerca de cada casa. Sin embargo, la computadora puede tardar más en procesarlo porque habrá más posibilidades y combinaciones que revisar.

### 8. ¿Cuál es la complejidad de explorar todos los estados posibles?

Para 3 ambulancias en 100 casillas hay aproximadamente 161,700 combinaciones posibles, suponiendo que no pueden ocupar la misma casilla. Para este mapa pequeño la fuerza bruta todavía se puede calcular, pero si el mapa fuera de 1000x1000 se volvería mucho más difícil de procesar.

### 9. ¿Qué diferencias existen entre Hill Climbing y A*?

En A* lo que importa es encontrar una ruta o camino paso a paso para llegar a una meta. En Hill Climbing no importa cómo se llegó a la solución, sino encontrar la mejor configuración final de las ambulancias.

### 10. ¿Por qué este problema es de optimización y no de búsqueda clásica?

Porque no estamos buscando llegar a una casilla específica ni calcular una ruta. Lo que buscamos es ajustar las posiciones de las ambulancias para obtener el menor costo posible. En otras palabras, queremos encontrar la mejor solución entre muchas opciones.


     COMPARATIVA ALGORITMOS 
Hill Climbing :
Es el más rápido en ejecutar porque hace muy pocos pasos. Su desventaja es que es muy simple: en cuanto se topa con un mínimo local se atora y ya no sigue buscando, así que casi nunca te da el costo más bajo.

Random-Restart:
Corre rápido aunque haga los 20 intentos. Evita quedarse atorado simplemente cambiando la posición inicial de las ambulancias en cada intento, por lo que casi siempre termina encontrando la mejor solución del mapa.

Simulated Annealing :
Tarda un poco más de tiempo por el proceso de la temperatura, pero es el más eficiente para explorar. Se sale de los mínimos locales porque al principio acepta movimientos peores a propósito para recorrer el mapa, logrando un costo muy bajo sin tener que reiniciar desde cero.
 
