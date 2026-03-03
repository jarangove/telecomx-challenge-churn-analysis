# telecomx-challenge-churn-analysis — Análisis de Evasión de Clientes (Churn)

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completado-2ecc71?style=for-the-badge)

# Descripción del Proyecto
**Telecom X** es una empresa de telecomunicaciones que enfrenta una alta tasa de cancelaciones de clientes. Como parte del equipo de análisis de datos, el objetivo de este proyecto es identificar los factores que generan la evasión de clientes mediante un proceso completo de **ETL + Análisis Exploratorio de Datos (EDA)**, entregando un dataset limpio y listo para que el equipo de Data Science construya modelos predictivos.

## Objetivos

- Extraer datos desde una API en formato JSON
- Aplicar un pipeline ETL completo (limpieza, transformación y enriquecimiento)
- Realizar un Análisis Exploratorio de Datos (EDA) con visualizaciones estratégicas
- Identificar patrones y factores asociados al churn
- Generar un informe con insights y recomendaciones accionables

---

## Estructura del Repositorio

```
telecomx-challenge-churn-analysis/
│
├── TelecomX_LATAM_Completo.ipynb   # Notebook principal (ETL + EDA + Informe)
├── TelecomX_Data.json               # Dataset original (fuente API)
├── TelecomX_diccionario.md          # Diccionario de datos
├── TelecomX_clean.csv               # Dataset limpio exportado
└── README.md                        # Este archivo
```

---

## Dataset

| Atributo | Detalle |
|----------|---------|
| Fuente | API REST — JSON |
| Registros originales | 7,267 |
| Registros válidos | 7,043 |
| Variables | 21 originales → 24 tras feature engineering |
| Variable objetivo | `churn` (0 = No canceló / 1 = Canceló) |
| Tasa de churn | ~26.5% |

### Diccionario de Variables Principales

| Variable | Descripción |
|----------|-------------|
| `customer_id` | ID único del cliente |
| `churn` | Si el cliente canceló (1) o no (0) |
| `tenure` | Meses de contrato activo |
| `contract` | Tipo de contrato (mensual, 1 año, 2 años) |
| `monthly_charges` | Cargo mensual total en USD |
| `total_charges` | Total acumulado pagado en USD |
| `internet_service` | Tipo de servicio de internet |
| `payment_method` | Método de pago |
| `total_services` | *(Feature)* Total de servicios contratados |
| `tenure_group` | *(Feature)* Segmento de antigüedad |


## Pipeline ETL

```
JSON (API)
    │
    ▼
pd.json_normalize()       ← Aplanar estructura anidada
    │
    ▼
Renombrado de columnas    ← snake_case estandarizado
    │
    ▼
Corrección de tipos       ← total_charges: str → float
    │
    ▼
Tratamiento de nulos      ← Eliminar churn vacío (224 filas)
    │
    ▼
Estandarización           ← Yes/No → 1/0
    │
    ▼
Feature Engineering       ← total_services, tenure_group, charge_per_service
    │
    ▼
df_clean (7,043 × 24)    ← Listo para modelado
```

---

## Principales Hallazgos

### 1 Tipo de Contrato — Factor más crítico
Los clientes con **contrato mensual** presentan una tasa de churn superior al **42%**, frente al 3% de contratos de 2 años. El compromiso a largo plazo es el predictor más fuerte de retención.

### 2 Primeros 12 meses — Ventana crítica
La mediana de tenure de quienes cancelan es de ~10 meses vs ~38 meses de quienes permanecen. Los clientes nuevos son los más vulnerables.

### 3 Fibra Óptica — Alto riesgo
Clientes con **Fiber Optic** tienen tasas de churn cercanas al **42%**, más del doble que los clientes DSL (~19%). Posible problema de calidad/precio.

### 4 Electronic Check — Señal de alerta
El pago por cheque electrónico presenta la mayor tasa de churn (~45%). Los métodos automáticos (débito/tarjeta) muestran tasas menores al 18%.

### 5 Servicios adicionales — Efecto protector
A mayor cantidad de servicios contratados, **menor es la tasa de churn**. El bundling genera mayor fidelización.


---

## Recomendaciones Estratégicas

| # | Estrategia | Impacto |
|---|-----------|---------|
| 1 | Incentivar migración de contratos mensuales a anuales | 🔴 Alto |
| 2 | Programa de onboarding para los primeros 3-6 meses | 🔴 Alto |
| 3 | Revisar calidad y precio del servicio Fiber Optic | 🔴 Alto |
| 4 | Promover pagos automáticos (débito/tarjeta) | 🟡 Medio |
| 5 | Crear bundles de servicios con precios especiales | 🟡 Medio |
| 6 | Atención especializada para adultos mayores (+65) | 🟡 Medio |

---

## 🚀 Cómo Ejecutar el Proyecto

### Opción 1 — Google Colab (Recomendado)

1. Abre el archivo `TelecomX_LATAM_Completo.ipynb` en Google Colab
2. Ejecuta todas las celdas con `Runtime → Run all`
3. El notebook descargará los datos automáticamente desde la API

### Opción 2 — Local

```bash
# Clonar el repositorio
git clone https://github.com/jarangove/telecomx-challenge-churn-analysis.git
cd telecomx-challenge-churn-analysis

# Instalar dependencias
pip install pandas numpy matplotlib seaborn requests

# Ejecutar el notebook
jupyter notebook TelecomX_LATAM_Completo.ipynb
```

---

## Tecnologías Utilizadas

| Herramienta | Uso |
|-------------|-----|
| Python 3.10+ | Lenguaje principal |
| Pandas | Manipulación y limpieza de datos |
| NumPy | Operaciones numéricas |
| Matplotlib | Visualizaciones |
| Seaborn | Visualizaciones estadísticas |
| Requests | Consumo de la API |
| Google Colab | Entorno de ejecución |

---

## Próximos Pasos

El dataset limpio `TelecomX_clean.csv` está listo para la fase de modelado predictivo. Se recomienda explorar:

- **Logistic Regression** — baseline interpretable
- **Random Forest / XGBoost** — mayor performance
- **Variables clave:** `contract`, `tenure`, `internet_service`, `payment_method`, `monthly_charges`, `total_services`

---

## Autor

**jarangove**  
GitHub: [@jarangove](https://github.com/jarangove)

---

## Licencia

Este proyecto fue desarrollado como parte del **Challenge de Data Science LATAM**.

















