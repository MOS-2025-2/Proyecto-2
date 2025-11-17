# Proyecto 2 – Optimización Logística para LogistiCo

- **Curso:** ISIS-3302 – Departamento de Ingeniería de Sistemas, Universidad de los Andes
- **Etapa:** Proyecto II – Implementación en Pyomo
- **Equipo:** Miguel Florez G., Diego Ortiz R., Manuel S. Prieto H.

## Estructura del repositorio
- `Proyecto2.ipynb`: notebook principal con documentación, preprocesamiento, formulación Pyomo y ejecución de los casos.
- `data/caso_base/`: insumos oficiales de los casos (`clientes.csv`, `vehicles.csv`, `depots.csv`, `parameters_base.csv`).
- `verificacion_caso1.csv`: archivo de rastreo generado automáticamente tras correr el Caso 1.
- `mapa_caso1.html`, `metricas_caso1.png`: visualizaciones exportadas desde el notebook.
- `README.md`: guía de ejecución y registro de resultados.

## Requisitos previos
1. Python 3.11+ con los paquetes: `pyomo`, `pandas`, `numpy`, `folium`, `matplotlib`.
2. Solver GLPK disponible en la ruta del sistema (se usa `SolverFactory('glpk')`).
3. Mantener la estructura de carpetas para que `cargar_datos()` encuentre los CSV estandarizados.

## Cómo ejecutar el Caso 1 (CVRP base)
```pwsh
cd "c:\Users\Manuel\OneDrive - Universidad de los andes\Documentos\Modelado\Proyecto 2\Proyecto-2"
jupyter notebook Proyecto2.ipynb
```
1. Abrir el notebook y ejecutar las celdas en orden hasta `consola()`.
2. El flujo hace: procesamiento → construcción del modelo → resolución con GLPK (300 s, mipgap 1 %) → extracción de rutas → generación de archivo de verificación → mapas y gráficos.
3. Al finalizar se actualizan `verificacion_caso1.csv`, `mapa_caso1.html` y `metricas_caso1.png`.

## Resultados actuales (Caso 1)
- **Datos cargados:** 24 clientes, 8 vehículos, 1 depósito, 600 arcos, demanda total 377 unidades vs capacidad 839 unidades.
- **Solver:** GLPK 5.0, tiempo límite 301.40 s. Se obtuvo solución factible con costo **$890,458.08 COP**, uso de 3 vehículos y brecha reportada de 69.4 % respecto a la cota inferior (272,878 COP).
- **Rutas extraídas:**
	- `V001`: CD01-C002-C018-C008-C005-C011-C009-C006-CD01 (67.14 km, 161 min, utilización 93.1 %).
	- `V002`: CD01-C023-C014-C003-C013-C022-C021-C007-C012-C020-CD01 (74.13 km, 178 min, utilización 99.3 %).
	- `V003`: CD01-C015-C004-C001-C010-C016-C024-C017-C019-CD01 (77.19 km, 185 min, utilización 97.5 %).
- **Verificación:** `verificacion_caso1.csv` sigue el formato oficial (VehicleId, DepotId, InitialLoad, RouteSequence, ClientsServed, DemandsSatisfied, TotalDistance, TotalTime, FuelCost).
- **Visualizaciones:** mapa Folium con rutas por color y figura `metricas_caso1.png` con utilización, distancia, clientes y costo de combustible por vehículo.