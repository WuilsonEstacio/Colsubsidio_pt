<div align="center">

# Desarrollo prueba técnica Colsubsidio

**Responsable:** Wuilson Adolfo Estacio Rojas<br/>  
**Analista de incorporación:** Yeimmy Verónica Bustos Moreno<br/> 
**Jefe gestión de la información:** Camilo Garzón Márquez<br/> 
**Fecha:** 15-09-2025  

</div>


# Prueba Tecnica Cientifico de Datos - Colsubsidio 

Este documento describe el proceso seguido para **explorar** los datos, **identificar** variables relevantes y **definir** un modelo heurístico para solucionar cada uno de los puntos planteados en la prueba tecnica.


## Tabla de Contenido
1. [Introducción](#introducción)  
2. [Objetivo](#objetivo)  
3. [Modelo de Fuga](#modelo-de-fuga)  
   - 3.1 [Exploración y Evaluación de Datos (EDA)](#eda)  
     - 3.1.1 [Data Quality](#data-quality)  
     - 3.1.2 [Estadísticas y descriptivos](#estadísticas-descriptivas)  
   - 3.2 [Definición del Modelo](#Definición-del-Modelo)
     - 3.2.1 [Flujo Datos](#Flujo-Datos)
     - 3.2.3 [Entrenamiento del modelo](#Entrenamiento_del_Modelo_de_Fuga)
4. [Ejercicios de numeral Dos](#ejercicios-de-numeral-dos)  
5. [Análisis de punto 3](#análisis-de-punto-3)
 - 5.1.1 [Interpretación técnica de la matriz](#Interpretación_técnica_de_la_matriz)
 - 5.1.2 [Cuáles y cuántas métricas puede obtener de dicha matriz](#Metricas_para_dicha_Matriz) 



## introduccion

### ¿Qué es Colsubsidio?

Es una **Caja de Compensación Familiar en Colombia**, entidad privada, de naturaleza social y sin ánimo de lucro, que administra recursos del subsidio familiar y presta servicios a trabajadores y sus familias en distintos sectores.  

Colsubsidio fue fundada en **1957**, por lo que lleva alrededor de **68 años** en funcionamiento en Colombia.  

Su objetivo principal es **mejorar la calidad de vida de los trabajadores afiliados y sus familias**.  
Para ello ha dividido la prestación de sus servicios en **once Unidades Especializadas de Servicio (UES)**.  

Una de ellas, la **UES de Crédito**, atiende las necesidades de la población (afiliados y no afiliados) a través de productos crediticios como: **cupo, consumo e hipotecario**, a nivel nacional.  

Pero existen otras UES, como se muestra en la siguiente imagen:  

<p align="center">
  <img src="./Imagenes/UES_Colsubsidio.png" alt="UES Disponibles" title="UES Disponibles" width="700"/>
</p>

---

## Objetivo
El objetivo de la prueba es idear una solución para el punto uno que es el Modelo de Fuga dado en el documento Data_Scientist_Test_2023.html el cual tiene un  peso del 75%, tambien la solucion de cualquiera de los puntos del numeral dos con un peso del 15% y finalizar con el desarrollo del numeral 3 con un peso del 10%


---

# Modelo-de-fuga

### Objetivo: En este caso, se requiere establecer la probabilidad que un cliente en una de sus líneas de negocio (Crédito) se vaya o no. Para esto se tiene un histórico de datos de los clientes que han solicitado un tipo de retiro y los que no lo han expresado.

## 📑 Columnas Relevantes

### BASE: train

- **id** → Identificación Única  
- **Fecha.Expedicion** → Fecha expedición del cupo  
- **Cancelacion** → Tipo Cancelación  
- **Gestionable** → Describe si es posible gestionar o no la identificación (*Gestionable / No Gestionable*)  
- **Retencion** → Se realizó una retención o no *(No cuenta para test)*  
- **TIPO** → Tipo de tarjeta  
- **ANO_MES** → Año mes de la intención de cancelación  
- **Target** → Variable Objetivo o Target *(No cuenta para test)*  
- **Fecha.Proceso** → Fecha análisis de la base  
- **Disponible.Avances** → Saldo disponible para avances de efectivo  
- **Limite.Avances** → Saldo límite para avances de efectivo  
- **Total.Intereses** → Total intereses pagados mes análisis  
- **Saldos.Mes.Ant** → Saldo del mes anterior  
- **Pagos.Mes.Ant** → Valor en COP de los pagos del mes anterior  
- **Vtas.Mes.Ant** → Consumo mes anterior  
- **Edad.Mora** → Días en mora  
- **Limite.Cupo** → Cupo límite  
- **Pago.del.Mes** → Valor en COP de los pagos del mes  
- **Pago.Minimo** → Valor en COP del pago mínimo  
- **Vr.Mora** → Valor en COP de la mora  
- **Vr.Cuota.Manejo** → Valor de la cuota de manejo  
- **Saldo** → Saldo disponible en el mes de análisis  

---

### BASE: test_demograficas

- **id** → Identificación Única  
- **categoria** → Categoría de afiliado a la caja Colsubsidio  
- **segmento** → Segmento poblacional *(según modelo analítico desarrollado en la caja)*  
- **edad** → Edad en años  
- **nivel_educativo** → Nivel educativo *(Sin definir, primaria, secundaria, profesional, posgrado)*  
- **estado_civil** → Estado civil actual  
- **Genero** → Género  
- **PAC** → Número de personas a cargo  
- **contrato** → Tipo de contrato *(1: Fijo, 2: Indefinido, 3: Prestación Servicios, 4: Independiente)*  
- **estrato** → Estrato socioeconómico  

---

### BASE: test_subsidio

- **cuota_monetaria** → (1: Tiene derecho a cuota monetaria, 2: No tiene derecho)  
- **sub_vivenda** → (1: Ha solicitado y desembolsado subsidio de vivienda, 2: No ha solicitado)  
- **bono_lonchera** → (1: Tiene derecho a Bono Lonchera, 2: No tiene derecho)  
---


## EDA
Para este modelo de fuga inicialmente para ello cargamos los datos de train (train_test_demograficas,train_test_subsidios, train) para la exploracion de los datos, con un total de 50001 clientes unicos o Id unicos, esta exploracion se realiza en el cuaderdo de python Modelo de Fuga -eda

### Data-Quality
1 Valores nulos se identificaron solo en la siguientes colunas:
- Gestionable:           48589   →   0.97
- Cancelacion:           48589   →   0.97
- ANO_MES:               48589   →   0.97
- TIPO:                  48589   →   0.97 
- Retencion:             48589   →   0.97
- estrato:               45885   →   0.97
- aqui claramente observamos el tipo de columna, su cantidad en nulos y su porcentaje respecto al total de los datos.
  
2 Duplicados
- No se encontraron Id duplicados
- No se encontraron casos donde Fecha.Proceso < Fecha.Expedicion

### Estadísticas-descriptivas
De los datos se encontraron 1412 cancelaciones, iniciando con una en 2017-01-01, de fecha maxima en 2018-03-01, donde los motivos mas frecuentes tipos de cancelacion son los siguientes
<p align="center">
  <img src="./Imagenes/top 10 Motivos Cancelacion1.png", title="top 10 Motivos Cancelacion" width="700"/>
</p>

- **TIPO (`Tipo de Tarjeta que tenian quienes se fueron`)**  
  - > Tipo de tarjeta: Cupo → 1106, Amparo → 306

- **Retencion (`Se realizó una retención o no`)**
  - > Tipo Retencion:  no efectiva  → 1239, efectiva →  173
     
- **Genero (`Con que genero se identifica el cliente Hombre o Mujer`)**
  - > Tipo Genero:  Masculino → 25126 → 0.50% , Femenino → 24875 → 0.49%

- **Edad (`Edad de los clinetes`)**
  - Máximo: 65
  - Mínimo: 18
  - Promedio:  41 
  - Desviación Estándar: ~13

- **PAC (`Número de personas a cargo`)**
  - Máximo: 63
  - Mínimo: 0
  - Promedio:  2 
  - Desviación Estándar: ~0.1

- **Meses De Duracion antes de churn (`Número de meses que duro la persona afiliada antes de churn`)**
  - Máximo: 138
  - Mínimo: 0
  - Promedio:  27
  - Mediana:   16
  - q1:   8
  - q3:   39 

- **Dias de la semana que mas realizaron churn**
- De las bajas organizadas por dia podemos ver que el dia domingo es el dia que mayor presenta bajas
<p align="center">
  <img src="./Imagenes/Bajas por dia.png", title="Bajas por dia" width="600"/>
</p>


- **Bajas por mes (`Cantidad de clientes que se fueron de a cuerdo al mes`)**
- De las bajas organizadas por mes podemos ver que el mes de diciembre presenta mas casos
<p align="center">
  <img src="./Imagenes/Bajas por mes.png", title="Bajas por mes" width="600"/>
</p>

### Análisis de bajas por mes

## Lo que muestran los datos

- **Enero 2017 (157)** y **diciembre 2017 (181)** fueron los meses con más bajas.  
- Entre **febrero y noviembre de 2017**, las salidas se mantuvieron estables dentro del rango **80–116**.  
- En **2018**, los meses medidos (febrero con 71 y marzo con 77) aún muestran cifras relevantes, aunque ligeramente por debajo del promedio de 2017.  
- El mínimo fue **81 en abril 2017** y el máximo **181 en diciembre 2017**, casi el doble de diferencia entre un mes y otro.

## Posibles patrones

- **Estacionalidad / fin de año:**  
  Diciembre marca un pico que puede relacionarse con gastos de temporada, ajustes financieros o vencimientos de contratos/subsidios.  
  Enero también aparece alto, lo que podría ser un efecto rezagado del mismo ciclo.  

- **Estabilidad en el resto del año:**  
  Entre febrero y noviembre de 2017, las bajas se mantuvieron bastante homogéneas (alrededor de **85–110**).  
  Esto sugiere que, fuera de los picos de inicio y cierre de año, la salida de clientes sigue un patrón constante.  

- **Tendencia en 2018:**  
  En los datos disponibles, los primeros meses de 2018 muestran una **leve reducción** comparados con el promedio de 2017, aunque aún dentro del rango de los meses más bajos del año anterior.  
  Esto podría indicar ajustes en políticas o mejoras en la retención de clientes.

- **Nota** segun los datos y teniendo una vista general a ellos nos percatamos que la mayor cantidad es estos estan agrupados en la fecha 2018-04 con un total de 48589 lo cuales mas del 90% de los datos agrupados en una sola fecha.
  
---

- **Distribución de Género y nivel_educativo (`Distribución de Género y nivel_educativo`)**
<p align="center">
  <img src="./Imagenes/Distribución de Género y nivel_educativo.png" alt="Distribución de Género y nivel_educativo" title="Distribución de Género y nivel_educativo" width="600"/>
</p>

## Hallazgos principales de acuerdo a Distribución de Género y nivel_educativo

### Diferencias por género

**Mujeres (F):**
- Mayor proporción en **primaria** (340 casos).  
- Le siguen **técnico/tecnológico** (273) y **secundaria** (71).  

**Hombres (M):**
- También dominan en **primaria** (397 casos), que es incluso mayor que en mujeres.  
- Luego **técnico/tecnológico** (260) y **secundaria** (71).  

**Patrón común:**  
En ambos géneros se mantiene la jerarquía:  
`primaria > técnico/tecnológico > secundaria`

---

### Concentración en nivel educativo bajo
- En ambos géneros, el grueso de las salidas ocurre en personas con **primaria**.  
- Esto sugiere que el **nivel educativo podría estar asociado a mayor probabilidad de salida**, reflejando posible menor estabilidad o menor retención.  

---

### Comparación de género en el mismo nivel educativo
- En **primaria**, los hombres que se fueron (397) superan a las mujeres (340).  
- En **técnico/tecnológico**, las mujeres tienen una ligera ventaja (273 vs 260).  
- En **secundaria**, ambos géneros presentan exactamente la misma cantidad (71).  

---

### Implicaciones estadísticas
- Existe un **patrón homogéneo de abandono según nivel educativo**: la **primaria concentra la mayor pérdida** en ambos géneros.  
- La diferencia más visible se da en **primaria**, con hombres más afectados que mujeres.  
- Los niveles educativos más altos (**técnico/tecnológico**) no eliminan la salida, pero presentan cifras menores que primaria.  


## **Diagnostic Plots (`Diagnostico de variables continuas`)**
<p align="center">
  <img src="./Imagenes/Diagnostic1.png", title="Diagnostic1" width="600"/>
</p>

<p align="center">
  <img src="./Imagenes/Diagnostic2.png", title="Diagnostic2" width="600"/>
</p>

<p align="center">
  <img src="./Imagenes/Diagnostic3.png", title="Diagnostic3" width="600"/>
</p>


## Análisis Estadístico de Variables

### 🔹 Edad
- Distribución bastante uniforme entre **20 y 65 años**.  
- **Media y mediana ≈ 40 años** → población balanceada.  
- **Skew ≈ 0** y **curtosis negativa** → no hay colas largas ni concentración fuerte.  
- Variable relativamente estable.  

---

### 🔹 Saldo y Saldos.Mes.Ant
- Claramente **asimétricos a la derecha** (Skew ~ 6, curtosis > 60).  
- La mayoría presenta saldos bajos, pero existen pocos casos con montos muy elevados.  
- En el **boxplot** se observa gran cantidad de **outliers sobre el percentil 75**.  

---

### 🔹 Pagos.Mes.Ant y Vtas.Mes.Ant
- Presentan **sesgo extremo** (Skew > 20, curtosis > 1000).  
- La gran mayoría de usuarios tiene consumos/pagos bajos.  
- Unos pocos concentran **montos millonarios**.  

---

### 🔹 Edad.Mora
- La **mediana es cero días** → la mayoría está al día.  
- Existen **outliers que alcanzan miles de días en mora**.  
- **Sesgo positivo (Skew ~ 6)**, indicador de concentración fuerte en pocos casos críticos.  

---

### 🔹 Intereses y Cuotas de Manejo
- Colas largas a la derecha.  
- La mayoría paga montos bajos.  
- Existen **valores atípicos muy altos** en pocos individuos.  

---

## 🔹 Conclusiones Globales
- **Edad**: variable estable y representativa.  
- **Variables financieras** (Saldo, Pagos, Ventas, Intereses, Avances) → presentan **alta asimetría y outliers**, requieren **normalización/transformación**.  
- **Edad.Mora**: clave para segmentar riesgo → mayoría sin mora vs minoría altamente morosa.  
- **AÑO_MES**: evidencia **sesgo temporal** → debe controlarse en el análisis.  

---

## Definición-del-Modelo
La preparación y preprocesamiento de los datos se llevó a cabo en el cuaderno "Preprocesamiento Data". En esta etapa se realizó la carga de los datos de entrenamiento, la limpieza y depuración de las variables, así como la transformación y normalización de los atributos relevantes del conjunto de datos. Se analizaron posibles correlaciones temporales de la variable objetivo y se evaluó el impacto potencial de las columnas de tipo fecha sobre el target.

- Durante el proceso, se trataron valores atípicos y se aplicaron técnicas de normalización utilizando el metodo de winsorize_serie, para garantizar la calidad y consistencia de los datos de entrada. Además, mediante el análisis de series temporales y la inspección de matrices de correlación, se comprobó que no existía una correlación significativa entre la variable objetivo y la segmentación por meses. Esto permitió descartar patrones estacionales laborales y validar la idoneidad de las fechas como predictoras en el modelo.

### Flujo-Datos

```plaintext
┌─────────────────────────────────────┐
│  Pasos para un desarrollo efectivo  │    
└─────────────────────────────────────┘
          │
          ▼
┌──────────────────────┐
│  Entendimiento       │ 
│   del problema       │
└──────────────────────┘
          │
          ▼
┌──────────────────────┐
│  DESCARGA DE DATOS   │ 
│  (Ingesta de Datos)  │
└──────────────────────┘
          │
          ▼
┌────────────────────────────────────────────────────┐
│1. RECEPCIÓN DE DATOS (ARCHIVOS CSV, PARQUET, ETC.) │
│   - Lectura de los ficheros                        │
│   - Otras fuentes de datos                         │
└────────────────────────────────────────────────────┘
          │
          ▼
┌───────────────────────────────────────────────────────┐
│2. PREPROCESAMIENTO Y VALIDACIÓN                       │
│   - Limpieza de registros (valores nulos, duplicados) │
│   - Formateo de fechas (datetime)                     │
│   - Conversión de tipos                               │
│   - Garantizar la integridad de los datos             │
└───────────────────────────────────────────────────────┘
          │
          ▼
┌──────────────────────────────────────────────────┐
│3. APLICACIÓN DE LA LÓGICA (REGLA DE NEGOCIOS)    │
│   - Para cada transacción, verificar si cumplen  │ 
│     el criterio o los criterios del negocio      │
└──────────────────────────────────────────────────┘
          │
          ▼
┌────────────────────────────────────────────────────────────────┐
│4. GENERACIÓN DE ATRIBUTOS (FEATURES)                           │
│   - Calculo de diferencia en dias, meses                       |
|   - Creacion de nuevas columnas explicativas                   |
|   - Verificacion y calidad de todos los datos                  │
└────────────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│5. Generacion del Modelo                                                     │
│   - Se generan varios modelos para poder obtener el de mejores resultados   |
│   - Ajuste de Hiperparametros y nueva selecion del mejor modelo             | 
└─────────────────────────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────────┐
│6 . Carga de datos nuevamente a la nube                      |
|    para disponibilidad del usuario,                         │
│   - mediante Azure o AWS                                    │
└─────────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────────┐
│5. SALIDA                                                    │
│   - Almacenamiento de resultados (CSV, Base de datos, etc.) │
│   - Consumir los resultados en dashboards                   │
└─────────────────────────────────────────────────────────────┘
          │
          ▼
┌───────────────────┐
│  FIN DEL PROCESO  │
└───────────────────┘
```

<p align="center">
  <img src="./Imagenes/Correlacion variable target.png", title="Correlacion variable target" width="600"/>
</p>

<p align="center">
  <img src="./Imagenes/Matriz correlacion1.png", title="Matriz correlacion1" width="600"/>
</p>

## Análisis de Correlación

El análisis de correlación evidencia que ninguna variable presenta una relación lineal fuerte con el Target (churn): todas las correlaciones se mantienen dentro del rango ±0.06.
Esto sugiere que la fuga no se explica por predictores individuales, sino por la combinación de múltiples factores no lineales.

🔹 Patrones observados

Saldo (≈ -0.06): correlación negativa → clientes con mayores saldos presentan menor probabilidad de fuga.

Pagos recientes (≈ +0.04): correlación positiva débil → podrían reflejar pagos de cierre antes de abandonar.

Total de intereses (≈ -0.04): ligera correlación negativa → clientes con mayor carga de intereses tienden a mantenerse activos.

Edad y estado civil: no muestran relación estadísticamente significativa con el churn.

🔹 Hallazgos adicionales

Existe multicolinealidad entre variables financieras (Pagos.Mes.Ant, Pago.del.Mes, Saldo, Total.Intereses), lo que sugiere:

Eliminar variables redundantes, o

Aplicar reducción de dimensionalidad (ej. PCA).

🔹 Implicación para el modelado

El churn en este dataset no depende de variables lineales aisladas, sino de interacciones complejas.
Por lo tanto, se recomienda:

Usar modelos no lineales y multivariados: árboles de decisión, ensembles (Random Forest, XGBoost, LightGBM), entre otros.

Complementar con técnicas de selección de variables (mutual information, feature importance) para identificar los predictores más relevantes.
---

### Entrenamiento_del_Modelo_de_Fuga

---

# Ejercicios-de-numeral-dos
El punto A indica lo siguiente:
A. (15%) Desarrollar una pregunta de diferencia de grupos para resolver con prueba de hipótesis (H1, H0)
y se nos pide resolver lo siguiente:
- Obtener descriptivos: media, mediana, asimetría
- Realizar pruebas de normalidad (plantear la hipótesis de normalidad)
- Aplicar a los mismos datos, tanto la prueba paramétrica como la no paramétrica para dos grupos independientes (Contestar a la pregunta realizada con base en los resultados de las pruebas).

Ejecucion en python
```python
# --- 0. Importar las bibliotecas necesarias ---
import numpy as np
from scipy import stats

# --- 1. Definir los datos de los grupos ---
grupo1_data = [
    66.180, 41.420, 81.880, 67.205, 58.626, 64.678, 74.439, 98.250, 39.465, 75.064,
    59.585, 66.086, 66.616, 41.374, 66.9, 39, 55.405, 46.824, 64.529, 64.517,
    65.166, 70.703, 77.391, 47.910, 66.116, 63.797, 53.051, 69.012, 60.368, 49.748,
    39.8, 34, 37.602, 66.948, 68.314, 85.354, 69.872, 85.009, 58.953, 41.744,
    91.509, 61.548, 37.981, 86.317, 59.479, 57.588, 53.0, 59, 46.234, 87.828,
    66.038, 65.175, 60.214, 74.662
]

grupo2_data = [
    0.621, 0.867, 0.550, 0.658, 0.794, 0.738, 0.855, 0.708, 0.774, 0.700,
    0.776, 0.904, 0.751, 0.921, 0.724, 0.754, 0.568, 0.867, 0.601, 0.725,
    0.798, 0.776, 0.835, 0.816, 0.842, 0.824, 0.706, 0.802, 0.738, 0.975,
    0.859, 0.644, 0.638, 0.809, 0.658, 0.824, 0.603, 0.855, 0.728, 0.838,
    0.932, 0.782, 0.727, 0.829, 0.809, 0.907, 0.871, 0.686, 0.750, 0.745, 0.662
]

# Convertir las listas a arrays de NumPy para facilitar los cálculos
grupo1 = np.array(grupo1_data)
grupo2 = np.array(grupo2_data)

# Nivel de significancia
alpha = 0.05

print("="*50)
print("ANÁLISIS ESTADÍSTICO PARA DOS GRUPOS INDEPENDIENTES")
print("="*50)

# --- 2. Obtener descriptivos: media, mediana, asimetría ---
print("\n--- A. ANÁLISIS DESCRIPTIVO ---")
print(f"Grupo 1 (n={len(grupo1)}):")
print(f"  Media: {np.mean(grupo1):.3f}")
print(f"  Mediana: {np.median(grupo1):.3f}")
print(f"  Asimetría (Skewness): {stats.skew(grupo1):.3f}\n")

print(f"Grupo 2 (n={len(grupo2)}):")
print(f"  Media: {np.mean(grupo2):.3f}")
print(f"  Mediana: {np.median(grupo2):.3f}")
print(f"  Asimetría (Skewness): {stats.skew(grupo2):.3f}")

# --- 3. Realizar pruebas de normalidad (Shapiro-Wilk) ---
print("\n--- B. PRUEBAS DE NORMALIDAD (Shapiro-Wilk) ---")
print("H₀: Los datos siguen una distribución normal.")
print("H₁: Los datos no siguen una distribución normal.")

# Grupo 1
stat_s1, p_s1 = stats.shapiro(grupo1)
print(f"\nGrupo 1: Estadístico W = {stat_s1:.3f}, p-valor = {p_s1:.3f}")
if p_s1 > alpha:
    print(f"  Decisión: No se rechaza H₀. Se asume normalidad (p > {alpha}).")
else:
    print(f"  Decisión: Se rechaza H₀. No se asume normalidad (p <= {alpha}).")

# Grupo 2
stat_s2, p_s2 = stats.shapiro(grupo2)
print(f"\nGrupo 2: Estadístico W = {stat_s2:.3f}, p-valor = {p_s2:.3f}")
if p_s2 > alpha:
    print(f"  Decisión: No se rechaza H₀. Se asume normalidad (p > {alpha}).")
else:
    print(f"  Decisión: Se rechaza H₀. No se asume normalidad (p <= {alpha}).")

# --- 4. Aplicar pruebas paramétrica y no paramétrica ---

# Primero, comprobamos la homogeneidad de varianzas para la prueba t
print("\n--- C. PRUEBA DE HOMOGENEIDAD DE VARIANZAS (Levene) ---")
print("H₀: Las varianzas de los grupos son iguales.")
print("H₁: Las varianzas de los grupos son diferentes.")
stat_l, p_l = stats.levene(grupo1, grupo2)
print(f"\nResultado: Estadístico F = {stat_l:.2f}, p-valor = {p_l}")
if p_l > alpha:
    print(f"  Decisión: No se rechaza H₀. Se asume que las varianzas son iguales (p > {alpha}).")
    equal_variances = True
else:
    print(f"  Decisión: Se rechaza H₀. Las varianzas son diferentes (p <= {alpha}).")
    equal_variances = False

print("\n--- D. APLICACIÓN DE PRUEBAS DE HIPÓTESIS ---")
print("Pregunta: ¿Existe una diferencia significativa entre los dos grupos?")
print("H₀: μ₁ = μ₂ (No hay diferencia entre las medias de los grupos)")
print("H₁: μ₁ ≠ μ₂ (Existe una diferencia entre las medias de los grupos)")

# Prueba Paramétrica: t de Student para muestras independientes
print("\n1. Prueba Paramétrica: t de Student para Muestras Independientes")
if not equal_variances:
    print("(Se aplicará la corrección de Welch porque las varianzas no son iguales)")

stat_t, p_t = stats.ttest_ind(grupo1, grupo2, equal_var=equal_variances)
print(f"  Estadístico t = {stat_t:.3f}")
print(f"  p-valor = {p_t}")
if p_t < alpha:
    print(f"  Conclusión: Se rechaza H₀. Existe una diferencia estadísticamente significativa entre los grupos (p < {alpha}).")
else:
    print(f"  Conclusión: No se rechaza H₀. No hay evidencia de una diferencia significativa (p >= {alpha}).")


# Prueba No Paramétrica: U de Mann-Whitney
print("\n2. Prueba No Paramétrica: U de Mann-Whitney")
stat_u, p_u = stats.mannwhitneyu(grupo1, grupo2, alternative='two-sided')
print(f"  Estadístico U = {stat_u}")
print(f"  p-valor = {p_u}")
if p_u < alpha:
    print(f"  Conclusión: Se rechaza H₀. Existe una diferencia estadísticamente significativa entre las distribuciones de los grupos (p < {alpha}).")
else:
    print(f"  Conclusión: No se rechaza H₀. No hay evidencia de una diferencia significativa (p >= {alpha}).")

print("\n" + "="*50)
print("CONCLUSIÓN FINAL DEL EJERCICIO")
print("="*50)
print("Ambas pruebas, la paramétrica (t de Welch) y la no paramétrica (U de Mann-Whitney),")
print(f"arrojaron p-valores extremadamente bajos (p < {alpha}), lo que proporciona evidencia")
print("contundente para rechazar la hipótesis nula. Por lo tanto, se concluye que")
print("existe una diferencia muy significativa entre el Grupo 1 y el Grupo 2.")
```
Con lo que tendremos el siguiente analisis estadistico

==================================================
### ANÁLISIS ESTADÍSTICO PARA DOS GRUPOS INDEPENDIENTES
==================================================

--- A. ANÁLISIS DESCRIPTIVO ---
Grupo 1 (n=54):
  - Media: 62.138
  - Mediana: 64.523
  - Asimetría (Skewness): 0.116

Grupo 2 (n=51):
  - Media: 0.767
  - Mediana: 0.776
  - Asimetría (Skewness): -0.215

--- B. PRUEBAS DE NORMALIDAD (Shapiro-Wilk) --- <br/>
H₀: Los datos siguen una distribución normal.<br/>
H₁: Los datos no siguen una distribución normal.

Grupo 1: Estadístico W = 0.969, p-valor = 0.171
  - Decisión: No se rechaza H₀. Se asume normalidad (p > 0.05).

Grupo 2: Estadístico W = 0.987, p-valor = 0.860
  - Decisión: No se rechaza H₀. Se asume normalidad (p > 0.05).
...
Ambas pruebas, la paramétrica (t de Welch) y la no paramétrica (U de Mann-Whitney),
arrojaron p-valores extremadamente bajos (p < 0.05), lo que proporciona evidencia
contundente para rechazar la hipótesis nula. Por lo tanto, se concluye que
existe una diferencia muy significativa entre el Grupo 1 y el Grupo 2. consistente bajo métodos paramétricos y no paramétricos.

---
El punto D indica lo siguiente:
D. (15%) Con los siguientes datos, realizar un ANOVA factorial:

- Realizar pruebas de normalidad (plantear la hipótesis de normalidad).
- (Contestar a la pregunta realizada con base en los resultados de laspruebas).

Ejecucion en R
```R
# Datos originales
medida <- c(2.1, 2.2, 1.8, 2, 1.9, 2.2, 2.6, 2.7, 2.5, 2.8, 
            1.8, 1.9, 1.6, 2, 1.9, 2.1, 2, 2.2, 2.4, 2.1)

factorA <- gl(4, 5)
factorB <- factor(rep(1:5, 4))

# Crear data frame
datos <- data.frame(medida, factorA, factorB)

# Modelo ANOVA sin interacción (efectos principales solamente)
modelo <- aov(medida ~ factorA + factorB, data = datos)

# Verificar residuos
residuos <- resid(modelo)
print("Primeros residuos:")
print(head(residuos))

# Prueba de normalidad (ahora funcionará)
shapiro_resultado <- shapiro.test(residuos)
print(shapiro_resultado)

# ANOVA factorial
print("ANOVA - Efectos principales:")
summary(modelo)

```
### Resultados

<p align="center">
  <img src="./Imagenes/Punto 2 D en R.png", title="Solucion Punto 2-D" width="600"/>
</p>

- El test de Shapiro–Wilk es una de las pruebas estadísticas más utilizadas para verificar si un conjunto de datos sigue una distribución normal.

==============================
### ANÁLISIS ESTADÍSTICO 
==============================
</p>
Para interpretar un ANOVA, es necesario comprobar que los residuos del modelo siguen una distribución normal.

- Hipótesis nula (H₀): los residuos provienen de una distribución normal.

- Hipótesis alternativa (H₁): los residuos no provienen de una distribución normal.

Dado que el test de Shapiro-Wilk aplicado a los residuos del modelo arrojó un estadístico W = 0.943 y un p-valor = 0.275.
Dado que p > 0.05, no se rechaza la hipótesis nula, lo cual indica que el supuesto de normalidad se cumple.

por lo que:
- El factor A presenta un efecto altamente significativo sobre la variable medida (p < 0.001), osea influye significativamente en la variable respuesta.

- El factor B no muestra un efecto estadísticamente significativo (p ≈ 0.64), sobre la variable respuesta.



---

# Análisis-de-punto-3

<p align="center">
  <img src="./Imagenes/Punto 3.png", title="Pregunta Punto 3" width="600"/>
</p>


## Interpretación_técnica_de_la_matriz

- Estructura:
  - Filas = etiqueta real (True label).
  - Columnas = etiqueta predicha (Predicted label).
  - Cada celda (𝑖,𝑗) (i= eje x,j= eje y) es el número de ejemplos de la clase real i que el modelo predijo como j.
  - La diagonal contiene los aciertos (TP de cada clase). Las celdas fuera de diagonal son errores: a la izquierda/derecha se ven los patrones de confusión entre clases
- Patrones destacados en la matriz dada
  - La clase 2:tiene un desempeño perfecto (444 aciertos, sin confusiones), y una Precisión, recall y F1 = 1.00.

Ejecucion en python de la replica de la Matriz dada.
```python
import numpy as np
import warnings
# import gc
import seaborn as sns
import matplotlib.pyplot as plt
# import unicodedata
import numpy as np
from sklearn.metrics import (
    accuracy_score,
    balanced_accuracy_score,
    f1_score,
    cohen_kappa_score,
    matthews_corrcoef
)

from sklearn.metrics import precision_recall_fscore_support
warnings.filterwarnings('ignore')

# Matriz de confusion
cm = np.array([
    [12, 26,  0,  4,  7, 21,  4, 1],  # clase real 0
    [22, 125, 0, 14, 43, 85, 12, 4],  # clase real 1
    [ 0,  0, 444, 0,  0,  0,  0, 0],  # clase real 2
    [ 4,  7,  0,  6,  1,  3,  0, 2],  # clase real 3
    [ 7, 34,  0,  2, 59, 14,  6, 3],  # clase real 4
    [13, 78,  0,  9, 26, 80,  7, 7],  # clase real 5
    [ 0, 11,  0,  4, 13, 13,  1, 1],  # clase real 6
    [ 1,  9,  0,  1,  3,  5,  1, 1]   # clase real 7
])

# Convertir la matriz de confusión en vectores de etiquetas verdaderas y predichas
y_true = []
y_pred = []
n_classes = cm.shape[0]
for true_label in range(n_classes):
    for pred_label in range(n_classes):
        count = cm[true_label, pred_label]
        # Añadimos tantas repeticiones como indique la celda (i,j)
        y_true += [true_label] * count
        y_pred += [pred_label] * count
y_true = np.array(y_true)
y_pred = np.array(y_pred)

# Calcular precisión, recall, F1-score y soporte por clase
precision, recall, f1, support = precision_recall_fscore_support(
    y_true, y_pred, labels=list(range(n_classes))
)

# Mostrar resultados por clase
for cls in range(n_classes):
    print(
        f"Clase {cls}: "
        f"P={precision[cls]:.3f}, "
        f"R={recall[cls]:.3f}, "
        f"F1={f1[cls]:.3f} "
        f"(n={support[cls]})"
    )


## Variables Globales
# 1) Exactitud micro (accuracy)
micro_accuracy = accuracy_score(y_true, y_pred)

# 2) Balanced accuracy (macro-recall)
balanced_acc = balanced_accuracy_score(y_true, y_pred)

# 3) Macro-F1 y Weighted-F1
macro_f1 = f1_score(y_true, y_pred, average="macro")
weighted_f1 = f1_score(y_true, y_pred, average="weighted")

# 4) Cohen’s kappa
kappa = cohen_kappa_score(y_true, y_pred)

# 5) MCC multiclass
mcc = matthews_corrcoef(y_true, y_pred)

# Mostrar resultados
print(f"Exactitud (micro): {micro_accuracy:.3f}")
print(f"Balanced accuracy (macro-recall): {balanced_acc:.3f}")
print(f"Macro-F1: {macro_f1:.3f}")
print(f"Weighted-F1: {weighted_f1:.3f}")
print(f"Cohen’s kappa: {kappa:.3f}")
print(f"MCC multiclass: {mcc:.3f}")

```
Salida del modelo:
- Clase 0: P=0.203, R=0.160, F1=0.179 (n=75)
- Clase 1: P=0.431, R=0.410, F1=0.420 (n=305)
- Clase 2: P=1.000, R=1.000, F1=1.000 (n=444)
- Clase 3: P=0.150, R=0.261, F1=0.190 (n=23)
- Clase 4: P=0.388, R=0.472, F1=0.426 (n=125)
- Clase 5: P=0.362, R=0.364, F1=0.363 (n=220)
- Clase 6: P=0.032, R=0.023, F1=0.027 (n=43)
- Clase 7: P=0.053, R=0.048, F1=0.050 (n=21)

Metricas Globales
- Exactitud (micro): 0.580
- Balanced accuracy (macro-recall): 0.342
- Macro-F1: 0.332
- Weighted-F1: 0.577
- Cohen’s kappa: 0.455
- MCC multiclass: 0.456


Clases 1 y 5: fuerte confusión cruzada.

- Muchos verdaderos 1 se predicen como 5 (85).

- Muchos verdaderos 5 se predicen como 1 (78). El modelo mezcla 1 ↔ 5, sugiere fronteras poco separables o desbalance

Clases 6 y 7: muy baja sensibilidad (apenas 1 acierto en cada caso).
- Recall 6 ≈ 0.023, Recall 7 ≈ 0.048; probablemente poca evidencia o variables poco informativas para esas clases.

Las clases dominantes:
- la clase 2 domina con 444 aciertos , seguida por la clase 1 con 125 aciertos y la clase 5 con 80 aciertos.

Datos Generales: 
- El modelo es excelente para la clase 2, aceptable-regular para 1, 4 y 5, y muy débil para 6 y 7. Existe confusión sistemática 1↔5. Conjunto desequilibrado, por lo que conviene ponderar por clase, remuestrear o ajustar umbrales.

## Metricas_para_dicha_Matriz
B. ¿Cuáles y cuántas métricas puede obtener de dicha matriz?

De una matriz de confusión de N clases se obtienen, para cada clase n

### Conteos básicos

- **TP₍c₎**: Verdaderos positivos (celda de la diagonal).  
- **FP₍c₎**: Falsos positivos (columna *c* fuera de la diagonal).  
- **FN₍c₎**: Falsos negativos (fila *c* fuera de la diagonal).  
- **TN₍c₎**: Verdaderos negativos.  
  \[
  TN_c = N - TP_c - FP_c - FN_c
  \]

- Tasas y medidas por clase
---

| Métrica                                | Fórmula                                                                                                        | Interpretación                                                                     |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Precisión (PPV)**                    | $\mathrm{Precision}_c = \frac{\mathrm{TP}_c}{\mathrm{TP}_c + \mathrm{FP}_c}$                                   | Proporción de predicciones de $c$ que son correctas.                               |
| **Sensibilidad / Recall**              | $\mathrm{Recall}_c = \frac{\mathrm{TP}_c}{\mathrm{TP}_c + \mathrm{FN}_c}$                                      | Proporción de ejemplos de $c$ detectados correctamente.                            |
| **Especificidad (TNR)**                | $\mathrm{TNR}_c = \frac{\mathrm{TN}_c}{\mathrm{TN}_c + \mathrm{FP}_c}$                                         | Capacidad para evitar falsos positivos de $c$.                                     |
| **Tasa de falsos positivos (FPR)**     | $\mathrm{FPR}_c = 1 - \mathrm{TNR}_c = \frac{\mathrm{FP}_c}{\mathrm{FP}_c + \mathrm{TN}_c}$                    | Frecuencia con que se confunden otras clases con $c$.                              |
| **Tasa de falsos negativos (FNR)**     | $\mathrm{FNR}_c = 1 - \mathrm{Recall}_c = \frac{\mathrm{FN}_c}{\mathrm{FN}_c + \mathrm{TP}_c}$                 | Proporción de ejemplos de $c$ que se pierden.                                      |
| **Valor predictivo negativo (NPV)**    | $\mathrm{NPV}_c = \frac{\mathrm{TN}_c}{\mathrm{TN}_c + \mathrm{FN}_c}$                                         | Probabilidad de que un ejemplo no etiquetado como $c$ sea realmente de otra clase. |
| **Tasa de descubrimiento falsa (FDR)** | $\mathrm{FDR}_c = 1 - \mathrm{Precision}_c$                                                                    | Proporción de predicciones de $c$ que son erróneas.                                |
| **Tasa de falta de predicción (FOR)**  | $\mathrm{FOR}_c = 1 - \mathrm{NPV}_c$                                                                          | Proporción de negativos predichos para $c$ que en realidad son $c$.                |
| **F1-score**                           | $F1_c = \frac{2 \cdot \mathrm{Precision}_c \cdot \mathrm{Recall}_c}{\mathrm{Precision}_c + \mathrm{Recall}_c}$ | Media armónica entre precisión y recall.                                           |
| **Balanced accuracy (por clase)**      | $\mathrm{BA}_c = \tfrac{1}{2}(\mathrm{Recall}_c + \mathrm{TNR}_c)$                                             | Valor medio entre sensibilidad y especificidad.                                    |
| **Prevalencia**                        | $\mathrm{Prev}_c = \frac{\mathrm{TP}_c + \mathrm{FN}_c}{N}$                                                    | Fracción de ejemplos verdaderos de la clase $c$.                                   |
| **Prevalencia predicha**               | $\mathrm{PrevPred}_c = \frac{\mathrm{TP}_c + \mathrm{FP}_c}{N}$                                                | Fracción de ejemplos que el modelo predice como $c$.                               |

---

### Agregaciones multiclass

- **Macro**: promedio no ponderado (Precision/Recall/F1 macro).  
- **Weighted**: promedio ponderado por soporte (Precision/Recall/F1 weighted).  
- **Micro**: cuenta global de TP, FP, FN (Precision/Recall/F1 micro = exactitud global).  
- **Exactitud global**, **Balanced accuracy macro**, **Cohen’s κ**, **MCC multiclass**.

---

### ¿Cuántas métricas?

Con \\(C = 8\\):

- **32 conteos** (TP/FP/FN/TN × 8). donde para cada clase en un problema multiclase se pueden defini 4 conteos basicos por clase×8 clases=32 conteos en total
- **≈ 10–12 tasas por clase** (ej. Precision, Recall, F1, TNR, FPR, FNR, NPV, FDR, FOR, BA…) → **80–96 métricas**.  que salen de 10 metricas x 8 clases =80 metricas, 12 metricas x 8 clases =96 metricas
- **≥ 10 métricas globales** (macro/micro/weighted de P/R/F1, accuracy, balanced accuracy, κ, MCC, etc.).  





	Fracción de ejemplos que el modelo predice co
---
