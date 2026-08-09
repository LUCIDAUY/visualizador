# 🌃 LÚCIDA: Luz Urbana, Contaminación e Inteligencia de Datos Abiertos

**LÚCIDA** es una herramienta interactiva construida sobre Google Earth Engine (GEE) diseñada para analizar la evolución de la huella lumínica, urbana y demográfica en Uruguay, cruzando estos datos con áreas de importancia ambiental.

## Objetivo del Proyecto
El script permite auditar a nivel de píxel múltiples capas de información geoespacial. Su propósito principal es evaluar la contaminación lumínica en relación con el desarrollo urbano (huella edilicia) y monitorear su impacto cercano a zonas de conservación, como el Sistema Nacional de Áreas Protegidas (SNAP).

## Fuentes de Datos

El motor de LÚCIDA procesa e integra las siguientes colecciones y vectores:

* **Radiancia Nocturna:** Satélite Suomi NPP, sensor VIIRS (NOAA VCMCFG). Serie temporal 2017-2024.
* **Huella y Altura Edilicia:** Open Buildings Temporal V1 (Google Research).
* **Densidad Poblacional:** WorldPop Global Project (Resolución 100m).
* **Uso del Suelo:** Ministerio de Ganadería, Agricultura y Pesca (MGAP) / RENARE - Uruguay.
* **Rutas Nacionales:** Ministerio de Transporte y Obras Públicas (MTOP).
* **Áreas Protegidas:** Sistema Nacional de Áreas Protegidas (SNAP) - DINABISE, Ministerio de Ambiente.

## Funcionalidades Principales

1.  **Auditoría de Píxel (Inspector):** Al hacer clic en el mapa, el sistema extrae simultáneamente la radiancia exacta, la cantidad de observaciones sin nubes, el delta histórico, uso de suelo, distancia a rutas y si el punto intersecta con un área del SNAP.
2.  **Cálculo de Anomalía Lumínica:** Un índice propio que divide la Radiancia 2024 sobre la Huella Edilicia, permitiendo detectar zonas con alta emisión de luz pero nula infraestructura (ej. focos industriales, rutas muy iluminadas, o errores del sensor).
3.  **Renderizado Dinámico (SLD):** Leyendas interactivas que permiten encender y apagar rangos específicos de datos en tiempo real (ej. ver solo edificios de más de 15 metros, o solo luz de alta intensidad).
4.  **Buscador por Coordenadas:** Permite saltar directamente a un punto específico ingresando `Latitud, Longitud`.

## Metodología y Fórmulas Analíticas
El motor realiza procesamientos espaciales *on-the-fly* utilizando las siguientes definiciones matemáticas:

### 1. Delta Histórico (Evolución)
Calcula la variación de la intensidad lumínica entre el año más reciente y el año base.
$$\Delta L = L_{2024} - L_{2017}$$
Donde $L$ representa la radiancia máxima anual libre de nubes observada en un píxel ($nW \cdot cm^{-2} \cdot sr^{-1}$).

### 2. Índice de Anomalía Lumínica
Identifica zonas con emisión de luz desproporcionada respecto a su infraestructura física.
$$A = \frac{L_{2024}}{H + 1}$$
Donde $H$ es el porcentaje de Huella Edilicia (0-100). El factor $+1$ actúa como suavizado (*smoothing*) para evitar divisiones por cero en áreas naturales.

---
*Desarrollado en JavaScript utilizando la API de Google Earth Engine.*
