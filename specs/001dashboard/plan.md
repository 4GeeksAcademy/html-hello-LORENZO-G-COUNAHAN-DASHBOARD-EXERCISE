# Plan de implementación: Dashboard de rendimiento del negocio

## 1. Propósito

Este documento describe el enfoque técnico y la organización de la implementación del dashboard especificado en [`spec.md`](./spec.md). Define arquitectura, estrategia de datos, composición visual y validación. No contiene tareas ni una lista de trabajo; estas se documentarán por separado.

## 2. Principios de implementación

- Mantener una página estática basada únicamente en HTML semántico y Tailwind CSS v4.
- No incorporar JavaScript, frameworks de interfaz, bibliotecas de gráficos, servicios externos ni lógica de backend.
- Tratar `mockdata.csv` como fuente de datos ficticios para la presentación; no modificar ni inferir el significado de las métricas más allá de las reglas documentadas en la especificación.
- Priorizar lectura rápida, consistencia de componentes visuales, accesibilidad y comportamiento mobile first.
- Distinguir siempre ventas brutas en euros, comisiones en euros y costes en euros.

## 3. Arquitectura de la página

La página se organizará como un único documento con estructura semántica:

- `<header>`: nombre del dashboard, período cubierto y contexto de que los datos son de demostración.
- `<nav>`: enlaces internos a los tres bloques principales, si se incluye navegación.
- `<main>`: área principal que contendrá las secciones del dashboard en el orden de lectura previsto.
- `<section>` de resumen KPI, con exactamente tres tarjetas principales: Volumen, Ingresos y Engagement.
- `<section>` de drivers, con tres widgets principales: Conversión, Rendimiento de plataforma y Actividad.
- `<section>` operacional, con Alertas, Tabla de productos y Tabla de campañas. La vista comparativa de plataformas y los rankings solicitados se integrarán en los widgets operacionales adecuados, sin crear bloques principales adicionales.
- `<footer>` opcional para nota metodológica, definiciones y aviso de datos ficticios.

Los encabezados de sección y widget mantendrán una jerarquía coherente (`h1`, `h2`, `h3`) y una secuencia lógica para navegación asistida.

## 4. Estrategia de datos y métricas

### Fuente y forma de presentación

`mockdata.csv` es el contrato de datos de la maqueta y debe ajustarse al esquema objetivo de `spec.md`. La visualización debe agrupar y resumir registros según período, producto, plataforma o campaña sin cambiar la semántica de los campos. Como la solución se limita a HTML y CSS, el dashboard será una representación estática: los resúmenes, rankings, alertas y valores mostrados deben estar preparados como contenido de presentación, sin prometer filtros o cálculos interactivos derivados del CSV en tiempo de ejecución.

### Reglas de agregación

- Ventas en euros: suma de `ventas_eur`, derivada de precio unitario por unidades vendidas. El KPI y las tablas presentan el importe monetario; el número de unidades se conserva como dato auxiliar para fórmulas y desglose por producto.
- Comisiones en euros: suma de `comisiones_eur`, equivalente al 15 % de las ventas brutas.
- Precio medio por unidad: ventas en euros divididas por unidades vendidas.
- CTR: clics divididos por impresiones, expresado en porcentaje.
- Conversión del ejercicio: unidades vendidas divididas por impresiones, expresado en porcentaje; aplicar el mismo período y agrupación a numerador y denominador. No confundir esta métrica con CTR ni con conversión clic-a-venta.
- Engagement: suma de me gusta, comentarios, compartidos y guardados, dividida por alcance, expresada en porcentaje.
- Calidad: inscripciones cualificadas sobre inscripciones; attendance rate como asistencias sobre inscripciones; pass rate como procesos completados sobre el denominador declarado por el negocio. En cada tasa se debe especificar el denominador.
- NPS y CSAT: obtenerlos de sus respectivos conteos de respuestas, usando las fórmulas estándar y mostrando la escala o unidad apropiada.
- Coste por resultado y ROI: utilizar el campo de coste y declarar qué resultado o ingreso se toma como base. Para ROI sobre comisiones, usar la fórmula propuesta en `spec.md` cuando el coste sea mayor que cero.
- Actividad: sumar posts, stories, reels y videos en su propia categoría por plataforma y período.

### Métricas no disponibles

MRR, churn y permanencia no se derivan del conjunto mock sin datos de suscripción recurrente y de fechas de altas/bajas. Se mostrarán como “No disponible” o se excluirán conforme a la composición aprobada, nunca como cero. Si cualquier métrica opcional carece de datos o denominador válido, se aplicará el mismo tratamiento y se evitará presentar un resultado calculado engañosamente.

## 5. Sistema visual y patrones reutilizables

La consistencia se logrará con convenciones repetibles de markup y grupos de clases Tailwind v4, sin introducir una tecnología de componentes:

- **Tarjeta KPI:** título, valor destacado con unidad, período y variación/contexto si existe una comparación válida.
- **Widget:** encabezado, resumen breve, visualización estática y nota metodológica o leyenda.
- **Alerta:** nivel expresado con texto, indicador implicado, período, valor o cambio y criterio que la activa. El color será complementario, no la única señal.
- **Tabla:** título, descripción o período, encabezados semánticos, alineación coherente de valores y tratamiento responsive.
- **Estados vacíos/no disponibles:** mensaje explícito y, si aporta valor, explicación del dato faltante.

La paleta, espaciado, bordes, tipografía y énfasis se aplicarán de forma consistente a los tres bloques. Las cantidades se formatearán en euros o porcentajes según corresponda, y las cifras comparables conservarán la misma precisión.

## 6. Diseño responsive y accesibilidad

La estructura partirá de una columna para pantallas pequeñas. Los widgets y tarjetas podrán distribuirse en varias columnas en `sm:`, `md:` y `lg:` a medida que haya espacio. No se fijarán tamaños en píxeles; se usarán utilidades de Tailwind para anchos flexibles, espaciado y adaptación.

Las tablas conservarán legibilidad en móvil mediante una región de desplazamiento horizontal contenida o una disposición responsive que preserve los encabezados y la asociación entre celdas. El orden visual no debe alterar el orden semántico.

Se emplearán etiquetas HTML semánticas, texto con contraste adecuado, encabezados de tabla explícitos, enlaces descriptivos, foco visible para elementos navegables y texto accesible para alertas e indicadores. No se dependerá solamente del color para expresar estados o tendencias.

## 7. Criterios para alertas y comparaciones

Las alertas de caídas, picos y anomalías deben basarse en períodos comparables y un umbral explícito, definido para la maqueta. Un aviso incluirá nombre de la métrica, período afectado y magnitud o descripción del cambio. No se presentarán alertas como conclusiones estadísticas reales, dado que el archivo contiene datos mock.

Los rankings de productos, plataformas y campañas indicarán el criterio de orden (por ejemplo, comisiones, tasa de conversión o alcance) y el período analizado. “Mejor plataforma” y “mejor producto” solo se destacarán entre elementos comparables según esa misma métrica.

## 8. Alcance técnico y limitaciones

El producto de este plan es una interfaz estática. HTML y Tailwind CSS por sí solos no leen, agregan ni filtran automáticamente un archivo CSV, ni producen gráficas dinámicas o alertas calculadas al cambiar filtros. Por lo tanto, el contenido que se muestre debe estar preparado de antemano en el documento a partir del mock data, y los filtros podrán representarse visualmente como controles no funcionales o como enlaces/estados estáticos, siempre que se identifiquen como demostrativos.

La implementación usará Tailwind CSS v4 y no utilizará `cdn.tailwindcss.com`, snippets de Tailwind v3, React, Vue, JavaScript ni bibliotecas externas. Si en una fase posterior se requiere interactividad o cálculos directos sobre el CSV, se deberá revisar el alcance técnico y actualizar la especificación antes de añadir tecnologías.

## 9. Enfoque de validación

La revisión de la implementación se centrará en:

- Correspondencia entre los datos mostrados y los campos/formulas definidos en `spec.md`.
- Coherencia aritmética de ventas en euros, comisiones y tasas porcentuales.
- Presencia de los tres bloques principales, los tres KPIs, los tres drivers y las tres áreas operacionales.
- Ausencia de métricas presentadas como reales cuando no tienen fuente o denominador válido.
- Cumplimiento de HTML semántico, Tailwind CSS v4, diseño mobile first y breakpoints `sm:`, `md:` y `lg:`.
- Legibilidad, accesibilidad y uso correcto de tablas en tamaños de pantalla variados.
- Ausencia de dependencias y tecnologías fuera del alcance aprobado.
