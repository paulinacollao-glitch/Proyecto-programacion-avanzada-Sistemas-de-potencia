# Sistemas de Potencia - Programación Avanzada

**Autores:** Vicente Flores y Paulina Collao  
**Proyecto:** Sistemas de Potencia
**Asignatura:** Programación Avanzada 

---

## Descripción del proyecto
Este proyecto implementa un modelo orientados a objetos en Python para la representación y gestión de los elementos que componen una **red eléctrica**. Permite administrar componentes físicos como generadores, líneas de transmisión y cargas, además de calcular de manera agregada costos operativos, demandas y flujos de corriente dentro del sistema.

---

## Estructura del modelo de clases

El sistema se estructura mediante Programación Orientada a Objetos (POO) utilizando una clase base abstracta y clases derivadas especializadas:

1. **`ElementoRed`** (Clase Base):
   * Atributos: `id_equipo` (Identificador único).
2. **`Generador`** (Hereda de `ElementoRed`):
   * Representa una central o unidad generadora.
   * Atributos privados: Potencia actual, potencia máxima y costo operativo por unidad de energía.
   * Métodos de validación para asegurar que la potencia actual no supere el límite máximo establecido.
3. **`LineaTransmision`** (Hereda de `ElementoRed`):
   * Representa los enlaces de transporte de energía.
   * Atributos: Corriente actual, corriente máxima e impedancia.
4. **`Carga`** (Hereda de `ElementoRed`):
   * Representa los puntos de consumo eléctrico.
   * Atributo: Demanda de potencia.
5. **`Nodo`** (Hereda de `ElementoRed`):
   * Representa una barra del sistema eléctrico.
6. **`SistemaPotencia`** (Clase Gestora):
   * Administra una colección de elementos de red mediante un diccionario indexado por su ID.
   * Permite agregar elementos, buscarlos por identificador y calcular métricas globales aplicando el principio **DRY (Don't Repeat Yourself)** mediante un método auxiliar genérico (`_calcular_total`):
     * `calcular_costo_total()`: Costo operativo total de generación.
     * `calcular_demanda_total()`: Demanda total del sistema.
     * `calcular_corriente_total()`: Corriente total circulante por las líneas.

---

## Ejemplo de uso
### Hito 1: Creación y gestión de elementos de la red
A continuación se muestra un ejemplo rápido de cómo instanciar elementos y agregarlos al sistema:

```python
# Creación de componentes de la red
g1 = Generador(
    id_equipo="G1",
    potencia=100,
    potencia_max=150,
    costo_op=25
)

c1 = Carga(
    id_equipo="C1",
    demanda=80
)

# Consultas básicas
print(g1.get_potencia())   # Salida: 100
print(g1.get_costo_op())   # Salida: 25
print(c1.get_demanda())    # Salida: 80

# Agregar elementos al sistema
sistema = SistemaPotencia()
sistema.agregar_elemento(g1)
sistema.agregar_elemento(c1)
```

### Hito 2: Ordenamiento, despacho económico y tablas hash

En este hito se incorporan nuevas funciones para ordenar generadores según su costo operativo mediante MergeSort, realizar un despacho económico uninodal y buscar elementos utilizando un diccionario.

#### 1. Ordenamiento mediante MergeSort

Se crean tres generadores con distintos costos operativos y se ordenan de menor a mayor costo.

```python
# Crear generadores
g1 = Generador("G1", 0, 60, 35)
g2 = Generador("G2", 0, 50, 20)
g3 = Generador("G3", 0, 40, 28)

generadores = [g1, g2, g3]

# Ordenamiento mediante MergeSort
ordenados = merge_sort_generadores(generadores)

for g in ordenados:
    print(g.get_costo_op())
```

Salida esperada:

```text
20
28
35
```


#### 2. Despacho económico uninodal

A partir de los generadores anteriores, se realiza un despacho económico para abastecer la demanda de una carga de 80 MW, priorizando los generadores de menor costo operativo.

```python
# Crear carga
c1 = Carga("C1", 80)

# Realizar despacho económico
despacho = despacho_economico(
    generadores,
    c1.get_demanda()
)

print(despacho)
```

Salida esperada:

```text
{'G1': 0, 'G2': 50, 'G3': 30}
```

El despacho permite satisfacer la demanda de la carga respetando las potencias máximas de cada generador, priorizando aquellos con menor costo operativo.


#### 3. Búsqueda de barras mediante tablas hash

Se utiliza un diccionario para almacenar los nodos o barras del sistema eléctrico, permitiendo buscarlos directamente mediante su identificador.

```python
# Crear sistema de potencia
sistema = SistemaPotencia()

# Registrar dos barras con identificadores únicos
sistema.agregar_elemento(Nodo("Barra_Quillota"))
sistema.agregar_elemento(Nodo("Barra_Centro"))

# Buscar una barra por su id
barra = sistema.buscar_elemento_por_id("Barra_Quillota")

print(barra.id_equipo)
```

Salida esperada:

```text
Barra_Quillota
```


El uso de un diccionario permite realizar búsquedas por identificador con una complejidad promedio de O(1), en comparación con O(n) para una búsqueda secuencial en una lista.
