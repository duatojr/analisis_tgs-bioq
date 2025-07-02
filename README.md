# README

Este repositorio contiene el código R utilizado para el análisis de un estudio longitudinal que investiga la relación entre perfiles genéticos y marcadores bioquímicos en futbolistas profesionales. El objetivo principal es identificar asociaciones significativas que puedan influir en el rendimiento, la prevención de lesiones y la salud de los atletas de élite.

Bases del Estudio

El rendimiento de los deportistas de élite es una interacción compleja de factores genéticos y ambientales. En el fútbol profesional, la monitorización continua de la fisiología de los jugadores es crucial para optimizar el entrenamiento y prevenir lesiones. Este estudio longitudinal se propuso identificar la relación entre perfiles genéticos específicos (predisposición eficiente de rendimiento muscular, hepáticos, cardiorrespiratorios y de eficiencia metabólica) y marcadores bioquímicos clave en futbolistas profesionales de equipos de élite, con datos registrados en seis momentos diferentes de la temporada.

Para cuantificar la predisposición genética de cada perfil, se utilizó una Puntuación de Genotipo Total (TGS). Los resultados obtenidos revelan relaciones significativas, con variaciones observadas según el momento de la temporada. Estas asociaciones sugieren que la genética de los atletas influye directamente en su capacidad de adaptación y recuperación frente a las demandas del entrenamiento y la competición. Este trabajo busca proporcionar herramientas para la medicina deportiva personalizada, permitiendo ajustar estrategias de entrenamiento y nutrición.

El estudio se realizó bajo un diseño longitudinal y observacional, con muestras bioquímicas recolectadas en seis momentos distintos de una temporada competitiva de fútbol: pretemporada, inicio, primera parte, mitad, segunda mitad y final de temporada. Las variables bioquímicas analizadas incluyeron Hierro (FE), Creatina Quinasa (CPK), Alanina Aminotransferasa (GPT), Urea, Nitrógeno Ureico en Sangre (BUN), Gamma-Glutamil Transferasa (GGT) y HMTCO.

Se establecieron cuatro perfiles genéticos basados en polimorfismos funcionales asociados con el rendimiento deportivo: Perfil Muscular, Perfil Hepático, Perfil Cardiorrespiratorio y Perfil de Eficacia Metabólica.

Base de datos

El conjunto de datos utilizado en este estudio, cargado desde el archivo `datos_sporting_completos.xlsx`, está organizado de la siguiente manera:

* Formato Ancho: La base de datos se encuentra en un formato "ancho". Esto significa que cada fila representa un sujeto individual (un jugador de fútbol), y las columnas contienen las diferentes variables y sus mediciones.

* Identificador Único del Sujeto: Existe una columna denominada `SUJETO` que actúa como identificador único para cada jugador.

* Variables de Puntuación de Genotipo Total (TGS): Se incluyen columnas para diferentes perfiles genéticos de Puntuación de Genotipo Total (TGS), como por ejemplo:
    * `%TGS muscular`
    * `%TGS hepático`
    * `%TGS cardio`
    * `%TGS eficacia`

* Variables Bioquímicas Longitudinales: Para cada marcador bioquímico medido, existen seis columnas distintas. Cada una de estas columnas representa una toma de medición realizada en un momento específico de la temporada (1º, 2º, 3º, 4º, 5º y 6º toma). Los nombres de estas columnas siguen un patrón de `[NOMBRE_BIOQUIMICA][NUMERO_TOMA]º`.

    Algunos ejemplos de estas variables incluyen:
    * `FERRIT1º`, `FERRIT2º`, ..., `FERRIT6º` (para Ferritina)
    * `FE1º`, `FE2º`, ..., `FE6º` (para Hierro)
    * `CPK1º`, `CPK2º`, ..., `CPK6º` (para Creatina Quinasa)

En resumen, la base de datos es una tabla donde cada fila es un jugador, y las columnas registran sus puntuaciones de perfiles genéticos y las mediciones de diversas variables bioquímicas en seis momentos diferentes a lo largo de una temporada.

Paquetes de R Empleados

El análisis de este estudio se llevó a cabo utilizando los siguientes paquetes de R:

  * `readxl`: Para la lectura de archivos Excel.
  * `dplyr`: Para la manipulación y transformación de datos.
  * `ggplot2`: Para la creación de gráficos de alta calidad.
  * `tidyr`: Para la transformación de datos de formato ancho a largo y viceversa.
  * `skimr`: Para resúmenes concisos de datos.
  * `GGally`: Para la creación de matrices de gráficos, útil en la exploración de pares de variables.
  * `corrplot`: Para la visualización de matrices de correlación.
  * `stringr`: Para la manipulación de cadenas de texto.
  * `tidyverse`: Un metapackage que incluye `dplyr`, `ggplot2`, `tidyr`, `readr`, `purrr`, `stringr`, y `forcats`.
  * `FactoMineR`: Para análisis multivariante.
  * `factoextra`: Para visualizar resultados de análisis multivariante.
  * `plotly`: Para gráficos interactivos.
  * `ggcorrplot`: Para visualizar matrices de correlación con `ggplot2`.
  * `pROC`: Para generar y analizar curvas ROC.

Asegúrate de tener estos paquetes instalados en tu entorno de R. Puedes instalarlos usando `install.packages("nombre_del_paquete")`.

Versión de R

La versión de R empleada es R 4.4.3; mientras que la versión de RStudio utilizada es 2025.05.0+496

Resumen del Código (`analisis_tgs-bioq.Rmd`)

El script `analisis_tgs-bioq.Rmd` está diseñado para realizar un análisis exhaustivo de los datos bioquímicos y genéticos. A continuación, se detalla cómo actúa el código:

1.  Carga de Librerías y Datos:

      * Al inicio, el script carga todas las librerías mencionadas anteriormente.
      * Carga los datos desde un archivo Excel (`datos_sporting_completos.xlsx`). Es importante que este archivo esté ubicado en la ruta especificada en el script o que la ruta sea actualizada.

2.  Función `analizar_bio_tgs_jugador`:

      * Esta es la función principal que encapsula el análisis. Toma como argumentos el dataframe de datos, el nombre de la columna de ID de sujeto, el nombre de la variable TGS y el nombre base de la variable bioquímica (ej. "Creatina", "GOT").
      * Validación de Entradas: Realiza comprobaciones para asegurar que los argumentos de entrada sean válidos y que las columnas requeridas existan y sean del tipo correcto.
      * Correlaciones Individuales: Calcula la correlación de Pearson entre la variable TGS y cada una de las 6 tomas temporales de la variable bioquímica seleccionada, manejando los valores `NA`.
      * Matriz de Correlación: Genera una matriz de correlación entre la variable TGS y todas las tomas de la variable bioquímica, y la visualiza utilizando `ggcorrplot`.
      * Gráfico de Trayectorias Individuales: Transforma los datos a un formato largo para crear un gráfico de líneas que muestra la evolución de la variable bioquímica para cada sujeto a lo largo de las seis tomas, coloreadas por el valor de su TGS. Se añade una línea de tendencia roja que representa la media.
      * Detección de Outliers: Calcula cuartiles y límites para la detección de valores atípicos (outliers) en cada toma de la variable bioquímica.
      * Curvas ROC: Se generan curvas ROC para determinar la capacidad predictiva de los perfiles genéticos sobre las variables bioquímicas, utilizando el Área Bajo la Curva (AUC) como métrica.

3.  Ejecución de la Función para Variables Bioquímicas:

      * El script llama a la función `analizar_bio_tgs_jugador` para diversas variables bioquímicas como GOT, GPT, GGT, CPK, FE, FERRITINA, HMTCO y HB, generando resultados específicos para cada una.

