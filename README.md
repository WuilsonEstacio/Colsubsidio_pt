<h1 align="center">Colsubsidio_pt</h1>

<div align="center">

# Desarrollo prueba técnica Colsubsidio

**Responsable:** Wuilson Adolfo Estacio Rojas  
**Analista de incorporación:** Yeimmy Verónica Bustos Moreno 
**Jefe gestión de la información:** Camilo Garzón Márquez 
**Fecha:** 15-09-2025  

</div>


# Prueba Tecnica Cientifico de Datos - Colsubsidio 

Este documento describe el proceso seguido para **explorar** los datos, **identificar** variables relevantes y **definir** un modelo heurístico para solucionar cada uno de los puntos planteados en la prueba tecnica.


## Tabla de Contenido
1. [Introducción](#introducción)  
2. [Objetivo](#objetivo)  
3. [Modelo de Fuga](#Modelo-de-fuga)  
   3.1 [Exploración y Evaluación de Datos (EDA)](#Eda)  
      3.1.1 [Data Quality](#Data-Quality)  
      3.1.2 [Estadísticas y descriptivos](#Estadísticas-descriptivas)  
      3.1.3 [Hipótesis preliminares](#Hipótesis-preliminares)  
4. [Ejercicios de numeral Dos](#Ejercicios-de-numeral-dos)  
5. [Análisis de punto 3](#Análisis-de-punto-3)  



## introduccion

### ¿Qué es Colsubsidio?

Es una **Caja de Compensación Familiar en Colombia**, entidad privada, de naturaleza social y sin ánimo de lucro, que administra recursos del subsidio familiar y presta servicios a trabajadores y sus familias en distintos sectores.  

Colsubsidio fue fundada en **1957**, por lo que lleva alrededor de **68 años** en funcionamiento en Colombia.  

Su objetivo principal es **mejorar la calidad de vida de los trabajadores afiliados y sus familias**.  
Para ello ha dividido la prestación de sus servicios en **once Unidades Especializadas de Servicio (UES)**.  

Una de ellas, la **UES de Crédito**, atiende las necesidades de la población (afiliados y no afiliados) a través de productos crediticios como: **cupo, consumo e hipotecario**, a nivel nacional.  

Pero existen otras UES, como se muestra en la siguiente imagen:  

<p align="center">
  <img src="./Imagenes/UES_Colsubsidio.png" alt="UES Disponibles" title="UES Disponibles" width="500"/>
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
Para este modelo de fuga inicialmente para ello cargamos los datos de train (train_test_demograficas,train_test_subsidios, train) para la exploracion de los datos, con un total de 50001 clientes unicos o Id unicos

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
  <img src="./Imagenes/top 10 Motivos Cancelacion1.png", title="top 10 Motivos Cancelacion" width="500"/>
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

- **Distribución de Género y nivel_educativo (`Distribución de Género y nivel_educativo`)**
<p align="center">
  <img src="./Imagenes/Distribución de Género y nivel_educativo.png" alt="Distribución de Género y nivel_educativo" title="Distribución de Género y nivel_educativo" width="500"/>
</p>

## 🔹 Hallazgos principales de acuerdo a Distribución de Género y nivel_educativo

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

## 🔹 Implicaciones estadísticas
- Existe un **patrón homogéneo de abandono según nivel educativo**: la **primaria concentra la mayor pérdida** en ambos géneros.  
- La diferencia más visible se da en **primaria**, con hombres más afectados que mujeres.  
- Los niveles educativos más altos (**técnico/tecnológico**) no eliminan la salida, pero presentan cifras menores que primaria.  



<p align="center">
  <img src="./Imagenes/Diagnostic1.png", title="Diagnostic1" width="500"/>
</p>

<p align="center">
  <img src="./Imagenes/Diagnostic2.png", title="Diagnostic2" width="500"/>
</p>

<p align="center">
  <img src="./Imagenes/Diagnostic3.png", title="Diagnostic3" width="500"/>
</p>


## 🔹 Análisis Estadístico de Variables

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






# Ejercicios-de-numeral-dos

---

# Análisis-de-punto-3
