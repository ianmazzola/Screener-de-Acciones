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

<ins>1. **Construcción del Dataset (Notebook 1)**</ins>  

El script consiste en:
- Definir el universo de empresas (acorté tiempos mediante un dataset de otro proyecto que tenía). En este caso utilice un conjunto de empresas formado por el S&P 500, Nasdaq, Dow Jones, ADRs de empresas argentinas y algunas de otros mercados (Brasil, Europa, Taiwan).
- Automatizar la extracción de métricas clave desde la API de Yahoo Finance mediante `yfinance`
- Integra todo en un único `DataFrame`
- Exporta los datos a un archivo `Screener_investing1.csv` para que la construcción de screeners pueda ejecutarse sin realizar llamadas adicionales a la API.



<ins>2. **Creación de los Screeners (Notebook 2)**</ins>

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



<ins>3. **Análisis cualitativo de moats utilizando LLMs**</ins> 

Una vez obtenidas las empresas que tuvieron puntaje perfecto en alguno de los 4 screeners, incorporo al proyecto una etapa cualitativa en el cual utilizo un prompt estructurado para obtener las ventajas competitivas de cada empresa. El objetivo acá es contextualizar los puntajes del screener y evaluar la sostenibilidad de los modelos de negocios. La sección 2 del proyecto simplemente es una preselección de acciones y queremos que la sección 3 elimine falsos positivos, aumentando las chances de encontrar un negocio con altas probabilidades de sostener los retornos a largo plazo.

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



<ins>4. **Presentación en Power Point**</ins>  

Como parte complementaria, se incluye un PowerPoint que resume:
- el flujo de trabajo
- los screeners y sus criterios
- un análisis de las ventajas competitivas de 10 empresas que pasaron todos los criterios
- conclusiones 



## 🚀 Posibles mejoras futuras

- **Dashboard interactivo**: implementación en Power BI o Tableau para explorar empresas y métricas de forma dinámica.
- **Mejorar la extracción de nombres para el universo de empresas**: Este proyecto es dependiente de un dataset antiguo, que no representa actualmente al detalle a los tres índices (S&P 500, Nasdaq, Dow Jones) que forman la mayoría del universo. Debería encontrar una forma alternativa de crear una lista de Python con todos esos nombres, con una metodología distinta a la que usé en mi proyecto antiguo
- **Nuevas APIs**:  Este proyecto se basa en la información que provee Yahoo Finance. Trabajar con una nueva API premium podría brindar un análisis mas robusto y no tan dependiente de datos públicos. 

