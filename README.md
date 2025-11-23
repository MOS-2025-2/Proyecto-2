# Proyecto 2 – Optimización Logística para LogistiCo

- **Curso:** ISIS-3302 – Departamento de Ingeniería de Sistemas, Universidad de los Andes
- **Etapa:** Proyecto II – Implementación en Pyomo
- **Equipo:** Miguel Florez G., Diego Ortiz R., Manuel S. Prieto H.

## Estructura del repositorio
- `Proyecto2.ipynb`: notebook principal con documentación, preprocesamiento, formulación Pyomo y ejecución de los casos.
- `data/caso_base/`: insumos oficiales del caso base (`clientes.csv`, `vehicles.csv`, `depots.csv`, `parameters_base.csv`).
- `data/Caso2/`: datos para el Caso 2 multi-depósito urbano.
- `data/Caso3/`: datos para el Caso 3 escala realista avanzada (60 clientes, 15 vehículos, 5 depósitos).
- `verificacion_caso1.csv`, `verificacion_caso2.csv`, `verificacion_caso3.csv`: archivos de verificación generados automáticamente.
- `mapa_caso1.html`, `mapa_caso2.html`, `mapa_caso3.html`: mapas interactivos de rutas.
- `metricas_caso1.png`, `metricas_caso2.png`, `metricas_caso3.png`: dashboards de análisis.
- `CASO3_QUICK_START.md`: guía rápida de ejecución del Caso 3.
- `CASO3_IMPLEMENTATION_SUMMARY.md`: documentación técnica completa del Caso 3.
- `README.md`: guía de ejecución y registro de resultados.

## Requisitos previos
1. Python 3.11+ con los paquetes: `pyomo`, `pandas`, `numpy`, `folium`, `matplotlib`.
2. Solver GLPK disponible en la ruta del sistema (se usa `SolverFactory('glpk')`).
3. Mantener la estructura de carpetas para que `cargar_datos()` encuentre los CSV estandarizados.

## Cómo ejecutar el Caso 1 (CVRP base)
1. Abrir el notebook y ejecutar las celdas en orden hasta `consola()`.
2. El flujo hace: procesamiento → construcción del modelo → resolución con GLPK → extracción de rutas → generación de archivo de verificación → mapas y gráficos.
3. Al finalizar se actualizan `verificacion_caso1.csv`, `mapa_caso1.html` y `metricas_caso1.png`.

## Cómo ejecutar el Caso 2 (Multi-Depósito Urbano)
1. Abrir el notebook y ejecutar las celdas hasta la sección de Caso 2.
2. Ejecutar `consola_caso2()` para el flujo completo.
3. Se generan `verificacion_caso2.csv`, `mapa_caso2.html` y `metricas_caso2.png`.

## Cómo ejecutar el Caso 3 (Escala Realista Avanzada)
1. Abrir el notebook y navegar hasta la sección de Caso 3 (células 32-55).
2. Ejecutar todas las celdas de funciones hasta llegar a la celda de ejecución.
3. Ejecutar:
   ```python
   datos3, modelo3, rutas3, depot_stats3, vehicle_stats3, global_stats3, sens3 = consola_caso3()
   ```
4. **Tiempo estimado:** 15-20 minutos (incluye análisis de sensibilidad con 7 escenarios).
5. Se generan automáticamente:
   - `verificacion_caso3.csv`: archivo oficial de verificación
   - `mapa_caso3.html`: mapa interactivo con 60 clientes y rutas
   - `metricas_caso3.png`: dashboard con 9 gráficos de análisis

Para más detalles, consultar:
- `CASO3_QUICK_START.md`: guía rápida de ejecución
- `CASO3_IMPLEMENTATION_SUMMARY.md`: documentación técnica completa
- `data/Caso3/README.md`: especificación de requerimientos

## Resultados actuales (Caso 1)
- **Datos cargados:** 24 clientes, 8 vehículos, 1 depósito, 600 arcos, demanda total 377 unidades vs capacidad 839 unidades.
- **Solver:** GLPK 5.0, tiempo límite 301.40 s. Se obtuvo solución factible con costo **$890,458.08 COP**, uso de 3 vehículos y brecha reportada de 69.4 % respecto a la cota inferior (272,878 COP).

## Caso 3 - Características Principales
- **Escala:** 60 clientes, 15 vehículos, 5 depósitos
- **Innovación:** Restricciones de vehículos por zona (VehicleSizeRestriction)
- **Análisis:** Sensibilidad ±20% en combustible, capacidad de depósitos y demanda
- **Reportes:** Estadísticas a nivel depósito, vehículo y global
- **Visualizaciones:** Mapa interactivo + dashboard de 9 gráficos
- **Business Insights:** Recomendaciones estratégicas con análisis de ROI
- **Modelo:** 62,400 variables binarias + 975 continuas, ~5,000 restricciones