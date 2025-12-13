# Proyecto: Screener de Acciones para detectar oportunidades de inversión + Evaluación cualitativa de los resultados

Este proyecto combina una serie de screeners de acciones construidos en Python con una sección de análisis cualitativo basado en las ventajas competitivas (moats) de los negocios que surgieron como oportunidades de los screeners. El objetivo principal del proyecto es crear un pipeline que permita automatizar el filtrado de empresas antes de hacer un análisis profundo. A su vez, mi intención es que este proyecto no solo sirva para mostrar habilidades de Python y es por eso que añado una evaluación cualitativa de algunas empresas que pasaron los filtros.


## 📌 Objetivo
Desarrollar un flujo de trabajo que permita:
- Obtener un universo de acciones para analizar y definir que métricas resultan relevantes a la hora de evaluar.
- Construir un dataset financiero extrayendo las métricas de interes a traves de (`yfinance`).
- Aplicar 4 screeners (Value Investing de Buffett y de Benjamin Graham, High Growth y uno basado en criterios personales) que filtren las acciones a partir del cumplimiento de las métricas de interes.
- Seleccionar solo las empresas con puntaje perfecto en cada screener. En este caso se pueden hacer salvedades según los intereses de cada uno, tomo esa decisión para simplificar el posterior análisis cualitativo.
- Evaluar cualitativamente las ventajas del negocio mediante un prompt específicamente diseñado para esta sección, basandome en los resultados que brinde el LLM (ChatGPT).
- Producir un informe visual (Power Point) para presentar criterios y resultados de ambos análisis.

Mi idea con este proyecto es ir más allá del uso de las herramientas de datos. Busco integrar el análisis con una lectura cualitativa de los negocios y que este flujo pueda ser replicable semanalmente, y de utilidad, como inversor minorista.




## 🛠 Tecnologías y librerías
- Python 
- Pandas, NumPy
- yfinance
- Jupyter Notebook
- Microsoft Excel (como soporte)
- Microsoft Power Point (presentación)
  

## 📊 Estructura del Proyecto
1. **Construcción del Dataset (Notebook 1)**  

El script consiste en:
- Definir el universo de empresas (acorté tiempos mediante un dataset de otro proyecto que tenía). En este caso utilice un conjunto de empresas formado por el S&P 500, Nasdaq, Dow Jones, ADRs de empresas argentinas y algunas de otros mercados (Brasil, Europa, Taiwan).
- Automatizar la extracción de métricas clave desde la API de Yahoo Finance mediante `yfinance`
- Integra todo en un único `DataFrame`
- Exporta los datos a un archivo `Screener_investing1.csv` para que la construcción de screeners pueda ejecutarse sin realizar llamadas adicionales a la API.

2. **Creación de los Screeners (Notebook 2)**

El script implementa 4 screeners basados en distintas filosofías de inversión:
    - **Screener 1: Warren Buffett**:  
  - Enfocado en la calidad del negocio, ROE, baja deuda, flujos de caja y estabilidad.  

    - **Screener 2: Benjamin Graham**:  
  - Enfocado de forma conservadora basandose en valuaciones y perspectiva financiera. 

    - **Screener 3: High Growth**:  
  - Enfocado en crecimiento acelerado, con valuaciones y deuda razonable. 

    - **Screener 4: Personalizado**:  
  - Basado en criterios propios de inversión, combinando lo que más valoro de las tres filosofías anteriores.+

Cada screener:
- Evalúa criterios uno por uno.
- Cuenta cuántos criterios cumple cada empresa y se ordena de forma descendiente según la cantidad de criterios aprobados.
- Las tablas se formatean con colores (verde/rojo) para visualizar cumplimiento.
- Se muestran solo las top 50 empresas por screener para claridad.

Los resultados se exportan a Excel para visualizar las tablas de forma sencilla.

3. **Análisis cualitativo de moats utilizando LLMs**  

Una vez obtenidas las empresas que tuvieron puntaje perfecto en alguno de los 4 screeners, incorporo al proyecto una etapa cualitativa en el cual utilizo un prompt estructurado para obtener las ventajas competitivas de cada empresa. El objetivo acá es contextualizar los puntajes del screener y evaluar la sostenibilidad de los modelos de negocios. La sección 2 del proyecto simplemente es una preselección de acciones.

Para el análisis cualitativo se analizan cinco dimensiones de ventajas competitivas:

- Network effects (red de usuarios)
- Switching costs
- Intangibles (marca, patentes, regulación)
- Ventajas de costos
- Counter-positioning

<details>
<summary><strong>Prompt utilizado para el análisis de moats</strong></summary>

[Analizá la empresa {TICKER – Nombre} desde la perspectiva de ventajas competitivas (moats).

Evaluá cada atributo de forma independiente, usando información pública, historia del negocio y su posición competitiva.

Para cada atributo:
- Indicá el nivel actual del moat:
    - Inexistente
    - Pequeño
    - Amplio
- Indicá la tendencia futura esperada:
    - Se achica
    - Se mantiene estable
    - Se amplía
- Justificá brevemente (3–5 líneas).

Atributos a evaluar:
1. Network Effects (red de usuarios)
2. Switching Costs
3. Intangibles (marca, patentes, contenido, regulación)
4. Cost Advantages
5. Counter-positioning

Output esperado:
- Tabla resumen con los 5 atributos
- Justificación breve por atributo
- Conclusión final:
    - ¿La empresa posee un moat sostenible en el largo plazo?
    - ¿Cuál es el principal riesgo competitivo?]

</details>

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


## 📈 Insights principales

- **Empresas con alto *earnings growth*** tienden a ocupar las primeras posiciones del ranking, lo que puede resultar atractivo para inversores con mayor tolerancia al riesgo.  
- **Un PEG alto combinado con un PE bajo** indica posibles oportunidades de revalorización, ya que el precio actual no refleja plenamente el potencial de crecimiento de la empresa.  
- **El score se aplica a diferentes sectores**, permitiendo comparar calidad y valuación de forma relativa dentro de cada industria.
- **El enfoque es adaptable al perfil del inversor**, pudiendo ajustar los pesos de las métricas en el score para priorizar crecimiento, estabilidad o valuación según la estrategia deseada.
- **El score, tal como está calculado, es dependiente del periodo de referencia**: estandarizar con datos contemporáneos limita la comparabilidad histórica.
  

## 🚀 Posibles mejoras futuras

- **Dashboard interactivo**: implementación en Power BI o Tableau para explorar empresas y métricas de forma dinámica.
- **Ampliación del universo de empresas**: incluir empresas de mercados emergentes, índices sectoriales adicionales (Merval, Mercados de China o Brasil, etc.) y small caps para ampliar el alcance del análisis.
- **Optimización y ajuste flexible de ponderaciones**:  permitir modificar los coeficientes del score para representar distintas filosofías de inversión. Esto podría dar pie a un proyecto que recomiende ponderaciones óptimas según el perfil de riesgo y los objetivos del usuario.
- **Actualizaciones**: buscar la forma de descargar nuevos datos, modificar el universo y recalcular el score de forma periódica.
