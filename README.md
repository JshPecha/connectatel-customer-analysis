# sprint7-final-project
# ConnectaTel: análisis y segmentación de clientes de telecomunicaciones

Análisis del comportamiento de 4,000 clientes de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica, para identificar patrones de uso, segmentar a los clientes y proponer estrategias de retención y mejoras en los planes.

## Contexto de negocio
ConnectaTel quiere entender cómo usan sus servicios los clientes para diseñar estrategias de retención y ajustar su oferta de planes. Este análisis responde: ¿cómo se comportan los clientes según su edad y nivel de uso?, ¿qué patrones atípicos existen?, ¿qué segmentos merecen atención?

## Datos (información registrada hasta 2024)
| Archivo | Contenido | Tamaño |
|---|---|---|
| `plans.csv` | 2 planes: Básico (USD 12) y Premium (USD 25), con minutos, mensajes y GB incluidos | 2 filas |
| `users_latam.csv` | Edad, ciudad, fecha de registro, plan y fecha de baja (churn) | 4,000 clientes |
| `usage.csv` | Detalle de llamadas y mensajes | 40,000 registros (17,908 llamadas y 22,092 mensajes) |

## Calidad de los datos
| Problema | Detalle | Acción |
|---|---|---|
| Edades con valor centinela -999 | 55 clientes (1.4%) | Reemplazo por la mediana (47 años) |
| Fechas de registro en 2026 | 40 clientes (1.0%), fuera del rango 2022–2024 | Marcadas como nulas |
| Ciudad con placeholder "?" | 96 registros; sumados a los 469 nulos originales, 565 sin ciudad (14.1%) | Reemplazo por nulo |
| Uso sin fecha | 50 registros (0.1%) | Se dejaron como nulos |
| Nulos en `duration` y `length` | Son esperados: los mensajes no tienen duración y las llamadas no tienen longitud | Se dejaron como nulos |

## Proceso
1. Carga y exploración de los tres datasets.
2. Revisión y estandarización de fechas con manejo de errores.
3. Corrección de valores centinela y evaluación de nulos según el tipo de registro.
4. Agregación del uso por cliente (mensajes, llamadas y minutos) y unión con los datos demográficos en `user_profile`.
5. Estadística descriptiva, histogramas y boxplots, con detección de outliers por el método IQR.
6. Segmentación por nivel de uso (Bajo, Medio y Alto) y por edad (Joven, Adulto y Adulto Mayor).

## Resultados
- **Planes:** 64.9% de los clientes tiene el plan Básico y 35.1% el Premium.
- **Uso promedio por cliente:** 5.5 mensajes, 4.5 llamadas y 23.3 minutos de llamada (mediana de 19.8 minutos).
- **Outliers:** hay valores atípicos en mensajes (límite IQR de 11.5; máximo de 17), llamadas (10.5; máximo de 15) y minutos (61.9; máximo de 155.7). La edad no tiene outliers. Se mantuvieron porque representan clientes de uso alto real, no errores de datos.
- **Distribución:** mensajes, llamadas y minutos tienen sesgo a la derecha (la media supera a la mediana).
- **Segmentos por uso:** [Bajo uso: X% · Uso medio: X% · Alto uso: X%]
- **Segmentos por edad:** [Joven: X% · Adulto: X% · Adulto Mayor: X%]
- **Comparación por plan:** [promedio de mensajes, llamadas y minutos de Básico frente a Premium]

## Conclusiones y recomendaciones
- **Retención:** diseñar campañas dirigidas a los clientes de alto uso, que son los más activos.
- **Bajo uso:** investigar por qué este segmento es tan amplio, ya que puede indicar oportunidad de upselling o riesgo de abandono.
- **Oferta de planes (hipótesis a validar):** el uso promedio registrado (23 minutos y 5.5 mensajes) está muy por debajo de lo que incluye el plan Básico (100 minutos y 100 mensajes), y más aún del Premium (600 minutos y 500 mensajes). Convendría revisar si el Premium se justifica por el uso observado o por los GB, que este dataset no mide.
- **Próximo paso:** analizar el churn (466 clientes con fecha de baja, el 11.7%) por plan y por segmento.

## Limitaciones
- El análisis es descriptivo y no incluye ingresos ni datos de consumo de GB.
- `usage.csv` cubre solo 2024 y no está claro si captura todo el uso real de cada cliente.

## Herramientas
Python (pandas, NumPy, Matplotlib, Seaborn), Jupyter Notebook, Google Colab.

## Cómo ejecutar el notebook
1. Abre [Google Colab](https://colab.research.google.com) y selecciona Archivo → Subir notebook, o la pestaña GitHub con la URL de este repositorio.
2. Sube `plans.csv`, `users_latam.csv` y `usage.csv` a la carpeta `/datasets/` del entorno de Colab.
3. Ejecuta las celdas en orden, de arriba hacia abajo: cada paso depende de las transformaciones anteriores.

Salida esperada: el DataFrame `user_profile` con los datos demográficos, las métricas de uso (`cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`) y las segmentaciones (`grupo_uso`, `grupo_edad`), más las visualizaciones de distribuciones y outliers.

## Autor
Julián Santiago Hernández Pecha · [LinkedIn](https://www.linkedin.com/in/julián-santiago-hernández-pecha) · [GitHub](https://github.com/JshPecha)
