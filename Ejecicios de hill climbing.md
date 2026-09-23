
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
