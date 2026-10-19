# Tareas de implementación: Dashboard de rendimiento del negocio

Las tareas se derivan de [`spec.md`](./spec.md) y [`plan.md`](./plan.md). Se ordenan según dependencias: primero preparación de datos y estilos, después estructura y bloques del dashboard, y finalmente revisión.

## Preparación

- [x] Revisar `mockdata.csv` y alinearlo con el esquema objetivo de `spec.md`.
- [x] Validar las relaciones de los datos: importe de ventas = precio unitario × unidades vendidas; comisión = 15 % del importe.
- [x] Incorporar o comprobar los campos mock requeridos para inscripciones, actividad, interacciones, calidad, satisfacción y tiempos.
- [x] Configurar Tailwind CSS v4 de acuerdo con el entorno existente, sin CDN de Tailwind ni configuración/snippets de v3.
- [x] Definir las convenciones visuales y clases repetibles para tarjetas KPI, widgets, alertas y tablas.

## Estructura semántica y diseño

- [x] Crear el esqueleto HTML con `<header>`, `<nav>` si aplica, `<main>`, secciones y `<footer>` opcional.
- [x] Añadir título, período cubierto y aviso de que los datos son ficticios.
- [x] Establecer jerarquía de encabezados, navegación interna y orden de lectura accesible.
- [x] Aplicar layout mobile first con breakpoints `sm:`, `md:` y `lg:`, evitando dimensiones fijas en píxeles.

## Bloque 1: KPIs

- [x] Crear exactamente tres tarjetas KPI principales: Volumen, Ingresos y Engagement.
- [x] Mostrar en Volumen el importe total de ventas en euros, inscripciones, usuarios activos e impresiones.
- [x] Mostrar en Ingresos comisiones, ventas brutas y precio medio por unidad, todos con formato/unidad adecuados.
- [x] Indicar MRR como “No disponible” por ausencia de datos de suscripciones recurrentes.
- [x] Mostrar interacciones y tasa de engagement con la fórmula definida en `spec.md`.
- [x] Mostrar la tasa de conversión porcentual como unidades vendidas / impresiones × 100, y diferenciarla visualmente del CTR.

## Bloque 2: Drivers

- [x] Crear el driver de Conversión con el funnel impresiones → clics → inscripciones → unidades vendidas.
- [x] Mostrar tasas por etapa solo si el numerador y denominador son válidos; etiquetar claramente cada denominador.
- [x] Crear el driver de Rendimiento de plataforma con alcance, impresiones, clics, inscripciones, ventas en euros, conversiones y coste.
- [x] Mostrar CTR y engagement por plataforma con sus fórmulas correspondientes.
- [x] Crear el driver de Actividad con posts, stories, reels y videos por plataforma.
- [x] Incorporar métricas de calidad: inscripciones cualificadas, attendance rate y pass rate, declarando denominadores.
- [x] Incorporar desglose de interacciones por plataforma: me gusta, comentarios, compartidos y guardados.
- [x] Incorporar resumen de rendimiento por producto, destacando comisiones y tasa de conversión comparables.

## Bloque 3: Detalles operacionales

- [x] Crear el área de Alertas para caídas, picos y anomalías, con período, métrica, cambio y criterio explícitos.
- [x] Evitar alertas cuando los datos no sean suficientes o los períodos no sean comparables.
- [x] Crear la tabla de productos con precio unitario, unidades vendidas, ventas en euros, comisiones, coste y ROI cuando se pueda calcular.
- [x] Crear una comparación o tabla de plataformas con alcance, engagement y conversiones/tasa de conversión.
- [x] Crear la tabla de campañas con fechas derivadas de los registros, productos, plataformas, inscripciones, ventas en euros, comisiones y coste.
- [x] Añadir vistas o filtros demostrativos para top productos, top plataformas, top campañas y oportunidades de mejora.
- [x] Especificar en cada ranking la métrica y el período usados para ordenar.

## Accesibilidad, formato y validación

- [x] Formatear importes como euros y tasas como porcentajes; diferenciar ventas brutas, comisiones y costes.
- [x] Mostrar “No disponible” para métricas ausentes, sin representarlas como cero.
- [x] Usar tablas semánticas con `<thead>`, `<tbody>`, encabezados asociados y presentación utilizable en pantallas estrechas.
- [x] Asegurar que alertas y estados no dependan únicamente del color.
- [x] Revisar la página en tamaños móvil, `sm:`, `md:` y `lg:`.
- [x] Comprobar la consistencia aritmética de ventas, comisiones, CTR, conversión y engagement respecto de `mockdata.csv`.
- [x] Verificar los tres bloques, las tres tarjetas KPI, los tres drivers y las tres áreas operacionales requeridas.
- [x] Confirmar que la solución usa únicamente HTML y Tailwind CSS v4, sin JavaScript, React, Vue, CDN de Tailwind ni bibliotecas externas.
