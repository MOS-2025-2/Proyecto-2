# Caso 3 Quick Start Guide

## Overview
This guide provides quick instructions for running the Caso 3 implementation.

## Prerequisites
- Python 3.11+
- Required packages: `pyomo`, `pandas`, `numpy`, `folium`, `matplotlib`
- GLPK solver installed

## Installation
```bash
pip install pyomo pandas numpy folium matplotlib
# Install GLPK (OS-specific)
```

## Quick Execution

### Option 1: Run Everything (Recommended)
Open `Proyecto2.ipynb` and execute all cells until you reach the Caso 3 section, then run:

```python
datos3, modelo3, rutas3, depot_stats3, vehicle_stats3, global_stats3, sens3 = consola_caso3()
```

**Expected time**: 15-20 minutes  
**Outputs**: 
- `verificacion_caso3.csv`
- `mapa_caso3.html`
- `metricas_caso3.png`

### Option 2: Step-by-Step Execution

```python
# 1. Load data
datos = cargar_datos_caso3('data/Caso3')

# 2. Build and solve model
modelo = construir_modelo_caso3(datos)
resultado, obj_value, solve_time = resolver_modelo3(modelo, tiempo_limite=300)

# 3. Extract routes
rutas = extraer_solucion_caso3(modelo, datos)

# 4. Generate verification file
generar_verificacion_caso3(rutas, datos, 'verificacion_caso3.csv')

# 5. Generate statistics
depot_stats, vehicle_stats, global_stats = generar_estadisticas_detalladas_caso3(rutas, datos, obj_value)

# 6. Create visualizations
crear_mapa_caso3(datos, rutas, 'mapa_caso3.html')

# 7. Run sensitivity analysis (optional, takes ~15 min)
resultados_sens = analisis_sensibilidad_caso3(datos)

# 8. Create dashboard
crear_graficos_caso3(depot_stats, vehicle_stats, global_stats, resultados_sens, 'metricas_caso3.png')
```

## Data Files Location
All data files are in: `data/Caso3/`
- `clients.csv` - 60 clients with vehicle restrictions
- `vehicles.csv` - 15 vehicles (mixed fleet)
- `depots.csv` - 5 distribution centers
- `parameters_urban.csv` - Cost parameters

## Checking Results

### Verification File
```python
import pandas as pd
df = pd.read_csv('verificacion_caso3.csv')
print(df.head())
print(f"\nTotal routes: {len(df)}")
print(f"Total distance: {df['TotalDistance'].sum():.2f} km")
print(f"Total fuel cost: {df['FuelCost'].sum():,.0f} COP")
```

### Interactive Map
Open `mapa_caso3.html` in a web browser to see:
- All 60 client locations
- 5 depot locations
- Color-coded routes by depot
- Interactive popups with route details

### Analytics Dashboard
Open `metricas_caso3.png` to see:
1. Depot utilization percentages
2. Vehicles dispatched per depot
3. Total distance per depot
4. Load factor distribution
5. Distance by vehicle type
6. Cost breakdown pie chart
7. Sensitivity analysis impacts
8. Global statistics summary
9. Clients per route histogram

## Troubleshooting

### Solver Not Found
```python
# Check GLPK installation
from pyomo.environ import SolverFactory
solver = SolverFactory('glpk')
print(solver.available())  # Should be True
```

### Memory Issues
If you encounter memory issues:
```python
# Reduce time limit or use fewer sensitivity scenarios
modelo = construir_modelo_caso3(datos)
resultado, obj_value, solve_time = resolver_modelo3(modelo, tiempo_limite=180)  # 3 min
```

### Long Solve Times
- Expected: 2-5 minutes per scenario
- If taking longer: Check solver output for progress
- Consider using commercial solvers (Gurobi/CPLEX) for faster results

## Understanding Results

### Key Metrics to Look For

**Cost Efficiency:**
- Total cost < average industry benchmark
- Cost per kg delivered
- Fuel cost as % of total cost

**Operational Efficiency:**
- Fleet utilization > 60%
- Average load factor > 70%
- Depot utilization < 90% (留 buffer capacity)

**Route Quality:**
- Average distance per route
- Clients per route
- Load balance (low std dev)

## Sensitivity Analysis Interpretation

The sensitivity analysis shows cost impact of ±20% changes:

**High Impact (>5% cost change):**
- Client demand variations
- Fuel price changes

**Medium Impact (2-5% cost change):**
- Depot capacity constraints

**Low Impact (<2% cost change):**
- Other parameters

Use these insights for:
1. Risk management (hedge fuel prices)
2. Capacity planning (depot expansion priority)
3. Demand forecasting (impact on costs)

## Next Steps

After running Caso 3:
1. Review the business insights section in the notebook
2. Compare with Caso 2 results to see scaling effects
3. Consider the recommendations for LogistiCo
4. Document any optimizations or innovations for bonus eligibility

## Questions or Issues?

Refer to:
- `CASO3_IMPLEMENTATION_SUMMARY.md` - Detailed technical documentation
- `data/Caso3/README.md` - Original requirements
- Notebook cells 51-55 - Analysis and insights sections

## Performance Benchmarks

Expected performance on typical hardware:

| Component | Time | Memory |
|-----------|------|--------|
| Data loading | <1 sec | ~50 MB |
| Model construction | 5-10 sec | ~100 MB |
| Single solve | 2-5 min | ~500 MB |
| Sensitivity analysis (7 scenarios) | 15-20 min | ~500 MB |
| Visualization generation | 10-30 sec | ~200 MB |

**Total pipeline**: ~20-25 minutes
