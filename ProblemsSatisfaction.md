Constraint Satisfaction Problems (CSP) - Python
OSCAR OCTAVIO GARCIA MAYA 182889 IA1

Problema 1: Programación de Presentaciones de Proyectos de IA

    import matplotlib
    matplotlib.use('Agg')
    import matplotlib.pyplot as plt
    import networkx as nx

    def resolver_problema_1():
    variables = ['A', 'B', 'C', 'D', 'E']
    dominios = {v: ['Mañana', 'Tarde', 'Noche'] for v in variables}
    restricciones = [('A', 'B'), ('A', 'C'), ('B', 'D'), ('C', 'E'), ('D', 'E')]

    # Generar y guardar imagen del grafo
    G = nx.Graph()
    G.add_nodes_from(variables)
    G.add_edges_from(restricciones)
    plt.figure(figsize=(6, 4))
    pos = nx.spring_layout(G, seed=42)
    nx.draw(G, pos, with_labels=True, node_color='skyblue', node_size=2000, font_weight='bold', font_size=12)
    plt.title("Grafo de Restricciones - Problema 1")
    plt.savefig("problema1_grafo.png", bbox_inches='tight')
    plt.close()

    print("=== PROBLEMA 1 ===")
    print("Consistencia de nodos: OK (Sin restricciones unarias).")
    print(f"Dominios tras AC-3: {dominios}")

    adj = {v: set() for v in variables}
    for u, v in restricciones:
        adj[u].add(v)
        adj[v].add(u)

    soluciones = []
    def backtrack(asignacion):
        if len(asignacion) == len(variables):
            soluciones.append(dict(asignacion))
            return
        no_asignadas = [v for v in variables if v not in asignacion]
        var = min(no_asignadas, key=lambda v: (len(dominios[v]), -sum(1 for nbr in adj[v] if nbr not in asignacion)))
        for val in dominios[var]:
            if all(val != asignacion[nbr] for nbr in adj[var] if nbr in asignacion):
                asignacion[var] = val
                backtrack(asignacion)
                del asignacion[var]

    backtrack({})
    print(f"Total de soluciones válidas: {len(soluciones)}")
    print(f"Ejemplo de solución: {soluciones[0]}\n")

    if __name__ == "__main__":
    resolver_problema_1()
    === PROBLEMA 1 ===
Consistencia de nodos: OK (Sin restricciones unarias).
Dominios tras AC-3: {'A': ['Mañana', 'Tarde', 'Noche'], 'B': ['Mañana', 'Tarde', 'Noche'], 'C': ['Mañana', 'Tarde', 'Noche'], 'D': ['Mañana', 'Tarde', 'Noche'], 'E': ['Mañana', 'Tarde', 'Noche']}
Total de soluciones válidas: 30
Ejemplo de solución: {'A': 'Mañana', 'D': 'Mañana', 'C': 'Tarde', 'B': 'Tarde', 'E': 'Noche'}

<img width="620" height="441" alt="image" src="https://github.com/user-attachments/assets/9a4143ff-3dc8-433a-a961-64595c025e13" />

Problema 2: Asignación de Laboratorios de IA

    import matplotlib
    matplotlib.use('Agg')
    import matplotlib.pyplot as plt
    import networkx as nx
  
    def resolver_problema_2():
    variables = ['P', 'V', 'R', 'C', 'D']
    dominios = {v: ['Lab1', 'Lab2', 'Lab3'] for v in variables}
    dominios['R'].remove('Lab1')
    dominios['D'].remove('Lab3')
    restricciones = [('P', 'C'), ('V', 'R'), ('C', 'D')]

    G = nx.Graph()
    G.add_nodes_from(variables)
    G.add_edges_from(restricciones)
    plt.figure(figsize=(6, 4))
    pos = nx.circular_layout(G)
    nx.draw(G, pos, with_labels=True, node_color='lightgreen', node_size=2000, font_weight='bold', font_size=12)
    plt.title("Grafo de Restricciones - Problema 2")
    plt.savefig("problema2_grafo.png", bbox_inches='tight')
    plt.close()

    print("=== PROBLEMA 2 ===")
    print("Dominios actualizados por Consistencia de Nodos:")
    for var, dom in dominios.items():
        print(f"  {var}: {dom}")

    adj = {v: set() for v in variables}
    for u, v in restricciones:
        adj[u].add(v)
        adj[v].add(u)

    soluciones = []
    def backtrack(asignacion):
        if len(asignacion) == len(variables):
            soluciones.append(dict(asignacion))
            return
        var = [v for v in variables if v not in asignacion][0]
        for val in dominios[var]:
            if all(val != asignacion[nbr] for nbr in adj[var] if nbr in asignacion):
                asignacion[var] = val
                backtrack(asignacion)
                del asignacion[var]

    backtrack({})
    print(f"Total de soluciones válidas: {len(soluciones)}")
    print(f"Ejemplo de solución: {soluciones[0]}\n")

    if __name__ == "__main__":
    resolver_problema_2()
    TareaProblems/ $ /usr/local/bin/python /workspaces/198798135/TareaProblems/problema2.py
  === PROBLEMA 2 ===
Dominios actualizados por Consistencia de Nodos:
  P: ['Lab1', 'Lab2', 'Lab3']
  V: ['Lab1', 'Lab2', 'Lab3']
  R: ['Lab2', 'Lab3']
  C: ['Lab1', 'Lab2', 'Lab3']
  D: ['Lab1', 'Lab2']
Total de soluciones válidas: 32
Ejemplo de solución: {'P': 'Lab1', 'V': 'Lab1', 'R': 'Lab2', 'C': 'Lab2', 'D': 'Lab1'}

<img width="620" height="441" alt="image" src="https://github.com/user-attachments/assets/d72ce8b2-ca5a-4773-9ab1-6d4710775d4e" />

Problema 3: Asignación de GPUs para Entrenamiento
    
    import matplotlib
    matplotlib.use('Agg')
    import matplotlib.pyplot as plt
    import networkx as nx

    def resolver_problema_3():
    variables = ['A', 'B', 'C', 'D']
    gpus = ['GPU-A100', 'GPU-RTX4090', 'GPU-H100']
    dominios = {v: list(gpus) for v in variables}
    dominios['D'].remove('GPU-RTX4090')
    dominios['B'].remove('GPU-RTX4090')
    restricciones = [('A', 'D'), ('B', 'C'), ('C', 'D')]
    arcos = [('A', 'D'), ('D', 'A'), ('B', 'C'), ('C', 'B'), ('C', 'D'), ('D', 'C')]

    # Generar y guardar imagen del grafo
    G = nx.Graph()
    G.add_nodes_from(variables)
    G.add_edges_from(restricciones)
    plt.figure(figsize=(6, 4))
    pos = nx.spring_layout(G, seed=42)
    nx.draw(G, pos, with_labels=True, node_color='gold', node_size=2000, font_weight='bold', font_size=12)
    plt.title("Grafo de Restricciones - Problema 3")
    plt.savefig("problema3_grafo.png", bbox_inches='tight')
    plt.close()

    print("=== PROBLEMA 3 ===")
    print(f"Arcos del CSP: {arcos}")
    print("Dominios tras Consistencia de Nodos:")
    for k, v in dominios.items():
        print(f"  {k}: {v}")

    candidatos_mrv = [v for v in variables if len(dominios[v]) == min(len(dominios[x]) for x in variables)]
    adj = {v: set() for v in variables}
    for u, v in restricciones:
        adj[u].add(v)
        adj[v].add(u)
    
    variable_mrv = max(candidatos_mrv, key=lambda v: len(adj[v]))
    print(f"Variable seleccionada primero (MRV + Degree): {variable_mrv}")

    soluciones = []
    def backtrack(asignacion):
        if len(asignacion) == len(variables):
            soluciones.append(dict(asignacion))
            return
        no_asignadas = [v for v in variables if v not in asignacion]
        var = min(no_asignadas, key=lambda v: (len(dominios[v]), -sum(1 for nbr in adj[v] if nbr not in asignacion)))
        for val in dominios[var]:
            if all(val != asignacion[nbr] for nbr in adj[var] if nbr in asignacion):
                asignacion[var] = val
                backtrack(asignacion)
                del asignacion[var]

    backtrack({})
    print(f"Total de soluciones válidas: {len(soluciones)}")
    print(f"Ejemplo de solución: {soluciones[0]}\n")

    if __name__ == "__main__":
    resolver_problema_3()
<img width="620" height="441" alt="image" src="https://github.com/user-attachments/assets/b2c92282-13b2-4f01-a219-b415df6e9698" />


TareaProblems/ $ python problema3.py
=== PROBLEMA 3 ===
Arcos del CSP: [('A', 'D'), ('D', 'A'), ('B', 'C'), ('C', 'B'), ('C', 'D'), ('D', 'C')]
Dominios tras Consistencia de Nodos:
  A: ['GPU-A100', 'GPU-RTX4090', 'GPU-H100']
  B: ['GPU-A100', 'GPU-H100']
  C: ['GPU-A100', 'GPU-RTX4090', 'GPU-H100']
  D: ['GPU-A100', 'GPU-H100']
Variable seleccionada primero (MRV + Degree): D
Total de soluciones válidas: 12
Ejemplo de solución: {'D': 'GPU-A100', 'B': 'GPU-A100', 'A': 'GPU-RTX4090', 'C': 'GPU-RTX4090'}


Problema 4: Organización de un Hackathon de IA

    import matplotlib
    matplotlib.use('Agg')
    import matplotlib.pyplot as plt
    import networkx as nx

    def resolver_problema_4():
    variables = ['E1', 'E2', 'E3', 'E4', 'E5']
    salas = ['Sala A', 'Sala B', 'Sala C']
    dominios = {v: list(salas) for v in variables}
    dominios['E1'].remove('Sala C')
    dominios['E4'].remove('Sala C')
    restricciones = [('E1', 'E2'), ('E1', 'E3'), ('E2', 'E4'), ('E3', 'E5'), ('E4', 'E5')]
    
    # Generar y guardar imagen del grafo
    G = nx.Graph()
    G.add_nodes_from(variables)
    G.add_edges_from(restricciones)
    plt.figure(figsize=(6, 4))
    pos = nx.spring_layout(G, seed=42)
    nx.draw(G, pos, with_labels=True, node_color='orchid', node_size=2000, font_weight='bold', font_size=12)
    plt.title("Grafo de Restricciones - Problema 4")
    plt.savefig("problema4_grafo.png", bbox_inches='tight')
    plt.close()

    adj = {v: set() for v in variables}
    for u, v in restricciones:
        adj[u].add(v)
        adj[v].add(u)

    print("=== PROBLEMA 4 ===")
    print("Dominios tras Consistencia de Nodos:")
    for k, v in dominios.items():
        print(f"  {k}: {v}")

    llamadas_sin_mrv = [0]
    sol_sin_mrv = []
    def backtrack_no_mrv(asignacion, unassigned):
        llamadas_sin_mrv[0] += 1
        if not unassigned:
            sol_sin_mrv.append(dict(asignacion))
            return True
        var = unassigned[0]
        for val in dominios[var]:
            if all(val != asignacion[nbr] for nbr in adj[var] if nbr in asignacion):
                asignacion[var] = val
                if backtrack_no_mrv(asignacion, unassigned[1:]):
                    return True
                del asignacion[var]
        return False

    backtrack_no_mrv({}, list(variables))

    llamadas_con_mrv = [0]
    sol_con_mrv = []
    def backtrack_mrv(asignacion):
        llamadas_con_mrv[0] += 1
        if len(asignacion) == len(variables):
            sol_con_mrv.append(dict(asignacion))
            return True
        no_asig = [v for v in variables if v not in asignacion]
        var = min(no_asig, key=lambda v: sum(1 for val in dominios[v] if all(val != asignacion[nbr] for nbr in adj[v] if nbr in asignacion)))
        for val in dominios[var]:
            if all(val != asignacion[nbr] for nbr in adj[var] if nbr in asignacion):
                asignacion[var] = val
                if backtrack_mrv(asignacion):
                    return True
                del asignacion[var]
        return False

    backtrack_mrv({})
    print(f"Sin MRV -> Llamadas: {llamadas_sin_mrv[0]}, Solución: {sol_sin_mrv[0]}")
    print(f"Con MRV -> Llamadas: {llamadas_con_mrv[0]}, Solución: {sol_con_mrv[0]}\n")

    if __name__ == "__main__":
    resolver_problema_4()

=== PROBLEMA 4 ===
Dominios tras Consistencia de Nodos:
  E1: ['Sala A', 'Sala B']
  E2: ['Sala A', 'Sala B', 'Sala C']
  E3: ['Sala A', 'Sala B', 'Sala C']
  E4: ['Sala A', 'Sala B']
  E5: ['Sala A', 'Sala B', 'Sala C']
Sin MRV -> Llamadas: 6, Solución: {'E1': 'Sala A', 'E2': 'Sala B', 'E3': 'Sala B', 'E4': 'Sala A', 'E5': 'Sala C'}
Con MRV -> Llamadas: 6, Solución: {'E1': 'Sala A', 'E2': 'Sala B', 'E4': 'Sala A', 'E3': 'Sala B', 'E5': 'Sala C'}

<img width="620" height="441" alt="image" src="https://github.com/user-attachments/assets/d64c9670-1971-4411-a65d-0592c66cb224" />

Problema 5: Calendario de Exámenes de Ciberseguridad e IA

    import matplotlib
    matplotlib.use('Agg')
    import matplotlib.pyplot as plt
    import networkx as nx

    def resolver_problema_5():
    variables = ['H', 'C', 'M', 'I', 'S', 'F']
    dias = ['Lunes', 'Martes', 'Miércoles']
    dominios = {v: list(dias) for v in variables}
    dominios['I'].remove('Lunes')
    dominios['C'].remove('Miércoles')
    restricciones = [
        ('H', 'C'), ('H', 'M'), ('C', 'I'),
        ('M', 'I'), ('M', 'S'), ('S', 'F'), ('I', 'F')
    ]

    G = nx.Graph()
    G.add_nodes_from(variables)
    G.add_edges_from(restricciones)
    plt.figure(figsize=(6, 4))
    pos = nx.spring_layout(G, seed=42)
    nx.draw(G, pos, with_labels=True, node_color='orange', node_size=2000, font_weight='bold', font_size=12)
    plt.title("Grafo de Restricciones - Problema 5")
    plt.savefig("problema5_grafo.png", bbox_inches='tight')
    plt.close()

    adj = {v: set() for v in variables}
    for u, v in restricciones:
        adj[u].add(v)
        adj[v].add(u)

    print("=== PROBLEMA 5 ===")
    print("Dominios tras Consistencia de Nodos:")
    for k, v in dominios.items():
        print(f"  {k}: {v}")

    mrv_vars = [v for v in variables if len(dominios[v]) == min(len(dominios[x]) for x in variables)]
    primera_var = max(mrv_vars, key=lambda v: len(adj[v]))
    print(f"1a Variable seleccionada (MRV + Degree): {primera_var}")

    soluciones = []
    def backtrack(asignacion):
        if len(asignacion) == len(variables):
            soluciones.append(dict(asignacion))
            return
        no_asig = [v for v in variables if v not in asignacion]
        var = min(no_asig, key=lambda v: (len(dominios[v]), -sum(1 for nbr in adj[v] if nbr not in asignacion)))
        for val in dominios[var]:
            if all(val != asignacion[nbr] for nbr in adj[var] if nbr in asignacion):
                asignacion[var] = val
                backtrack(asignacion)
                del asignacion[var]

    backtrack({})
    print(f"Total de soluciones válidas: {len(soluciones)}")
    print(f"Ejemplo de solución: {soluciones[0]}\n")

    if __name__ == "__main__":
    resolver_problema_5()

=== PROBLEMA 5 ===
Dominios tras Consistencia de Nodos:
  H: ['Lunes', 'Martes', 'Miércoles']
  C: ['Lunes', 'Martes']
  M: ['Lunes', 'Martes', 'Miércoles']
  I: ['Martes', 'Miércoles']
  S: ['Lunes', 'Martes', 'Miércoles']
  F: ['Lunes', 'Martes', 'Miércoles']
1a Variable seleccionada (MRV + Degree): I
Total de soluciones válidas: 27
Ejemplo de solución: {'I': 'Martes', 'C': 'Lunes', 'M': 'Lunes', 'S': 'Martes', 'H': 'Martes', 'F': 'Lunes'}

<img width="620" height="441" alt="image" src="https://github.com/user-attachments/assets/5b2b8afa-6fa3-4ce3-ae87-18269abd5c4a" />
