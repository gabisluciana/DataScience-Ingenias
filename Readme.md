# Data Sciece a partir de Encuesta Mundial de Salud Escolar

## Grupo 9: Integrantes, dataset y objetivos
### Integrantes:
- Cruz, Ruth
- Gabis Leccesi, Luciana
- Palma, Malena Agustina
- Stachoni, Yanina

### Dataset seleccionado:
- Resultados encuesta EMSE
- Origen de datos: https://datos.gob.ar/ar/dataset/salud-base-datos-3deg-encuesta-mundial-salud-escolar-emse-con-resultados-nacionales-argentina
- Dataset: 
- - [Comprimido](/Segunda%20Entrega/emse_datosabiertos.zip)
- - [Completo](/Segunda%20Entrega/EMSE_DatosAbiertos2.csv)

La Encuesta Mundial de Salud Escolar (EMSE) utiliza una metodología estandarizada a nivel mundial para relevar datos sobre aspectos sociodemográficos e indaga sobre conductas relacionadas con el comportamiento alimentario, el consumo de alcohol y otras drogas, la violencia y daños no intencionados, la seguridad vial, la salud mental, el con- sumo de tabaco, la actividad física, los comportamientos sexuales y los factores de protección

### Objetivos:
* La EMSE abarca muchas temáticas con más de 70 preguntas. Para nuestro estudio decidimos enforcarnos en temas de violencia y daños no intencionados, persiguiendo los siguientes objetivos:
* * Detectar patrones en niños y niñas, considerando relaciones familiares y con amigos, datos físicos y sociodemográficos.
  * Predecir victimas de intimidación escolar.


## Análisis exploratorio de datos:
En [esta notebook](/Segunda%20Entrega/Segunda_entrega.ipynb) se encuentra el análisis exploratio, tratamiento de datos nulos, outliers y gráficos. En ella se realizaron las siguientes tareas:

1) Como primera medida se analizaron todas las preguntas de la encuensta y se decidió cuáles eran de interés para nuestros objetivos. Mediante [este formulario](/Segunda%20Entrega/Analisis%20Preguntas.xlsx) se configuraron las columnas a importar y sus respectivos nombres de columas.

2) Luego se procedió a tratar los datos nulos, reemplazando el valor `Dato perdido` por `nan` 

3) Se reemplazaron los datos nulos por la mediana o el más frecuente según corresponda. 

4) Se eliminaron outliers de la columna `peso`.

5) Se eliminaron las columnas calculadas `obeso`, `bajo_peso` y `sobre_peso`.

6) Se convirtieron a numérico todas las columnas, con distintas técnicas como:
    - Expresiones regulares para extraer valor numérico.
    - Si y No por 0 y 1
    - `Sexo` por columnas `Masculino` y `Femenino`
    - Valores con frecuencia de `Nunca` a `Siempre` por valores de 0 a 4.
    - LabelEncoder() para el resto de columnas.

7) Se realizaron cálculos y gráficos para obtener información de los datos.

### Conclusiones:
El análisis de los datos obtenidos de la Encuesta Mundial de Salud Escolar revela una preocupante prevalencia de bullying entre los estudiantes. De un total de 34.878 encuestados, un alarmante **65,82%** reportó haber sido **víctima de intimidación**. Dentro de este grupo de víctimas, el **59.6%** son mujeres.
Se detectó tambien que en el contexto escolar, el **60%** de las víctimas se siente solo y el **56,6%** no tienen o rara vez tienen ayuda de sus padres para realizar o controlar tarea.

Se concluye que **es crucial implementar programas de prevención y concientización en las escuelas que no solo aborden el bullying sino que traten otras problemáticas que pueden desencadenarlo.**

## Modelo Supervisado:
En [esta notebook](/Tercer%20Entrega/Tercer_entrega.ipynb) se crearon modelos supervisados con el objetivo de **detectar víctimas de intimidación escolar**. Por lo tanto:
- La **variable dependiente** es **intimidacion_escuela**, que solo toma dos valores "Si" y "No".
- El tipo de **aprendizaje** es **supervisado** ya que contamos con **datos etiquetados** (víctima / no víctima)
- Nuestro problema es de **clasificación binaria** ya que tenemos dos posibles clases:
    - Víctima de intimidación escolar.
    - No víctima de intimidación escolar.

1) Se realizaron los siguientes modelos:

    * **KNN**: Con una exactitud general (accuracy) del 77,68%. Pero con ciertos problemas como el **desbalance de clases** ya que el 79,3% de los datos eran de una clase ("No Víctima"), **alto número de Falsos Negativos** (18,63% del total de muestras) y un **muy bajo recall (0.10) y precisión (0.36)** para la clase Víctima.

    * **Random Forest**: Con una mejor exactitud general (accuracy) del 82,42%. Pero con ciertos problemas como el **desbalance de clases** ya que el 79,3% de los datos eran de una clase ("No Víctima"), **alto número de Falsos Negativos** (15% del total de muestras) y para la clase Víctima, un **muy bajo recall (0.27)** y **baja precisión (0.69)**.

2) Se realizó un balanceo de clases mediante SMOTE

3) Se realizó búsqueda de los mejores hiperparámtros para los dos modelos con los siguientes resultados:

    * **KNN**: Con una exactitud general (accuracy) del 82,78%. Y unas excelentes métricas para la clase Víctima. **recall de 0.98** y **precisión de 0.75**

    * **Random Forest**: Con una exactitud general (accuracy) del 86,44%. Y unas buenas métricas para la clase Víctima. **recall de 0.84** y **precisión de 0.88**

4) **Conclusiones**
Si bien Random Forest tiene un mejor equilibrio en las métricas de rendimiento y una mayor precisión general, **recomendamos elegir KNN** ya que nuestro objetivo es identificar a las víctimas de bullying para implementar medidas preventivas. Su capacidad para detectar casi todas las instancias de "Víctima" (alto recall) es fundamental para asegurar que se tomen las medidas adecuadas para ayudar a los estudiantes que están en riesgo.

5) Se realizó validación cruzada para confirmar que el modelo generaliza bien a nuevos datos.

6) También se realizó análisis de pesos de las variables para poder orientar en la creación de programas de intervención específicos.

## Modelo No Supervisado:
Se realizaron varios modelos de clustering, con el **objetivo** de **Segmentar a los estudiantes en grupos basados en características similares, como hábitos alimenticios, actividad física, consumo de sustancias, y salud mental**. Esto permitiría a los responsables de políticas diseñar intervenciones más específicas y dirigidas a cada grupo identificado. 

Primeramente se decidió trabajar con todas las columnas del dataset original por lo que se realizaron las siguientes tareas:

1) Renombrar columnas para mejor entendimiento de los datos.
2) Tratamiento de nulos y outliers siguiendo los lineamientos del análsis exploratorio inicial.
3) Conversión a numérico de todas las columnas categóricas, siguiendo los mismos lineamientos establecidos con anterioridad


Luego se procedió a realizar una **reducción de dimensionalidad** quedando 36 componentes principales que explican el 70% de los datos.

Los modelos de aprendizaje no supervisado son:

* **[DBSCAN](/Cuarta%20Entrega/Cuarta%20entrega%20(DBScan).ipynb)**
En este caso se buscó la mejor combinación posible de los hiperparámetros `eps` y `min_samples`. Ya desde este punto se detectó algo extraño, ya que el gráfico del codo está sobre valores de epsilon muy grandes, lo que indica que varios puntos muy distantes serán tomados como parte del mismo clúster. Se probó de todas formas varias combinaciones con esos valores sugeridos y con otros menores. Pero se **concluye que los datos no tienen una distribución adecuada para este método** ya que con todas las variantes posibles no se logró formar clústers con sentido.

* **[K-means (k = 2)](/Cuarta%20Entrega/Cuarta%20entrega(K-Means%20k=2).ipynb)** guiándonos por el método de Silhouette para determinar k. En este caso se encontraron dos clústeres bien diferenciados.

El **Clúster 0** agrupa principalmente a estudiantes de 11 a 15 años que no consumen alcohol, no se sienten solos, no han pensado en el suicidio, no han fumado marihuana y reciben apoyo de sus padres con las tareas.

Por otro lado, el **Clúster 1** se compone mayoritariamente de adolescentes de 16 a 18 años que han experimentado soledad, han consumido alcohol, han considerado el suicidio y, en menor medida, han consumido marihuana, con menos apoyo parental en tareas escolares.

* **[K-means (k = 6)](/Cuarta%20Entrega/Cuarta%20entrega(K-Means%20k=6).ipynb)** con k a partir del método del codo. En este caso los 6 clústers que se formaron no tenían una distribución clara ni estaban balanceados. El clúster 0, 3 y 4 tenían pocos individuos mientras los otros 3 concentraban la mayor cantidad de datos.

**Conclusiones:**
Consideramos apropiado utilizar el modelo K-means con un k de 2 que nos permite clasificar a los individuos encuestados entre aquellos que requieren mayor supervisión, talleres y otras acciones para abordar sus problemáticas como el consumo de sustancias, embarazos, sentimientos de soledad o pensamientos suicidas y aquellos que cuentan con apoyo de su familia y no presentan mayores problemas de comportamiento.


