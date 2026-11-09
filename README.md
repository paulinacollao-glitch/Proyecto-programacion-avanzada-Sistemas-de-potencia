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
5. **`SistemaPotencia`** (Clase Gestora):
   * Administra una colección de elementos de red mediante un diccionario indexado por su ID.
   * Permite agregar elementos, buscarlos por identificador y calcular métricas globales aplicando el principio **DRY (Don't Repeat Yourself)** mediante un método auxiliar genérico (`_calcular_total`):
     * `calcular_costo_total()`: Costo operativo total de generación.
     * `calcular_demanda_total()`: Demanda total del sistema.
     * `calcular_corriente_total()`: Corriente total circulante por las líneas.

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
