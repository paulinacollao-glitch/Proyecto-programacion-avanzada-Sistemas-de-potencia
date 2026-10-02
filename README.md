# Sistemas de Potencia - Programación Avanzada

**Autores:** Vicente Flores y Paulina Collao  
**Curso:** Sistemas de Potencia[cite: 1]  
**Asignatura:** Programación Avanzada[cite: 1]  

---

## Descripción del proyecto
Este proyecto implementa un modelo orientados a objetos en Python para la representación y gestión de los elementos que componen una **red eléctrica**[cite: 1]. Permite administrar componentes físicos como generadores, líneas de transmisión y cargas, además de calcular de manera agregada costos operativos, demandas y flujos de corriente dentro del sistema.

---

## Estructura del modelo de clases

El sistema se estructura mediante Programación Orientada a Objetos (POO) utilizando una clase base abstracta y clases derivadas especializadas:

1. **`ElementoRed`** (Clase Base)[cite: 1]:
   * Atributos: `id_equipo` (Identificador único).
2. **`Generador`** (Hereda de `ElementoRed`)[cite: 1]:
   * Representa una central o unidad generadora[cite: 1].
   * Atributos privados: Potencia actual, potencia máxima y costo operativo por unidad de energía[cite: 1].
   * Métodos de validación para asegurar que la potencia actual no supere el límite máximo establecido[cite: 1].
3. **`LineaTransmision`** (Hereda de `ElementoRed`)[cite: 1]:
   * Representa los enlaces de transporte de energía[cite: 1].
   * Atributos: Corriente actual, corriente máxima e impedancia[cite: 1].
4. **`Carga`** (Hereda de `ElementoRed`)[cite: 1]:
   * Representa los puntos de consumo eléctrico[cite: 1].
   * Atributo: Demanda de potencia[cite: 1].
5. **`SistemaPotencia`** (Clase Gestora)[cite: 1]:
   * Administra una colección de elementos de red mediante un diccionario indexado por su ID[cite: 1].
   * Permite agregar elementos, buscarlos por identificador y calcular métricas globales aplicando el principio **DRY (Don't Repeat Yourself)** mediante un método auxiliar genérico (`_calcular_total`)[cite: 1]:
     * `calcular_costo_total()`: Costo operativo total de generación[cite: 1].
     * `calcular_demanda_total()`: Demanda total del sistema[cite: 1].
     * `calcular_corriente_total()`: Corriente total circulante por las líneas[cite: 1].

---

## Ejemplo de uso

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
