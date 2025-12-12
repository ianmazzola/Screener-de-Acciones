Proyecto: Análisis Cuantitativo y Cualitativo de Empresas mediante Screeners Fundamentales + Evaluación de Moats

Este proyecto combina análisis cuantitativo mediante screeners fundamentales con una etapa cualitativa basada en ventajas competitivas (moats) para identificar empresas de alta calidad a partir de criterios objetivos y estratégicos de inversión.

El objetivo es construir un pipeline reproducible de análisis, desde la extracción de datos financieros con Python hasta la interpretación ejecutiva mediante una presentación final (PowerPoint).

Este enfoque permite evaluar empresas desde dos dimensiones:

Fundamentals sólidos (números)

Ventaja competitiva sostenible (negocio)

y combinar ambas partes en un proceso similar al que usan analistas profesionales.

📌 Objetivo

Desarrollar un framework de análisis que permita:

Construir y actualizar automáticamente un dataset financiero usando datos de yfinance y cálculos propios.

Aplicar 4 screeners cuantitativos (Buffett, Graham, Growth, y uno de criterios personalizados).

Seleccionar solo las empresas con puntaje perfecto en cada screener.

Evaluar cualitativamente sus moats mediante una metodología estándar apoyada en LLMs (IA), usando un prompt diseñado especialmente para este proyecto.

Producir un informe visual (PowerPoint) que presenta criterios, resultados y hallazgos clave.

Este proyecto busca ir más allá del simple uso de herramientas de datos: integra análisis financiero real, métricas cuantitativas y lectura cualitativa del negocio.

🧩 Estructura del Proyecto

El proyecto se divide en tres partes principales:

1. Construcción del Dataset (Notebook 1)

Se desarrolla un script que:

Define el universo de empresas (más de 1000 tickers globales, excluyendo SP500, Nasdaq y Dow Jones).

Automatiza la extracción de métricas clave desde la API de Yahoo Finance mediante yfinance.

Calcula indicadores derivados (FCF Yield, Revenue Growth YoY, márgenes, etc.).

Integra todo en un único DataFrame.

Exporta los datos a un archivo .csv para que el análisis posterior pueda ejecutarse sin realizar llamadas adicionales a la API.

Este proceso desacopla extracción de análisis, reduciendo tiempos y errores.

2. Aplicación de Screeners Cuantitativos (Notebook 2)

Se implementan cuatro screeners inspirados en filosofías de inversión reales:

✔ Screener Warren Buffett

Enfocado en calidad, márgenes, ROE, baja deuda, cash flow y estabilidad.

✔ Screener Benjamin Graham

Enfoque conservador basado en valuación (P/E, P/B, dividendos) y fortaleza financiera.

✔ Screener High Growth

Enfocado en crecimiento acelerado (Revenue YoY >20%, márgenes en expansión, PEG razonable, deuda manejable).

✔ Screener Personalizado

Basado en criterios propios como ROIC, ROE, margen operativo, FCF positivo y estructura de deuda sostenible.

Cada screener:

Evalúa criterios uno por uno.

Cuenta cuántos criterios cumple cada empresa.

Las tablas se estilizan con colores (verde/rojo) para visualizar cumplimiento.

Se muestran solo las top 50 empresas por screener para claridad.

Los resultados se exportan a Excel para usar en el informe final.

3. Análisis Cualitativo de Moats

Una vez identificadas las empresas con puntaje perfecto, el proyecto incorpora una etapa cualitativa inspirada en criterios de Buffett y frameworks como Porter.

🎯 ¿Por qué agregar moats?

Para evitar que el proyecto sea “solo Python”, se suma una parte estratégica:

moat tecnológico

ventajas de costo

diferenciación

red de usuarios (network effects)

switching costs

intangibles (marca, patentes, contenido)

regulación favorable

🧠 Prompt diseñado (resumido)

Un prompt estructurado que analiza cada empresa en:

Descripción del negocio

Tipo de moat principal

Indicadores de fortalecimiento/debilitamiento del moat

Riesgos competitivos reales

Evidencia fundamental que respalda el moat

Conclusión: ¿el moat es “Strong”, “Moderate” o “Weak”?

Este componente cualitativo es el corazón del storytelling del proyecto.

4. Presentación Final (PowerPoint)

Como parte complementaria del proyecto, se incluye un PowerPoint que resume:

el flujo de trabajo

los screeners y sus filosofías

criterios utilizados

empresas que pasaron todos los criterios

análisis de moats para las mejores empresas

conclusiones ejecutivas

Este paso replica exactamente el trabajo de un analista en un fondo o en un rol BI/Finance.

📊 Tecnologías y Librerías

Python

Pandas, NumPy

Matplotlib

yfinance

Jupyter Notebook

Excel como soporte

PowerPoint (visualización ejecutiva)

🚀 Resultados y Hallazgos Iniciales

(Se completa después de seleccionar las empresas significativas.)
Ejemplo:

Solo 3–5 empresas pasaron más de un screener.

Varias empresas high growth tienen fundamentals impecables.

Buffett y Graham tienden a seleccionar compañías distintas, demostrando la diferencia entre inversión en calidad vs. valor.

El análisis de moats permitió descartar empresas con fundamentals buenos pero sin ventaja sostenible.

📈 Mejoras Futuras

Automatización completa del pipeline

Dashboard interactivo (Power BI / Tableau)

Agregar análisis de riesgos macro y sectoriales

Usar datos de múltiples fuentes para aumentar robustez

Incorporar estimaciones forward y DCF simple
