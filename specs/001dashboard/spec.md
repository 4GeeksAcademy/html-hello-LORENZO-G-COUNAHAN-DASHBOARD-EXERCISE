# Especificación: Dashboard de rendimiento del negocio

## 1. Objetivo

Definir un dashboard visual, responsive y de lectura rápida que presente los indicadores comerciales y de marketing del negocio a partir de `mockdata.csv`. El dashboard se realizará únicamente con HTML semántico y Tailwind CSS v4. Esta especificación define los requisitos; **no incluye la implementación**.

## 2. Alcance

- Mostrar los datos del archivo `mockdata.csv` en tres bloques principales: KPIs, drivers de rendimiento y detalles operacionales.
- Facilitar la comparación por producto, plataforma, campaña y período, en la medida en que existan datos en el CSV.
- Proporcionar una visualización de ejemplo clara. Los datos del CSV son de mock y no representan resultados comerciales reales.
- No se requiere conexión a servicios externos, procesamiento de backend ni integración con plataformas.

## 3. Fuente de datos y reglas de interpretación

La fuente será `mockdata.csv`. Para cubrir las visualizaciones, el archivo debe contener las columnas base y las columnas adicionales indicadas abajo. El CSV existente debe ampliarse y normalizarse antes de implementar el dashboard. Esquema objetivo:

`fecha, producto, items_vendidos, ventas_eur, comisiones_eur, plataforma, campaña, impresiones, alcance, clics, inscripciones, coste_eur, me_gusta, comentarios, compartidos, guardados, posts, stories, reels, videos, usuarios_activos, inscripciones_cualificadas, asistencias, procesos_completados, nps_promotores, nps_pasivos, nps_detractores, csat_respuestas_satisfechas, csat_respuestas_totales, tiempo_conversion_horas`

Productos y precios de referencia:

| Producto | Precio unitario | Comisión por unidad (15 %) |
|---|---:|---:|
| Producto A | 50 € | 7,50 € |
| Producto B | 120 € | 18,00 € |
| Producto C | 80 € | 12,00 € |

Reglas:

- `items_vendidos` es el número de unidades vendidas usado como dato de entrada para los cálculos. El valor monetario de ventas se calcula como `precio_unitario × items_vendidos` y se almacena en `ventas_eur`; en la visualización de ventas se presenta el importe en euros, no el conteo de unidades.
- Los precios unitarios son 50 €, 120 € y 80 € para los productos A, B y C respectivamente. La comisión de referencia es el 15 % del importe vendido; `comisiones_eur` debe ser igual a `ventas_eur × 0,15`.
- El precio medio de venta por unidad se calculará como `ventas_eur / items_vendidos`, cuando haya unidades vendidas.
- CTR se calculará como `clics / impresiones × 100`, cuando haya impresiones.
- La tasa de conversión definida para este ejercicio es `items_vendidos / impresiones × 100`, calculada por producto y período, y se presenta como porcentaje. No es lo mismo que CTR ni que la conversión clic-a-venta.
- La tasa de engagement es `(me_gusta + comentarios + compartidos + guardados) / alcance × 100`, cuando el alcance sea mayor que cero.
- `inscripciones` es la métrica del funnel; no hay leads en este ejercicio. La inscripción es un paso anterior y distinto a la venta.
- Para que todas las vistas de demostración presenten valores, el CSV debe ampliarse con datos mock de actividad e interacciones, usuarios activos, calidad/asistencia/compleción, NPS, CSAT y tiempos de conversión. Dichos valores deben identificarse como ficticios.
- No hay datos de suscripciones recurrentes ni bajas: MRR y churn no se pueden calcular y se mostrarán como “No disponible”. La permanencia requiere fechas de alta y baja y tampoco se inferirá si faltan.
- ROI solo se mostrará si se define su fórmula y existen datos suficientes. Como propuesta, usar `(comisiones atribuibles − coste) / coste × 100`; presentar el resultado como ROI sobre comisiones, no sobre ventas brutas. Si no puede calcularse, indicar “No disponible”.
- Las fechas y los períodos agregados deben derivarse del CSV. El usuario debe poder identificar el período mostrado.

## 4. Estructura del dashboard

El documento incluirá un `<header>` con el título y contexto del informe, una `<nav>` para navegación entre bloques si corresponde, y un `<main>` que contenga las secciones del dashboard. Cada bloque estará agrupado en una `<section>` con encabezado descriptivo.

### Bloque 1: KPIs

Se mostrarán **exactamente tres tarjetas KPI principales**, con jerarquía visual consistente:

1. **Volumen**
   - Ventas totales en euros (`ventas_eur`).
   - Inscripciones.
   - Usuarios activos.
   - Impresiones totales.
2. **Ingresos**
   - Comisiones generadas en euros (ingreso del negocio).
   - Ventas brutas en euros.
   - Precio medio de venta.
   - MRR: no disponible porque no existen suscripciones recurrentes.
3. **Engagement**
   - Tasa de engagement e interacciones totales a partir de las columnas de interacción.
   - Tasa de conversión porcentual: unidades vendidas / impresiones × 100, con definición visible.
   - Si no hay datos suficientes, informar “No disponible” sin mostrar valores inventados.

Las métricas adicionales indicadas en el requerimiento inicial —retención, rendimiento, satisfacción y eficiencia— podrán incluirse como indicadores secundarios dentro de las mismas tarjetas o en un área auxiliar del primer bloque, sin crear una cuarta tarjeta KPI principal. Se aplican las limitaciones de disponibilidad de datos descritas en la sección 3.

### Bloque 2: Drivers

Se mostrarán **tres drivers principales**:

1. **Conversión**
   - Funnel por etapas: impresiones → clics → inscripciones → unidades vendidas.
   - Mostrar cantidades y tasas entre etapas únicamente cuando el denominador exista.
2. **Rendimiento de plataforma**
   - Comparación de plataformas disponibles en el CSV (por ejemplo, Instagram, TikTok, YouTube y Facebook si aparecen en los datos).
   - Métricas: alcance, impresiones, clics, inscripciones, importe vendido, conversión, coste y CTR cuando sean calculables.
3. **Actividad**
   - Publicaciones, stories, reels y videos por plataforma.

Dentro de estos drivers también se presentarán la calidad de inscripciones (ratio cualificado, attendance rate y pass rate), el rendimiento por producto y las métricas de engagement por plataforma (me gusta, comentarios, compartidos y guardados).

### Bloque 3: Detalles operacionales

Se mostrarán **tres áreas operacionales principales**:

1. **Alertas**
   - Señalar caídas relevantes de conversión, picos y anomalías comparando períodos equivalentes, solo cuando haya suficiente historial.
   - Las alertas deben explicar el indicador, período y criterio aplicado.
   - No generar alertas basadas en datos ausentes o no comparables.
2. **Tabla de productos**
   - Columnas mínimas: producto, precio unitario, unidades vendidas, ventas en euros, comisiones en euros, coste y ROI cuando sea calculable.
   - Permitir identificar el producto con más comisiones y el de mejor conversión, indicando la métrica y el período.
3. **Tabla de campañas**
   - Columnas mínimas: campaña, fechas, producto(s) promocionado(s), plataforma, inscripciones, unidades vendidas, ventas en euros, comisiones e inversión/coste.
   - Las fechas se obtendrán de los registros; no se inventarán períodos de campaña no disponibles.

Se incluirá también una tabla o vista de plataformas con alcance, engagement y conversiones, destacando la mejor plataforma por métrica cuando haya datos comparables. Los listados de “top productos”, “top plataformas”, “top campañas” y “oportunidades de mejora” podrán implementarse como filtros o vistas dentro de estas áreas; cada ranking deberá especificar la métrica que ordena los resultados.

## 5. Diseño y requisitos técnicos

- Utilizar **Tailwind CSS v4**.
- No utilizar `cdn.tailwindcss.com`, snippets de Tailwind v3 ni sintaxis de configuración exclusiva de Tailwind v3.
- Utilizar únicamente HTML y Tailwind CSS. No utilizar React, Vue ni otros frameworks o tecnologías de interfaz.
- Utilizar HTML semántico y etiquetas adecuadas, incluyendo `<header>`, `<nav>`, `<main>`, `<section>`, tablas con `<table>`, `<thead>`, `<tbody>`, `<th>` y `<td>`, además de `<footer>` si se necesita.
- El diseño debe ser **mobile first**, con breakpoints responsive de Tailwind (`sm:`, `md:`, `lg:`) para adaptar columnas, tablas y navegación.
- No fijar dimensiones en píxeles; usar unidades y utilidades responsive de Tailwind.
- Mantener consistencia visual mediante patrones reutilizables de presentación para tarjetas KPI, cajas de widgets, alertas y tablas. La repetición se resolverá con markup y clases consistentes; no se añadirá un framework de componentes.
- Asegurar contraste legible, jerarquía visual clara, encabezados descriptivos y etiquetas/unidades junto a cada métrica.
- Las tablas deben permanecer utilizables en pantallas estrechas, por ejemplo mediante desplazamiento horizontal contenido o una presentación responsive apropiada.

## 6. Requisitos de visualización y accesibilidad

- Cada cifra debe tener etiqueta, período y unidad cuando corresponda.
- Mostrar importes en euros con formato coherente.
- Diferenciar visualmente ventas brutas, comisiones y costes.
- No comunicar como cero una métrica ausente; usar “No disponible” y, cuando sea útil, explicar qué dato falta.
- Utilizar encabezados de tabla asociados a sus columnas y mantener el orden semántico del contenido.
- Los estados de alerta no dependerán exclusivamente del color: incluir texto o icono con nombre accesible.

## 7. Fuera de alcance

- Ingesta automática o actualización en tiempo real de datos.
- Autenticación, persistencia, filtros con lógica de backend o conexión a APIs de redes sociales.
- Cálculo de métricas cuya fuente o fórmula no se haya definido.
- Generación de datos ficticios adicionales para cubrir campos ausentes del CSV.

## 8. Criterios de aceptación

- [ ] Existe una especificación del dashboard basada en `mockdata.csv` y distingue claramente datos disponibles de métricas no disponibles.
- [ ] El dashboard previsto tiene tres bloques principales y exactamente tres KPIs principales: Volumen, Ingresos y Engagement.
- [ ] El segundo bloque presenta tres drivers: Conversión, Rendimiento de plataforma y Actividad.
- [ ] El tercer bloque presenta tres áreas operacionales: Alertas, tabla de productos y tabla de campañas; también contempla el análisis de plataformas y rankings/filtros solicitados.
- [ ] Las métricas calculadas definen sus fórmulas y denominadores; no se inventan datos para MRR, satisfacción, retención, actividad o interacciones.
- [ ] El diseño especificado es mobile first y utiliza breakpoints `sm:`, `md:` y `lg:`, sin tamaños fijados en píxeles.
- [ ] La implementación futura utilizará HTML semántico y exclusivamente HTML más Tailwind CSS v4; no usará CDN de Tailwind ni frameworks de interfaz.
- [ ] Se especifican patrones visuales reutilizables para KPIs, widgets, alertas y tablas.
