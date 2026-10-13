# Reporte de Resolución CSP: Asignación de Tareas a Robots de Almacén

**Objetivo:** Resolver un problema de satisfacción de restricciones (CSP) aplicando Node Consistency, AC-3, Heurísticas (MRV, Degree, LCV) y Backtracking, mostrando tanto la implementación en código como la traza manual del algoritmo.

---

## 1. Definición del Modelo (Contexto y Restricciones)

* **Contexto:** Un centro logístico con robots autónomos y tareas por asignar.
* **Variables (Tareas):** `T1` (Transportar), `T2` (Clasificar), `T3` (Reabastecer), `T4` (Inventario), `T5` (Empaquetado)
* **Dominios (Robots Disponibles):** `{R1, R2, R3, R4}` para cada variable.
* **Restricciones:**
  1. Un robot no puede ejecutar más de una tarea simultánea.
  2. T3 solo puede ser realizada por R2 o R4. (Unaria)
  3. T4 solo puede ser realizada por R1. (Unaria)
  4. T1 y T2 requieren robots diferentes: T1 ≠ T2 (Binaria)
  5. T5 no puede asignarse a R3: T5 ≠ R3 (Unaria)
  6. Si T1 se asigna a R2, entonces T3 debe asignarse a R4: T1 = R2 ⇒ T3 = R4 (Binaria)

---

## 2. Implementación en Python

```python
class WarehouseCSP:
    def __init__(self):
        self.variables = ['T1', 'T2', 'T3', 'T4', 'T5']
        self.domains = {var: ['R1', 'R2', 'R3', 'R4'] for var in self.variables}
        self.arcs = [('T1', 'T2'), ('T2', 'T1'), ('T1', 'T3'), ('T3', 'T1')]

    def enforce_node_consistency(self):
        self.domains['T3'] = [v for v in self.domains['T3'] if v in ['R2', 'R4']]
        self.domains['T4'] = [v for v in self.domains['T4'] if v == 'R1']
        self.domains['T5'] = [v for v in self.domains['T5'] if v != 'R3']

    def check_binary_constraint(self, x, x_val, y, y_val):
        if (x == 'T1' and y == 'T2') or (x == 'T2' and y == 'T1'):
            return x_val != y_val
        if x == 'T1' and y == 'T3':
            if x_val == 'R2': return y_val == 'R4'
            return True
        if x == 'T3' and y == 'T1':
            if y_val == 'R2': return x_val == 'R4'
            return True
        return True

    def revise(self, x, y):
        revised = False
        for x_val in self.domains[x][:]:
            if all(not self.check_binary_constraint(x, x_val, y, y_val) for y_val in self.domains[y]):
                self.domains[x].remove(x_val)
                revised = True
        return revised

    def ac3(self, arcs=None):
        if arcs is None:
            arcs = self.arcs[:]
        queue = arcs[:]
        
        while queue:
            x, y = queue.pop(0)
            if self.revise(x, y):
                if not self.domains[x]:
                    return False
                for neighbor in [n for (n, var) in self.arcs if var == x and n != y]:
                    queue.append((neighbor, x))
        return True

    def assignment_complete(self, assignment):
        return len(assignment) == len(self.variables)

    def consistent(self, assignment):
        if 'T1' in assignment and 'T2' in assignment:
            if assignment['T1'] == assignment['T2']: return False
        if 'T1' in assignment and 'T3' in assignment:
            if assignment['T1'] == 'R2' and assignment['T3'] != 'R4': return False
        return True

    def get_degree(self, var, assignment):
        return sum(1 for (x, y) in self.arcs if x == var and y not in assignment)

    def select_unassigned_variable(self, assignment):
        unassigned = [v for v in self.variables if v not in assignment]
        unassigned.sort(key=lambda var: (len(self.domains[var]), -self.get_degree(var, assignment)))
        return unassigned[0]

    def order_domain_values(self, var, assignment):
        def count_conflicts(val):
            conflicts = 0
            for neighbor in [y for (x, y) in self.arcs if x == var and y not in assignment]:
                for neighbor_val in self.domains[neighbor]:
                    if not self.check_binary_constraint(var, val, neighbor, neighbor_val):
                        conflicts += 1
            return conflicts
        return sorted(self.domains[var], key=count_conflicts)

    def backtrack(self, assignment):
        if self.assignment_complete(assignment):
            return assignment

        var = self.select_unassigned_variable(assignment)
        
        for value in self.order_domain_values(var, assignment):
            assignment[var] = value
            if self.consistent(assignment):
                result = self.backtrack(assignment)
                if result is not None:
                    return result
            del assignment[var]
            
        return None
# ==========================================
# BLOQUE DE EJECUCIÓN (Lo que faltaba)
# ==========================================
if __name__ == '__main__':
    print("Iniciando CSP de Robots de Almacén...\n")
    
    # 1. Instanciar el problema
    csp = WarehouseCSP()
    print("1. Dominios Iniciales:")
    for var, dom in csp.domains.items():
        print(f"   {var}: {dom}")
        
    # 2. Aplicar Node Consistency
    csp.enforce_node_consistency()
    print("\n2. Dominios después de Node Consistency:")
    for var, dom in csp.domains.items():
        print(f"   {var}: {dom}")
        
    # 3. Aplicar AC-3
    csp.ac3()
    print("\n3. Dominios después de AC-3:")
    for var, dom in csp.domains.items():
        print(f"   {var}: {dom}")
        
    # 4. Resolver usando Backtracking (que internamente usa MRV, Degree y LCV)
    print("\n4. Iniciando Búsqueda Backtracking...")
    solucion = csp.backtrack({})
    
    # Mostrar el resultado final
    print("\n==========================================")
    if solucion:
        print("¡SOLUCIÓN ENCONTRADA!")
        for tarea, robot in sorted(solucion.items()):
            print(f"La {tarea} se asigna al robot {robot}")
    else:
        print("No se encontró ninguna solución que satisfaga todas las restricciones.")
    print("==========================================")
```

---

## 3. Rastreo Manual (Prueba de Escritorio)

El siguiente procedimiento demuestra el comportamiento interno del algoritmo paso a paso, mostrando las salidas en cada etapa.

### Paso 0: Dominios Iniciales
* T1: `{R1, R2, R3, R4}` | T2: `{R1, R2, R3, R4}` | T3: `{R1, R2, R3, R4}` | T4: `{R1, R2, R3, R4}` | T5: `{R1, R2, R3, R4}`

### Paso 1: Node Consistency (Consistencia de Nodo)
Se evalúan las restricciones unarias (Restricciones 2, 3 y 5).
* **T3:** Se eliminan R1 y R3. Queda `{R2, R4}`.
* **T4:** Se eliminan R2, R3 y R4. Queda `{R1}`.
* **T5:** Se elimina R3. Queda `{R1, R2, R4}`.
* **Salida de dominios:** 
  `T1{R1,R2,R3,R4}`, `T2{R1,R2,R3,R4}`, `T3{R2,R4}`, `T4{R1}`, `T5{R1,R2,R4}`

### Paso 2: AC-3 (Consistencia de Arco)
Se evalúan los arcos `(T1, T2)` y `(T1, T3)` buscando si algún valor pierde soporte.
* **(T1, T2) y (T2, T1):** Todos los valores en T1 tienen al menos un valor diferente en T2. (Sin poda).
* **(T1, T3):** Si T1=R2, existe R4 en T3. (Sin poda).
* **(T3, T1):** Los valores R2 y R4 de T3 tienen soporte válido en T1. (Sin poda).
* **Salida de dominios:** Idénticos al Paso 1.

### Paso 3: Búsqueda Backtracking con Heurísticas

**Iteración 1: Asignación de la 1ra variable**
* **MRV:** Tamaños de dominio: T1(4), T2(4), T3(2), T4(1), T5(3). Se selecciona **T4** por tener el dominio más pequeño.
* **Asignación:** `T4 = R1`
* *Estado parcial:* `{T4: R1}`

**Iteración 2: Asignación de la 2da variable**
* **MRV:** Se selecciona **T3** (dominio de 2).
* **LCV para T3 {R2, R4}:** Si T3=R2, restringe a T1 impidiendo que sea R2. Si T3=R4, no restringe a T1. LCV elige el menos restrictivo: **R4**.
* **Asignación:** `T3 = R4`
* *Estado parcial:* `{T4: R1, T3: R4}`

**Iteración 3: Asignación de la 3ra variable**
* **MRV:** Se selecciona **T5** (dominio de 3).
* **LCV:** Se toma el primero al no tener conflictos con variables futuras.
* **Asignación:** `T5 = R1`
* *Estado parcial:* `{T4: R1, T3: R4, T5: R1}`

**Iteración 4: Asignación de la 4ta variable**
* **MRV:** Quedan T1(4) y T2(4) (Empate).
* **Degree Heuristic:** Grado T1 = 1, Grado T2 = 1. Se desempata por orden de lista: **T1**.
* **Asignación:** `T1 = R1` (Se toma el primer valor válido).
* *Estado parcial:* `{T4: R1, T3: R4, T5: R1, T1: R1}`

**Iteración 5: Asignación de la 5ta variable**
* **Selección:** Solo queda **T2** `{R1, R2, R3, R4}`.
* **Evaluación Backtracking:**
  * *Intento 1:* `T2 = R1`. La restricción T1 ≠ T2 falla (`R1 ≠ R1`). Se rechaza (Backtrack).
  * *Intento 2:* `T2 = R2`. La restricción T1 ≠ T2 se cumple (`R1 ≠ R2`).
* **Asignación final:** `T2 = R2`

**Solución Final:** 
`{T1: R1, T2: R2, T3: R4, T4: R1, T5: R1}`

---

## 4. Preguntas para el Reporte

**1. ¿Cuántos valores fueron eliminados por Node Consistency?**
Se eliminaron un total de **6 valores**: 2 valores en T3 (R1, R3), 3 valores en T4 (R2, R3, R4) y 1 valor en T5 (R3).

**2. ¿Qué dominios se redujeron después de ejecutar AC-3?**
**Ninguno**. Tras aplicar Node Consistency, los dominios restantes eran lo suficientemente amplios (ej. T1 y T2 aún tenían 4 valores). Cada valor de una variable encontró al menos un "soporte" válido en su variable vecina, por lo que AC-3 no podó valores adicionales en esta etapa.

**3. ¿Qué variable fue seleccionada primero por MRV?**
La variable **T4**. Gracias a la poda inicial de Node Consistency, su dominio quedó reducido a 1 solo valor (`R1`). MRV prioriza variables con menos alternativas para evitar ramificaciones inútiles.

**4. ¿Cómo influyó Degree Heuristic en la búsqueda?**
Actuó como el **mecanismo de desempate** para MRV. Si dos variables empatan en tamaño de dominio, Degree Heuristic selecciona la que participa en más restricciones binarias con variables no asignadas (mayor grado). Esto fuerza al algoritmo a resolver los cuellos de botella estructurales primero.

**5. ¿Cuántos estados exploró Backtracking?**
Exploró **5 estados directos** y realizó **0 retrocesos profundos (backtracks)**. Solo hubo un pequeño rechazo local de valor al final (evaluando T2=R1), pasando de inmediato al valor correcto. Esto prueba la inmensa eficiencia de combinar MRV y LCV.

**6. ¿Por qué este problema puede modelarse como un CSP?**
Porque cumple perfectamente con la tríada matemática de un CSP:
* Un **conjunto finito de variables discretas** (las 5 tareas).
* Un **dominio finito** de valores mutuamente excluyentes para cada variable (los 4 robots).
* Un **conjunto de restricciones lógicas** (unarias y binarias). El objetivo no es maximizar o minimizar una métrica, sino encontrar un estado exacto que satisfaga todas las reglas dadas simultáneamente.

---

## 5. Reto Adicional (Evolución del Modelo)

Para llevar este script básico hacia un sistema logístico del mundo real, se requiere evolucionar la arquitectura del CSP:

* **Agregar prioridades de tareas & Optimizar energía:** Transformar el modelo a un **CSOP (Constraint Satisfaction Optimization Problem)**. En lugar de usar un Backtracking clásico que se detiene en la primera solución, se implementa *Branch and Bound*. Esto permite evaluar soluciones basadas en una "función de costo" (por ejemplo, asignar tareas a robots con menos batería aumenta el costo) y devolver la solución óptima.
* **Permitir fallos de robots:** Requiere un **DCSP (Dynamic CSP)**. Si un robot falla durante la ejecución, su valor se retira del dominio dinámicamente. En lugar de reiniciar todo el cálculo, se utilizan algoritmos de reparación local (como *Min-Conflicts*) para reasignar únicamente las tareas que tenía el robot averiado, minimizando las disrupciones.
* **Generación Automática de Instancias:** Se puede construir un script que genere restricciones aleatorias y utilice **AC-3 como filtro de viabilidad**. Si al generar un problema el algoritmo AC-3 detecta que un dominio se vacía (retorna `False`), el sistema sabe que el problema no tiene solución lógica y lo descarta automáticamente antes de presentarlo.
