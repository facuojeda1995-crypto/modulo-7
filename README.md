Análisis de Datos Meteorológicos con DuckDB y SQL

Proyecto de análisis de series temporales climáticas utilizando **DuckDB** como motor analítico embebido en Python, 
consultando directamente archivos planos sin necesidad de servidores SQL tradicionales.

## Estructura del Proyecto
- `analisis_clima.py`:
- Script principal que gestiona la generación/lectura del dataset, la limpieza de datos y la ejecución de las consultas SQL.
- `requirements.txt`:
- Archivo con las dependencias necesarias (`duckdb` y `pandas`).
- `README.md`:
- Documentación del flujo de trabajo y resultados.

## Obtención del Dataset
El script es completamente autónomo: al ejecutarse, verifica si existe el archivo `clima_datos.csv` en la raíz del proyecto. 
Si no lo encuentra, genera automáticamente un dataset sintético de 10.000 registros meteorológicos (`estacion_id`, `fecha`, `temperatura`, `humedad`, `precipitacion`). También puede reemplazarse por cualquier CSV climático externo respetando esas columnas.

## Instalación y Ejecución
1. Instalar las dependencias:
```bash
pip install -r requirements.txt
```

2. Ejecutar el análisis:
```bash
python analisis_clima.py



## Etapas del Análisis
1. **Carga directa y vista virtual:** Se utiliza `read_csv_auto()` de DuckDB para leer el archivo CSV directamente desde el almacenamiento sin cargar la tabla completa en la memoria RAM de Pandas.
2. **Limpieza básica:** Se crea la vista virtual `clima_limpio`, filtrando registros con temperaturas nulas (`WHERE temperatura IS NOT NULL`) y asegurando los tipos de datos (`DATE` y `FLOAT`).
3. **Consultas analíticas:**
   - **Promedios mensuales:** Agrupación temporal por año y mes para obtener la temperatura y humedad media.
   - **Máximos históricos:** Cálculo de los valores pico de temperatura y precipitación registrados por cada estación.
   - **Filtrado complejo:** Detección de eventos extremos donde la temperatura supera los 35°C o la precipitación supera los 50mm.

## Hallazgos y Rendimiento
- **Velocidad y memoria:** DuckDB ejecuta las consultas en milisegundos gracias a su procesamiento vectorizado, evitando el consumo excesivo de memoria RAM (`MemoryError`) típico de las cargas completas en memoria.
- **Portabilidad:** Al no generar archivos binarios de base de datos (`.db`), el repositorio se mantiene liviano y fácil de replicar.
