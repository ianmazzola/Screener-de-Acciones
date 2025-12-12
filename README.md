# Proyecto: Screener de Acciones para detectar oportunidades de inversión + Evaluación cualitativa de los resultados

Este proyecto combina análisis cuantitativo mediante screeners fundamentales con una etapa cualitativa basada en ventajas competitivas (moats) para identificar empresas de alta calidad a partir de criterios objetivos y estratégicos de inversión.
El objetivo es construir un pipeline reproducible de análisis, desde la extracción de datos financieros con Python hasta la interpretación mediante una presentación final (PowerPoint).
Este enfoque permite evaluar empresas desde dos dimensiones:
- Fundamentals sólidos (números)
- Ventaja competitiva sostenible (negocio)

## 📈 Insights principales

- **Empresas con alto *earnings growth*** tienden a ocupar las primeras posiciones del ranking, lo que puede resultar atractivo para inversores con mayor tolerancia al riesgo.  
- **Un PEG alto combinado con un PE bajo** indica posibles oportunidades de revalorización, ya que el precio actual no refleja plenamente el potencial de crecimiento de la empresa.  
- **El score se aplica a diferentes sectores**, permitiendo comparar calidad y valuación de forma relativa dentro de cada industria.
- **El enfoque es adaptable al perfil del inversor**, pudiendo ajustar los pesos de las métricas en el score para priorizar crecimiento, estabilidad o valuación según la estrategia deseada.
- **El score, tal como está calculado, es dependiente del periodo de referencia**: estandarizar con datos contemporáneos limita la comparabilidad histórica.
  
## 📌 Objetivo
Aplicar técnicas de análisis de datos para evaluar y comparar empresas del mercado bursátil mediante un **score propio** que integra métricas financieras clave (crecimiento, rentabilidad, apalancamiento y valuación).  
El objetivo es **identificar oportunidades de inversión accionables** y demostrar un flujo de trabajo completo y reproducible que abarca recolección de datos, preprocesamiento, análisis exploratorio, visualización de resultados y comunicación de insights.

## 📊 Descripción del flujo de trabajo
1. **Obtención de datos**  
Para este análisis se desarrolló un dataset propio a partir de datos obtenidos mediante la API de Yahoo Finance (`yfinance`).  
El proceso consistió en:

- **Selección del universo de empresas**: lista de tickers representativos de distintos sectores y tamaños de mercado. (S&P 500, Nasdaq y Dow Jones)
- **Descarga automatizada de datos financieros**: métricas de crecimiento, rentabilidad, apalancamiento y valuación.
- **Estructuración del dataset**: integración en un único `DataFrame` y exportación a `Acciones.csv` para uso en el análisis principal.

El código base para este proceso es:

```python
import yfinance as yf
import pandas as pd

sublistas = [tickers[i:i + 10] for i in range(0, len(tickers), 10)]
for i, bloque in enumerate(sublistas):
    print(i, bloque)

atributos = [
    "marketCap",
    "sector",
    "trailingPE",
    "forwardPE",
    "priceToBook",
    "dividendYield",
    "beta",
    "profitMargins",
    "returnOnEquity",
    "debtToEquity",
    "currentRatio",
    "earningsGrowth",
    "trailingPegRatio",
    "returnOnAssets",
    "epsForward",
]

dfs = []  # Acá guardamos todos los dataframes

for i, bloque in enumerate(sublistas):
    datos = []  # Se crea una nueva lista vacía para cada bloque
    for ticker in bloque:
        info = yf.Ticker(ticker).info
        data = {atributo: info.get(atributo, None) for atributo in atributos}
        data["Ticker"] = ticker
        datos.append(data)
    
    df_bloque = pd.DataFrame(datos).set_index("Ticker")
    dfs.append(df_bloque)  # Almacenar Dataframes por bloques

# Creamos un único dataframe con todos los datos
df_final = pd.concat(dfs)

df_final.to_csv("Acciones.csv")
```
Se crean sublistas y se itera creando dataframes más pequeños para evitar bugs o fallas a la hora de consultar con la API.

2. **Preprocesamiento**  
    - **Gestión de valores faltantes**:  
  - No hubo eliminación de ninguna columna, debido a que no tenían un gran porcentaje de valores nulos.  
  - Imputación utilizando medianas y medias según las distribuciones de cada variable.
  - Imputación utilizando ceros, debido al significado del valor nulo de dicha variable. 

    - **Detección y tratamiento de outliers**:  
  - Identificación de valores extremos mediante diagramas de caja (*boxplots*) y análisis de rango intercuartílico (IQR).  
  - Ajuste o eliminación de valores que distorsionaban el análisis, manteniendo coherencia en la comparación entre empresas.  

![Outliers](Outliers.png)

3. **Análisis Exploratorio de Datos (EDA)**  
Con el dataset limpio se realizó un análisis exploratorio para comprender la distribución de las métricas y las relaciones entre ellas:

- **Distribuciones individuales**: histogramas y *boxplots* para variables clave como *earnings growth*, *ROE*, *debt-to-equity*, *PE* y *PEG*.  
- **Mapas de correlaciones**: identificación de relaciones significativas entre indicadores de rentabilidad, apalancamiento y valuación.  
- **Comparaciones sectoriales**: visualizaciones *scatter* (por ejemplo, *ROE* vs *Price-to-Book*) diferenciadas por sector, tamaño de empresa o múltiplos de valuación.
- **Análisis de relaciones esperadas vs. atípicas**: detección de empresas que se desvían de patrones sectoriales, potencialmente indicando oportunidades o riesgos.

4. **Score**  
Para ordenar las empresas según su atractivo relativo, se desarrolló un **score ponderado** que combina métricas de crecimiento, rentabilidad, apalancamiento y valuación:

- **Selección de métricas**: *earnings growth*, *ROE*, *debt-to-equity*, *trailing PE*, *forward PE*, *PEG*, *Price-to-Book*, entre otras.
- **Estandarización de variables**: uso de `StandardScaler` para garantizar comparabilidad entre métricas con escalas distintas.
- **Asignación de ponderaciones**: pesos definidos según relevancia teórica y empírica en la valoración de empresas.
- **Cálculo del ranking**: empresas ordenadas de mayor a menor score, identificando las más atractivas dentro de cada sector.

5. **Casos de estudio**  
Se aplicó el score a eventos históricos para evaluar su capacidad predictiva y su utilidad en contextos reales:

- **Apple (2016)**: simulación del score en el momento de la inversión de Warren Buffett, mostrando fundamentos sólidos previos a un periodo de gran revalorización.
- **NVIDIA (2017 y 2023)**: análisis antes y después de hitos clave como el boom de la inteligencia artificial, evidenciando cambios en métricas y posición en el ranking.
- **Microsoft (2025)**: evaluación en un contexto de impacto por aranceles, analizando cómo el score refleja cambios en sus fundamentales.

Estos casos permiten validar el score como herramienta de *screening* inicial y entender sus limitaciones, especialmente en relación a la estandarización basada en un periodo de referencia fijo.

![Microsoft](Microsoft.png)

## 📊 Visualizaciones clave

**1. Mapa de correlaciones de métricas financieras**  
Identifica relaciones entre indicadores clave.  
![Mapa de correlaciones](Mapa_Correlaciones.png)

**2. Relación *ROE* vs *Price-to-Book* – Sector Tecnológico**  
Muestra cómo empresas tecnológicas se distribuyen en función de rentabilidad y valuación, destacando potenciales oportunidades y riesgos.  
![ROE vs P/B Tecnología](PB_Tecnologico.png)

**3. Relación *ROE* vs *Price-to-Book* – Sector Financiero**  
Análisis equivalente para el sector financiero, con patrones y dispersiones distintas al tecnológico.  
![ROE vs P/B Financiero](PB_Financiero.png)

**4. Relación entre *Trailing PE* y *Forward PE***  
Incluye la línea y = x como referencia para identificar si el mercado espera crecimiento o no de las empresas de Tecnología y Real Estate.  
![PE Ratio comparativo](PE_Ratio.png)


## 🛠 Tecnologías y librerías
- Python 
- Pandas, NumPy
- Matplotlib, Seaborn
- scikit-learn
- yfinance
- Jupyter Notebook

## 🚀 Posibles mejoras futuras

- **Dashboard interactivo**: implementación en Power BI o Tableau para explorar empresas y métricas de forma dinámica.
- **Ampliación del universo de empresas**: incluir empresas de mercados emergentes, índices sectoriales adicionales (Merval, Mercados de China o Brasil, etc.) y small caps para ampliar el alcance del análisis.
- **Optimización y ajuste flexible de ponderaciones**:  permitir modificar los coeficientes del score para representar distintas filosofías de inversión. Esto podría dar pie a un proyecto que recomiende ponderaciones óptimas según el perfil de riesgo y los objetivos del usuario.
- **Actualizaciones**: buscar la forma de descargar nuevos datos, modificar el universo y recalcular el score de forma periódica.
