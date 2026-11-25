# 🚢 Práctica 6: Identificación de Problemas en los Datos (Caso Titanic)

* **Clave de actividad:** `u3-act6-titanic-0028`
* **Dataset analizado:** Pasajeros del Titanic (`titanic.csv` — 891 registros, 15 columnas)
* **Herramientas:** Python, Pandas, NumPy, Matplotlib, Seaborn

---

## 📊 Resultados de la Auditoría de Datos

Al ejecutar el script `p6-titanic-0028.py`, se detectaron los siguientes problemas en el conjunto de datos original:

### 1. Datos Nulos (Faltantes)
| Columna | Cantidad de Nulos | Porcentaje | Estrategia aplicada |
| :--- | :---: | :---: | :--- |
| **`deck`** | 688 | 77.22 % | Se conserva sin imputar en limpieza básica (demasiados faltantes). |
| **`age`** | 177 | 19.87 % | Imputación con la **mediana** (`28.0` años) para no sesgar por extremos. |
| **`embarked`** | 2 | 0.22 % | Imputación con la **moda** (`S`). |
| **`embark_town`** | 2 | 0.22 % | Imputación con la **moda** (`Southampton`). |

### 2. Registros Duplicados
* Se encontraron **107 filas duplicadas** exactas (debido a que el dataset de Seaborn no incluye identificador único de pasajero y algunos pasajeros de tercera clase comparten exactamente los mismos atributos). Se eliminaron mediante `drop_duplicates()`.

### 3. Tipos de Datos
* Todas las columnas numéricas (`survived`, `pclass`, `age`, `sibsp`, `parch`, `fare`) tienen tipos `int64` o `float64` adecuados, por lo que no fue necesario convertir cadenas de texto a números.

### 4. Valores Atípicos (Outliers) en Edad
* Usando el método del Rango Intercuartílico ($IQR = Q3 - Q1$), el límite superior esperado fue de **64.81 años**.
* Se detectaron **11 valores atípicos** superiores a ese umbral (edades entre 65 y 80 años). Al ser edades humanas reales y posibles, **no se eliminaron**, ya que representan pasajeros adultos mayores legítimos.

---

## 📝 Respuestas al Cuestionario de Evaluación

1. **¿Qué es un dato nulo y por qué le "molesta" a un algoritmo de Machine Learning?**  
   Es la ausencia de un valor registrado (`NaN` o vacío). La mayoría de los algoritmos matemáticos de Machine Learning no pueden realizar operaciones aritméticas con valores indefinidos, lo que provoca errores de ejecución o sesgos si se ignoran sin criterio.

2. **¿Qué diferencia práctica hay entre un dato nulo y un dato duplicado?**  
   Un dato nulo es información que falta en una celda; un dato duplicado es un registro completo repetido que hace que el modelo le dé doble peso (sobreajuste) a un mismo caso.

3. **¿Qué función de Pandas nos da un resumen rápido de cuántos nulos hay por columna?**  
   `df.isnull().sum()` (o `df.isna().sum()`).

4. **Si `df.dtypes` muestra que la columna "Salario" es `object` en lugar de `float`, ¿qué problema hay?**  
   Significa que Pandas la está interpretando como texto (cadenas de caracteres), probablemente porque contiene símbolos (`$`, comas) o palabras mezcladas, impidiendo hacer cálculos estadísticos o entrenar un modelo con ella.

5. **¿Qué hace exactamente `errors="coerce"` dentro de `pd.to_numeric()`?**  
   Fuerza la conversión de los valores a formato numérico y transforma automáticamente cualquier texto o carácter inválido que no pueda convertirse en `NaN` (valor nulo), evitando que el programa se detenga con un error.

6. **Define con un ejemplo qué es un valor atípico (outlier).**  
   Es una observación que se aleja drásticamente del comportamiento normal del resto de los datos. Por ejemplo, registrar una edad de `250` años en un censo humano o una altura de `1000 cm` en una flor Iris.

7. **¿Por qué NO debemos eliminar automáticamente todos los valores atípicos sin investigarlos primero?**  
   Porque algunos valores extremos no son errores de captura, sino casos reales poco frecuentes (como un pasajero de 80 años en el Titanic o una compra corporativa muy grande). Eliminarlos sin revisar haría que el modelo pierda capacidad para predecir casos reales importantes.

---

## 🌟 Conclusión
El principio *"Garbage in, garbage out"* demuestra que la calidad de un modelo de Machine Learning depende directamente de la calidad de los datos con los que se entrena. Auditar nulos, duplicados, tipos y valores atípicos antes de entrenar evita predicciones erróneas y asegura resultados confiables.