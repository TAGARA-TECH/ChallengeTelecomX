# ChallengeTelecomX
Desafío Telecom X
El desafío Telecom X ofrece una oportunidad única para aplicar habilidades esenciales de análisis de datos en un escenario de negocios real.

Aplicación práctica del conocimiento
La limpieza y tratamiento de datos es una habilidad fundamental para cualquier analista de datos. La manipulación de grandes volúmenes de información exige la capacidad de identificar y corregir inconsistencias en los datos, como valores nulos, duplicados y datos fuera de estándar. Garantizar que los datos estén listos para el análisis es un paso esencial para obtener resultados precisos y confiables.

El análisis exploratorio de datos (EDA) es una etapa crucial para comprender en profundidad los datos. La capacidad de aplicar estadísticas descriptivas y generar visualizaciones permite identificar patrones, tendencias y relaciones entre las variables. Esto ayuda a formular hipótesis y generar insights que pueden influir en decisiones estratégicas dentro de la empresa.

Al participar en este desafío, aplicarás conocimientos esenciales para el análisis de grandes volúmenes de datos en un contexto real, donde tus hallazgos podrán impactar directamente en las estrategias de la empresa para mejorar el principal problema que están enfrentando.

Este desafío no solo contribuye a tu crecimiento en el área de Data Science, sino que también ofrece la oportunidad de entender cómo la ciencia de datos puede aplicarse para resolver problemas reales que enfrentan las empresas en el mercado.

# Sobre el desafio
Transcripción del video explicativo
Bienvenidos a este desafío de Telecom X, análisis y evasión de clientes. Mi nombre es Wilfredo Rojas, instructor en el programa One. Para efectos de accesibilidad, me describiré a mí mismo. Soy una persona morena con cabello negro. Llevo puesta una camiseta negra con una palabra que dice Alura. Detrás de mí hay una pared blanca con colores azul y rosa.

Telecom X es una empresa de telecomunicaciones y has sido contratado como analista de datos para trabajar en un proyecto específico de Churn de clientes. La empresa está enfrentando un alto índice de evasión de clientes y aún no han identificado el problema de esta evasión. Para ello, te han proporcionado algunos datos en los que tendrás que buscar, tratar y realizar un análisis exploratorio para, una vez limpiados estos datos, poder proporcionárselos al equipo de ciencia de datos. De esta manera, podrán realizar un análisis predictivo y determinar de dónde proviene esta evasión de clientes.

Como siempre, te hemos proporcionado un tablero de Trello donde tenemos esta primera columna llamada "Material de apoyo" donde hay algunos consejos sobre cómo utilizar Trello. Y tenemos esta columna mucho más importante llamada "Listo para iniciar". En esta columna tenemos tarjetas donde está el paso a paso para que puedas completar este desafío.

Una parte muy importante es esta sección de extracción de datos donde estamos proporcionando una API que te entregará un JSON desde donde tendrás que extraer estos datos en tu Google Colab. Una vez que hayas completado todos los pasos del desafío, tendrás que venir a esta sección, esta tarjeta de informe final y tendrás que escribir tus conclusiones, dar las razones por las cuales crees que está ocurriendo esta evasión de clientes.

En este desafío, pondrás en práctica todas las habilidades que has adquirido hasta ahora a través de los cursos de ETL, por ejemplo, y de análisis exploratorio y podrás utilizar herramientas como Python, Pandas y Matplotlib. Estos te ayudarán a completar el desafío. Así que, ¡éxito en este desafío y manos a la obra!

#03
Preparando el ambiente
 Siguiente pregunta

Enfoque en el Proceso de ETL
El principal objetivo de este desafío es desarrollar tus habilidades en ETL (Extract, Transform, Load) con Python. Los datos para este desafío están disponibles en una API.

Accede a los datos de la API
TelecomX Datos
En este desafío, el enfoque está en el proceso de extracción de datos desde la API, limpieza y transformación. Después de esta etapa de procesamiento, deberás organizar los datos de manera que permitan análisis más profundos y visualizaciones.

Como apoyo adicional, hemos creado un cuaderno base opcional para ayudarte a estructurar mejor el desafío. Este cuaderno proporciona una base inicial, sugiriendo un flujo de trabajo organizado y buenas prácticas para el proceso de ETL. Puedes usarlo como guía durante la ejecución del desafío, pero también tienes la opción de no utilizarlo y estructurar la solución por tu cuenta.

Accede al cuaderno base (opcional)
Cuaderno Base - Telecom X
Al completar este desafío, habrás aplicado competencias esenciales en ETL, fundamentales para el trabajo de un analista de datos y otras áreas dentro de Data Science.


**Repositorio** https://github.com/alura-cursos/challenge2-data-science-LATAM/tree/main

**Trello** https://trello.com/b/hJk1ior5/telecomxlatam
Descripción
Editar
Telecom X - Análisis de Evasión de Clientes
Has sido contratado como asistente de análisis de datos en Telecom X y formarás parte del proyecto "Churn de Clientes". La empresa enfrenta una alta tasa de cancelaciones y necesita comprender los factores que llevan a la pérdida de clientes.

Tu desafío será recopilar, procesar y analizar los datos, utilizando Python y sus principales bibliotecas para extraer información valiosa. A partir de tu análisis, el equipo de Data Science podrá avanzar en modelos predictivos y desarrollar estrategias para reducir la evasión.

¿Qué vas a practicar?
✅ Importar y manipular datos desde una API de manera eficiente.
✅ Aplicar los conceptos de ETL (Extracción, Transformación y Carga) en la preparación de los datos.
✅ Crear visualizaciones estratégicas para identificar patrones y tendencias.
✅ Realizar un Análisis Exploratorio de Datos (EDA) y generar un informe con insights relevantes.

¡Ahora es tu turno! 🚀 Usa tus conocimientos para transformar datos en información estratégica y ayudar a Telecom X a retener más clientes.

**INICIANDO EL PROYECTO**
Etapas del Proyecto
Comprensión del problema

Objetivo: Entender por qué los clientes se van.

Entregables esperados: Informe exploratorio, visualizaciones, insights clave.

Recolección de datos

¿Tenés un archivo .csv o base de datos? Si no, puede que esté en otra tarjeta de Trello.

Si me lo podés subir o copiar la estructura, mejor.

Limpieza y procesamiento de datos (Data Wrangling)

Eliminar o imputar valores nulos.

Corregir tipos de datos (fechas, números, categóricos).

Crear variables nuevas si es necesario (por ejemplo, antigüedad del cliente).

Análisis exploratorio de datos (EDA)

Distribuciones, correlaciones, outliers.

Visualización de patrones de churn.

Insights

¿Qué factores se relacionan más con la evasión?

¿Qué segmentos de clientes tienen más riesgo?

Informe o notebook

Código limpio, visualizaciones, conclusiones.

🧰 Herramientas recomendadas
Python con:

pandas

numpy

matplotlib y seaborn

scikit-learn (si se llega a algo más predictivo)



