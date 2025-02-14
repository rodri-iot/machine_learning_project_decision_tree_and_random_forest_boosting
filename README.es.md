# Plantilla de Proyecto de Ciencia de Datos

Esta plantilla está diseñada para impulsar proyectos de ciencia de datos proporcionando una configuración básica para conexiones de base de datos, procesamiento de datos, y desarrollo de modelos de aprendizaje automático. Incluye una organización estructurada de carpetas para tus conjuntos de datos y un conjunto de paquetes de Python predefinidos necesarios para la mayoría de las tareas de ciencia de datos.

## Estructura

El proyecto está organizado de la siguiente manera:

- `app.py` - El script principal de Python que ejecutas para tu proyecto.
- `explore.py` - Un notebook para que puedas hacer tus exploraciones, idealmente el codigo de este notebook se migra hacia app.py para subir a produccion.
- `utils.py` - Este archivo contiene código de utilidad para operaciones como conexiones de base de datos.
- `requirements.txt` - Este archivo contiene la lista de paquetes de Python necesarios.
- `models/` - Este directorio debería contener tus clases de modelos SQLAlchemy.
- `data/` - Este directorio contiene los siguientes subdirectorios:
  - `interim/` - Para datos intermedios que han sido transformados.
  - `processed/` - Para los datos finales a utilizar para el modelado.
  - `raw/` - Para datos brutos sin ningún procesamiento.

## Configuración

**Prerrequisitos**

Asegúrate de tener Python 3.11+ instalado en tu máquina. También necesitarás pip para instalar los paquetes de Python.

**Instalación**

Clona el repositorio del proyecto en tu máquina local.

Navega hasta el directorio del proyecto e instala los paquetes de Python requeridos:

```bash
pip install -r requirements.txt
```

**Crear una base de datos (si es necesario)**

Crea una nueva base de datos dentro del motor Postgres personalizando y ejecutando el siguiente comando: `$ createdb -h localhost -U <username> <db_name>`
Conéctate al motor Postgres para usar tu base de datos, manipular tablas y datos: `$ psql -h localhost -U <username> <db_name>`
NOTA: Recuerda revisar la información del archivo ./.env para obtener el nombre de usuario y db_name.

¡Una vez que estés dentro de PSQL podrás crear tablas, hacer consultas, insertar, actualizar o eliminar datos y mucho más!

**Variables de entorno**

Crea un archivo .env en el directorio raíz del proyecto para almacenar tus variables de entorno, como tu cadena de conexión a la base de datos:

```makefile
DATABASE_URL="your_database_connection_url_here"
```

## Ejecutando la Aplicación

Para ejecutar la aplicación, ejecuta el script app.py desde la raíz del directorio del proyecto:

```bash
python app.py
# or
app_dt.ipynb
```

## Añadiendo Modelos

Para añadir clases de modelos SQLAlchemy, crea nuevos archivos de script de Python dentro del directorio models/. Estas clases deben ser definidas de acuerdo a tu esquema de base de datos.

Definición del modelo de ejemplo (`models/example_model.py`):

```py
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class ExampleModel(Base):
    __tablename__ = 'example_table'
    id = Column(Integer, primary_key=True)
    name = Column(String)

```

## Optimizando el modelo Arbol de decisiones

Hiper parametros para optimización del modelo

```py
hyperparams = {
    'criterion': ['gini', 'entropy', 'log_loss'],  # Función para medir la calidad de una división.
    'splitter': ['best', 'random'],  # Estrategia usada para dividir en nodos.
    'max_depth': [None, 10, 20, 30, 40, 50],  # Profundidad máxima del árbol.
    'min_samples_split': [2, 5, 10],  # Número mínimo de muestras necesarias para dividir un nodo.
    'min_samples_leaf': [1, 2, 5, 10],  # Número mínimo de muestras necesarias en un nodo hoja.
    'max_features': [None, 'sqrt', 'log2'],  # Número de características a considerar cuando se divide.
    'max_leaf_nodes': [None, 10, 20, 30, 40],  # Número máximo de nodos hoja.
    'min_impurity_decrease': [0.0, 0.01, 0.05],  # Umbral para una disminución mínima de la impureza.
    'ccp_alpha': [0.0, 0.01, 0.1],  # Parámetro de complejidad usado para el podado.
}
```

#### Explicación de los hiperparámetros
- `criterion`: Define la métrica para evaluar las divisiones.

   - 'gini': Gini Impurity (por defecto).
   - 'entropy': Información de ganancia usando la entropía.
   - 'log_loss': Entropía cruzada.
- `splitter`:

   - 'best': Escoge la mejor división.
   - 'random': Escoge la división aleatoriamente.
- `max_depth`: Limita la profundidad del árbol. Evita el sobreajuste. Un valor más bajo de max_depth simplifica el modelo.

- `min_samples_split`: Número mínimo de muestras requeridas para dividir un nodo interno.

- `min_samples_leaf`: Número mínimo de muestras que un nodo hoja debe tener.

- `max_features`: Número de características a considerar cuando se busca la mejor división. Puede ser:

   - None: Considera todas las características.
   - 'sqrt': Usa la raíz cuadrada del número de características.
   - 'log2': Usa el logaritmo en base 2 del número de características.
- `max_leaf_nodes`: Restringe el número de nodos hoja. Un valor más bajo limita la cantidad de nodos y ayuda a evitar el sobreajuste.

- `min_impurity_decrease`: Un nodo se dividirá si la disminución de impureza es mayor a este valor.

- `ccp_alpha`: Parámetro de poda para la complejidad del árbol.



## Optimizando el modelo Arbol de decisiones

Hiper parametros para optimización del modelo

```py
hyperparams = {
    'criterion': ['gini', 'entropy', 'log_loss'],  # Función para medir la calidad de una división.
    'splitter': ['best', 'random'],  # Estrategia usada para dividir en nodos.
    'max_depth': [None, 10, 20, 30, 40, 50],  # Profundidad máxima del árbol.
    'min_samples_split': [2, 5, 10],  # Número mínimo de muestras necesarias para dividir un nodo.
    'min_samples_leaf': [1, 2, 5, 10],  # Número mínimo de muestras necesarias en un nodo hoja.
    'max_features': [None, 'sqrt', 'log2'],  # Número de características a considerar cuando se divide.
    'max_leaf_nodes': [None, 10, 20, 30, 40],  # Número máximo de nodos hoja.
    'min_impurity_decrease': [0.0, 0.01, 0.05],  # Umbral para una disminución mínima de la impureza.
    'ccp_alpha': [0.0, 0.01, 0.1],  # Parámetro de complejidad usado para el podado.
}
```

#### Optimizando modelo RANDOM FOREST

```py
param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4],
    'max_features': ['sqrt', 'log2'],
    'bootstrap': [True, False]
}
```

#### **Hiperparámetros principales:**
- `n_estimators`:

   - Número de árboles en el bosque.
   - Valor por defecto: 100.
   - Ajustar este parámetro puede aumentar la precisión a costa de más tiempo de cómputo. Más árboles tienden a mejorar el rendimiento, pero con retornos decrecientes.

- `max_depth`:

   - Profundidad máxima de cada árbol.
   - Valor por defecto: Sin límite (es decir, cada árbol crece hasta que todas las hojas están puras o contienen menos muestras que min_samples_split).
   - Limitar la profundidad puede prevenir el sobreajuste.

- `min_samples_split`:

   - Número mínimo de muestras requeridas para dividir un nodo.
   - Valor por defecto: 2.
   - Un valor más alto previene que los árboles crezcan demasiado, lo que puede reducir el sobreajuste.

- `min_samples_leaf`:

   - Número mínimo de muestras que debe tener un nodo hoja.
   - Valor por defecto: 1.
   - Similar a min_samples_split, pero aplicado a las hojas. Aumentar este valor reduce el sobreajuste al hacer que las hojas sean más robustas.

- `max_features`:

   - Número máximo de características a considerar para encontrar la mejor división en cada nodo.
   - Valores posibles: "auto", "sqrt", "log2" o un número entero.
      - "auto" (o None) usa todas las características.
      - "sqrt" usa la raíz cuadrada del número total de características.
      - "log2" usa el logaritmo base 2 del número total de características.
   - Reducir max_features puede llevar a un modelo más robusto, especialmente con conjuntos de datos con muchas características.

- `bootstrap`:

   - Si se deben utilizar muestras de "bootstrap" (muestreo con reemplazo).
   - Valor por defecto: True.
   - Si se establece en False, se utilizan todos los datos sin muestreo de "bootstrap".

- `criterion`:

   - Función para medir la calidad de una división.
   - Para clasificación: "gini" (índice de Gini) o "entropy" (ganancia de información).
   - Para regresión: "mse" (error cuadrático medio) o "mae" (error absoluto medio).

- `max_samples`:

   - Si bootstrap=True, define el número máximo de muestras de entrenamiento que se dibujan para entrenar cada árbol.
   - Puede ser un número entero o un flotante que representa una fracción del total de muestras.

- `class_weight` (solo para clasificación):

   - Asigna pesos a las clases.
   - Opciones: None (sin peso), 'balanced' (ajusta automáticamente basado en las frecuencias de clase) o un diccionario con pesos personalizados.

#### **Otros hiperparámetros útiles:**

- `min_weight_fraction_leaf`:

   - Fracción mínima de la suma de los pesos de las muestras requerida para estar en un nodo hoja.
   - Valor por defecto: 0.

- `max_leaf_nodes`:

   - Número máximo de nodos hojas en el árbol.
   - Valor por defecto: None (sin límite).

- `n_jobs`:

   - Número de trabajos (núcleos de CPU) a ejecutar en paralelo.
   - Valor por defecto: None (usa un solo núcleo).
   - Puedes ajustar este valor a -1 para usar todos los núcleos disponibles y acelerar el entrenamiento.

- `random_state`:

   - Controla la aleatoriedad de la construcción del bosque y las particiones.
   - Útil para obtener resultados reproducibles.


## Optimizando modelo Boosting

```py
hyperparams = {
    'n_estimators': [100, 200, 300],
    'learning_rate': [0.01, 0.1, 0.2],
    'max_depth': [3, 5, 7],
    'subsample': [0.7, 0.8, 1.0],
    'colsample_bytree': [0.7, 0.8, 1.0],
    'gamma': [0, 0.1, 0.3],
    'reg_alpha': [0, 0.1, 0.5],
    'reg_lambda': [1, 1.5, 2]
}
```

Principales hiperparámetros en un modelo de Boosting
1. Número de árboles o estimadores (`n_estimators`)
   - Descripción: Controla el número total de árboles (o iteraciones) que se construyen en el proceso de boosting. Un número mayor puede mejorar el rendimiento, pero también incrementa el riesgo de sobreajuste y aumenta el tiempo de entrenamiento.
   - Valor recomendado: Generalmente, entre 100 y 500, pero depende del tamaño y la complejidad de los datos.
   - Impacto: Cuanto mayor sea el número, más complejo será el modelo.
2. Tasa de aprendizaje (`learning_rate`)
   - Descripción: Este hiperparámetro controla cuánto contribuye cada árbol al modelo final. Valores más bajos hacen que cada árbol contribuya menos, lo que requiere más árboles para lograr el mismo nivel de ajuste.
   - Valor recomendado: Comúnmente entre 0.01 y 0.3. Un valor bajo, como 0.1, es habitual, pero requerirá más árboles.
   - Impacto: Valores bajos reducen el riesgo de sobreajuste, pero pueden requerir un número mayor de árboles.
3. Profundidad máxima (`max_depth`)
   - Descripción: Establece la profundidad máxima de cada árbol. Los árboles más profundos permiten capturar relaciones más complejas, pero también pueden llevar a sobreajuste.
   - Valor recomendado: Entre 3 y 10 en la mayoría de los casos.
   - Impacto: Un valor más alto puede captar interacciones más complejas, pero puede llevar a sobreajuste si es demasiado alto.
4. Mínimo de muestras por hoja (`min_samples_leaf`)
   - Descripción: Especifica el número mínimo de muestras necesarias en una hoja final. Esto evita que el modelo aprenda demasiado de pequeñas variaciones en los datos.
   - Valor recomendado: Generalmente de 1 a 10.
   - Impacto: Valores más altos pueden evitar el sobreajuste y hacer que el modelo sea más generalizable.
5. Subsampleo (`subsample`)
   - Descripción: Controla la fracción de muestras que se utilizan para entrenar cada árbol. Un valor inferior a 1 introduce aleatoriedad en el entrenamiento, lo que ayuda a reducir el sobreajuste.
   - Valor recomendado: Generalmente entre 0.5 y 1.
   - Impacto: Mejora la robustez del modelo al introducir variabilidad entre los árboles.
6. Tasa de columnas (`colsample_bytree`)
   - Descripción: Controla la fracción de características que se seleccionan aleatoriamente para construir cada árbol.
   - Valor recomendado: Entre 0.5 y 1. En general, se prueba con diferentes valores para encontrar el equilibrio adecuado.
   - Impacto: Reduce la correlación entre árboles, lo que puede ayudar a reducir el sobreajuste.
7. Gamma (parámetro de regularización para XGBoost)
   - Descripción: Controla la reducción mínima en la función de pérdida para hacer una partición adicional en un nodo. Si la mejora no es suficiente, no se hace la partición.
   - Valor recomendado: Entre 0 y 5. Un valor de 0 significa que no hay restricciones, mientras que valores más altos refuerzan la poda.
   - Impacto: Ayuda a prevenir el sobreajuste ajustando la complejidad del modelo.
8. Alpha y Lambda (Regularización L1 y L2 en XGBoost)
   - Descripción: Son los parámetros de regularización para controlar el sobreajuste mediante penalizaciones L1 (alpha) y L2 (lambda).
   - Valor recomendado: Alpha se utiliza para la regularización L1 (similar a Lasso), y lambda para L2 (similar a Ridge). Los valores típicos son entre 0 y 1.
   - Impacto: Ayudan a reducir el sobreajuste al restringir la magnitud de los coeficientes.
9. Criterio de división (`criterion`)
   - Descripción: Especifica la métrica utilizada para seleccionar las mejores divisiones en cada árbol.
   - Opciones: gini, entropy (para clasificación), o mse, mae (para regresión).
   - Impacto: Dependiendo de la tarea (clasificación o regresión), la elección del criterio puede afectar el rendimiento del modelo.
10. Tasa de columnas por nivel (`colsample_bylevel`)
   - Descripción: Controla el subconjunto de características que se consideran en cada nivel del árbol.
   - Valor recomendado: Generalmente, se configura entre 0.5 y 1, donde 1 significa usar todas las características.
   - Impacto: Reduce la correlación entre las características y mejora la capacidad de generalización.

#### **Hiperparámetros específicos para LightGBM:**
   - `num_leaves`: Controla el número de hojas máximas en un árbol. Un valor más bajo puede evitar el sobreajuste.
   - `max_bin`: Controla el número máximo de bins usados para discretizar las características. Afecta la precisión y velocidad.
   - `min_child_samples`: El número mínimo de datos en una hoja, similar a min_samples_leaf.


## Trabajando con Datos

Puedes colocar tus conjuntos de datos brutos en el directorio data/raw, conjuntos de datos intermedios en data/interim, y los conjuntos de datos procesados listos para el análisis en data/processed.

Para procesar datos, puedes modificar el script app.py para incluir tus pasos de procesamiento de datos, utilizando pandas para la manipulación y análisis de datos.

## Contribuyentes

Esta plantilla fue construida como parte del [Data Science and Machine Learning Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/datascience-machine-learning) de 4Geeks Academy por [Alejandro Sanchez](https://twitter.com/alesanchezr) y muchos otros contribuyentes. Descubre más sobre [los programas BootCamp de 4Geeks Academy](https://4geeksacademy.com/us/programs) aquí.

Otras plantillas y recursos como este se pueden encontrar en la página de GitHub de la escuela.