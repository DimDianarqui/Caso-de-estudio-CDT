# Caso de estudio: Proyección de Certificado de Depósito a Término (CDT)

## Descripción

Este proyecto consiste en una aplicación desarrollada en Python para registrar y realizar la proyección de un **Certificado de Depósito a Término (CDT)**.

La aplicación permite ingresar un monto inicial y un plazo en meses, calcular la tasa mensual correspondiente y generar una proyección del crecimiento del saldo durante todo el plazo del CDT.

La aplicación cuenta con una interfaz gráfica desarrollada mediante **Gradio**, desde la cual es posible agregar, editar, eliminar y consultar registros, además de guardar la información generada en archivos CSV.

---

## Tecnologías utilizadas

* **Python**
* **Gradio** — Desarrollo de la interfaz gráfica.
* **Pandas** — Organización de datos y generación de archivos CSV.
* **CSV** — Almacenamiento de los registros y las proyecciones.

Las librerías principales utilizadas en el proyecto son `gradio` y `pandas`.

---

## Funcionamiento

El programa trabaja con dos estructuras principales:

* `registros`: almacena la información básica de cada CDT.
* `proyecciones`: almacena la proyección mensual correspondiente a cada registro.

Ambas listas se encuentran relacionadas mediante la posición del registro.

### Información de los registros

Cada registro contiene:

| Campo         | Descripción                                      |
| ------------- | ------------------------------------------------ |
| Registro      | Número identificador del CDT                     |
| Plazo         | Cantidad de meses del CDT                        |
| Monto inicial | Cantidad de dinero utilizada para iniciar el CDT |

### Información de las proyecciones

Cada proyección contiene:

| Campo       | Descripción                                      |
| ----------- | ------------------------------------------------ |
| Mes         | Número del mes de la proyección                  |
| Intereses   | Intereses generados durante el mes               |
| Monto final | Saldo acumulado después de aplicar los intereses |

---

## Cálculo de la tasa mensual

La tasa mensual se obtiene mediante la función `tasa_mensual(plazo)`:

```python
def tasa_mensual(plazo):
    return 0.001695 * plazo + 0.0983
```

La tasa utilizada depende del plazo seleccionado para el CDT.

---

## Validación de datos

Antes de realizar un registro, el programa verifica que:

* El monto inicial sea mayor que `0`.
* El plazo sea un número entero.
* El plazo sea mayor que `1` mes.

Si alguno de estos valores no cumple las condiciones, el programa genera un mensaje de error mediante Gradio.

También se valida que el número de registro utilizado para editar, eliminar o consultar exista dentro de los registros almacenados.

---

## Cálculo de la proyección

La función `calcular_proyeccion()` genera el crecimiento del CDT mes a mes.

El proceso comienza con el monto inicial en el mes `0`. Posteriormente, para cada mes se calcula el interés utilizando el saldo acumulado y la tasa mensual:

```text
Interés = Saldo actual × (Tasa mensual / 100)
```

Después, el interés se suma al saldo para obtener el nuevo monto acumulado.

La función almacena para cada mes:

```text
Mes
Intereses generados
Saldo acumulado
```

y devuelve todas las filas correspondientes a la proyección.

---

# Operaciones CRUD

La aplicación permite realizar las operaciones básicas de administración de registros.

## Agregar registro

La función `agregar()`:

1. Valida los datos ingresados.
2. Crea un nuevo número de registro.
3. Guarda el plazo y monto inicial.
4. Calcula la proyección completa del CDT.
5. Actualiza las tablas de la interfaz.

El número de registro se genera automáticamente utilizando la cantidad actual de registros.

---

## Editar registro

La función `editar()` permite modificar un registro existente.

Para realizar la operación se debe indicar:

* Número de registro.
* Nuevo monto inicial.
* Nuevo plazo.

Después de modificar los datos, se vuelve a calcular la proyección correspondiente al CDT.

---

## Eliminar registro

La función `eliminar()` elimina el registro seleccionado y también elimina su proyección asociada.

Después de la eliminación, los registros restantes son renumerados para mantener una numeración consecutiva.

---

## Consultar una proyección

La función `mostrar()` permite seleccionar un número de registro y consultar:

* La proyección mensual del CDT.
* Un resumen con el saldo final.
* La tasa mensual utilizada.
* El total de intereses generados.

El resumen se genera mediante la función `resumen()`.

---

# Interfaz gráfica

La interfaz fue desarrollada utilizando `gr.Blocks()` de Gradio.

La aplicación cuenta con:

### Datos del CDT

* Número de registro.
* Cantidad de dinero inicial.
* Plazo en meses.

### Operaciones

* **Agregar registro**
* **Editar registro**
* **Eliminar registro**
* **Guardar CSV**

### Consulta

* Número de registro que se desea mostrar.
* Botón para mostrar la proyección.
* Tabla con la proyección mensual.
* Resumen del CDT.

La interfaz utiliza componentes como `gr.Number`, `gr.Button`, `gr.Dataframe`, `gr.Markdown` y `gr.File`.

---

# Exportación de información

El programa permite guardar la información en dos archivos CSV:

### `registros_cdt.csv`

Contiene la información general de cada CDT:

```text
Registro
Plazo
Monto inicial
```

### `proyecciones_cdt.csv`

Contiene las proyecciones mensuales asociadas a cada registro:

```text
Registro
Mes
Intereses
Monto final
```

Los archivos son generados utilizando Pandas y codificación UTF-8.

---

# Estructura general del programa

El funcionamiento general puede representarse de la siguiente manera:

```text
                    ┌─────────────────────┐
                    │   Interfaz Gradio   │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
          Agregar           Editar          Eliminar
              │                │                │
              └────────────────┼────────────────┘
                               ▼
                    ┌─────────────────────┐
                    │      Registros      │
                    └──────────┬──────────┘
                               │
                               ▼
                    Calcular proyección
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Proyección CDT   │
                    └──────────┬──────────┘
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
               Visualización          Archivos CSV
```

---

# Ejecución

Para ejecutar el programa es necesario contar con Python y las librerías utilizadas:

```bash
pip install gradio pandas
```

Posteriormente, ejecutar el archivo Python:

```bash
python cdt_caso_estudio.py
```

La aplicación iniciará la interfaz de Gradio mediante:

```python
interfaz.launch(debug=True)
```

---

## Autor

**Diego Ardila Quintero**

Código: **01240372038**
