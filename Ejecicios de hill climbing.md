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

