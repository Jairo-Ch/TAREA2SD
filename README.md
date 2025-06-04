# Tarea2SD: Procesamiento Distribuido de Eventos de Tránsito

Proyecto de Sistemas Distribuidos - 2025-1

---

## Descripción

En esta segunda entrega se aborda el procesamiento y análisis de eventos de tránsito extraídos desde Waze, utilizando herramientas distribuidas como **Apache Pig** para manipulación de datos y **Python** para visualización gráfica. 

El objetivo fue transformar los datos crudos recolectados en la entrega anterior, aplicando filtros, agrupaciones y conversiones, y generar visualizaciones que permitan comprender el comportamiento del tránsito en la Región Metropolitana.

---

## Estructura del Proyecto


```
Tarea1SD/
|-- README.md
|-- docker-compose.yml
|-- event_collector
|   |-- accidentes_por_tipo
|   |-- csv
|   |-- eventos.json
|   |-- eventos_accidente_por_fecha
|   |-- eventos_limpios
|   |-- eventos_por_fecha
|   |-- eventos_por_tipo
|   |-- eventos_por_tipo_y_comuna
|   |-- img
|   |-- resultados_agrupados
|   |-- scriptspig
|   |-- scriptspy
|   |-- top5_ciudades_accidente
|   `-- total_accidentes
|-- pig_env
|   `-- Dockerfile
|-- storage
|   |-- cache_manager.py
|   |-- carga_a_redis.py
|   |-- crear_tabla.py
|   |-- insertar_eventos.py
|   |-- probar_cache.py
|   `-- probar_redis.py
|-- tests
|   `-- pruebas_rendimiento.py
|-- traffic_api
|   |-- Dockerfile
|   |-- __pycache__
|   |-- main.py
|   `-- requirements.txt
`-- traffic_generator
    |-- generador_poisson.py
    `-- generador_uniforme.py
```





---

## Tecnologías Utilizadas

- **Python 3.11**
- **Apache Pig 0.17**
- **Hadoop 3.3.6**
- **Docker / Docker Compose**
- **Pandas, Matplotlib, Seaborn** (para visualización)

---

## ⚙️ Instalación y Ejecución

1. Clona el repositorio:
   ```bash
    git clone https://github.com/Jairo-Ch/Tarea2SD.git
   cd Tarea2SD
   ```

2. Crea y activa un entorno virtual para evitar conflictos de dependencias:

   ```bash
  python3 -m venv venv
  source venv/bin/activate
  pip install -r requirements.txt
   ```
3. Levanta el entorno distribuido con Docker (incluye Apache Pig y Hadoop):
  ```bash
    docker compose up --build
  ```


Ejecución de Scripts

1. Recolectar y transformar datos
  
  ```bash
  python3 event_collector/scriptspy/json_a_csv.py
  ```

2. Ejecutar scripts de Apache Pig dentro del contenedor
  
  ```bash
  docker exec -it pig_worker bash

  pig -x mapreduce event_collector/scriptspig/limpiar_eventos.pig
  pig -x mapreduce event_collector/scriptspig/eventos_por_tipo.pig
  pig -x mapreduce event_collector/scriptspig/eventos_por_fecha.pig  
  pig -x mapreduce event_collector/scriptspig/accidentes_por_tipo.pig
  pig -x mapreduce event_collector/scriptspig/contar_accidentes.pig
  pig -x mapreduce event_collector/scriptspig/eventos_accidente_por_fecha.pig
  pig -x mapreduce event_collector/scriptspig/eventos_por_tipo_y_comuna.pig
  pig -x mapreduce event_collector/scriptspig/procesar_eventos.pig
  pig -x mapreduce event_collector/scriptspig/top5_ciudades_accidente.pig
  ```

3. Visualizar resultados con python

```bash
  source venv/bin/activate
  python3 graficador/graficar_eventos.py
  python3 graficador/heatmap_eventos.py
  python3 graficador/mapa_eventos.py

```
## Resultados Generados

A continuación, se listan los archivos producidos durante la ejecución del proyecto, tanto en formato CSV como en gráficos visuales:

### Archivos CSV

- `eventos_por_tipo.csv`  
- `eventos_por_tipo_y_comuna.csv`  
- `eventos_por_fecha`  
- `top5_ciudades_accidente`

nota: en caso de no estar el .csv es puede visualizar desde la carpeta con su nombre en el apartado de "part-r-00000" o "part-m-0000"

### Gráficos

- `eventos_por_tipo.png`  
- `mapa_eventos_tipo_calle.png`  
- `top_calles_eventos.png`

Notas Finales

Esta entrega demuestra el uso de herramientas de procesamiento distribuido como Apache Pig y la integración con Python para análisis visual. El proyecto se diseñó en contenedores para asegurar portabilidad y reproducibilidad del entorno de análisis.

