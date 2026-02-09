# 🛠️ Proyecto HIDROMASTER - Plataforma de Analytics

## 📝 Descripción del Proyecto
**Hidromaster** es una microempresa con 16 años de trayectoria dedicada a la distribución de material ferretero para construcción y reparación. 

Este proyecto implementa una solución de **ETL (Extract, Transform, Load)** en Databricks para centralizar datos provenientes de un sistema ERP. El objetivo principal es generar un tablero de control estratégico que permita identificar las causas del **margen bruto negativo** y optimizar la toma de decisiones financieras.

## 🏗️ Arquitectura de Datos (Medallion)
El flujo de datos sigue el patrón de Arquitectura Medallón para garantizar la calidad de la información:

- **Capa Bronze (Raw):** Ingesta de datos crudos desde Amazon S3 y archivos CSV locales.
- **Capa Silver (Cleaned):** Limpieza, tipado de datos y validación de reglas de negocio.
- **Capa Gold (Business):** Modelo dimensional (Esquema de Galaxia) optimizado para reporting.



## 📊 Modelo de Datos (Gold Layer)
El modelo se basa en un esquema de galaxia con dimensiones compartidas:

### Dimensiones:
- `dim_tiempo`: Maestro de fechas (2024-2026).
- `dim_productos`: Catálogo de materiales con costos y precios.
- `dim_clientes`: Registro de compradores y segmentación.
- `dim_proveedores`: Información de abastecimiento y formas de pago.

### Hechos:
- `fact_ventas`: Transacciones de salida y métricas de ingreso.
- `fact_compras`: Registros de entrada y costos de adquisición.

## 🚀 Guía de Ejecución
Para procesar los datos, ejecute los notebooks en el siguiente orden:

1. `01_Ingesta_Fuentes`: Carga inicial de S3 y DBFS.
2. `02_Limpieza_Validacion`: Procesamiento de Capa Silver.
3. `03_Creacion_Dimensiones`: Generación de SKs (Surrogate Keys) y dimensiones.
4. `04_Generacion_Hechos`: Construcción de tablas Fact con manejo de nulos (`coalesce`).

## 📈 Indicadores Clave (KPIs)
El análisis se centra en las siguientes métricas calculadas:

| Métrica | Fórmula de Cálculo | Descripción |
| :--- | :--- | :--- |
| **Ingresos Netos** | `SUM(fact_ventas[subtotal])` | Ventas totales sin incluir impuestos. |
| **Costo de Ventas** | `SUM(cantidad * costo_sin_iva)` | Costo de adquisición de los productos vendidos. |
| **Margen Bruto** | `Ingresos Netos - Costo de Ventas` | Rentabilidad monetaria de la operación. |
| **% Margen** | `(Margen Bruto / Ingresos Netos) * 100` | Rentabilidad porcentual por venta. |

## ⚙️ Requisitos
- **Databricks Runtime:** 11.0 o superior.
- **Cluster:** Configurado con acceso de lectura a buckets de AWS S3.
- **Lenguajes:** PySpark (Python) y Spark SQL.

---
*Desarrollado para la optimización de rentabilidad de Hidromaster.*
