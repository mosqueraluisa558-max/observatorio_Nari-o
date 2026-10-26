# 🇨🇴 Observatorio de Actividad Económica y Social por Departamentos

**Fuente del código original:** Google Colab  
**Archivo base:** `observatorio_nariñoipynb.py`  
**Tecnologías:** Gradio · Plotly · Pandas · Requests · datos.gov.co (SODA)

---

## Descripción

Aplicación interactiva que consolida y visualiza **datos oficiales** de los departamentos de Colombia en tres indicadores principales:

| Indicador | Descripción | Fuente |
|-----------|-------------|--------|
| **PIB** | Producto Interno Bruto (miles de millones, precios constantes 2015) | datos.gov.co / `kgyi-qc7j` |
| **Homicidios** | Número de víctimas por departamento y año | MinDefensa / `m8fd-ahd9` |
| **Recursos públicos DNP** | Valor pagado/obligado/comprometido (COP) | DNP / `wc86-9j4a` |

Los datos se descargan en tiempo real desde las APIs de **datos.gov.co**, se normalizan y se agregan por departamento y año.

---

## Características principales

- **Normalización robusta de nombres de departamentos** (elimina tildes, unifica variantes de Bogotá, San Andrés, Valle del Cauca, etc.).
- Filtros interactivos:
  - Selección múltiple de departamentos
  - Selección de indicador
  - Rango de años (slider)
  - Año específico para el gráfico de barras
- Visualizaciones:
  - **Tendencias** → gráfico de líneas (Plotly)
  - **Barras comparativas** → Top 15 departamentos
  - **Tabla de datos** → hasta 400 registros filtrados
- Estadísticas descriptivas (media, mediana, desviación estándar, mínimo, máximo y conteo).

---

## Estructura del código

### 1. Instalación e importaciones
```python
!pip install -q gradio plotly pandas requests
```
Se importan: `gradio`, `pandas`, `numpy`, `plotly`, `requests`, `unicodedata`.

### 2. Normalización de texto
- Función `quitar_tildes()`: convierte a minúsculas y elimina acentos.
- Diccionario `DEPARTAMENTOS_CANONICOS`: mapea todas las variantes a nombres oficiales.
- Función `normalizar_departamento()`.

### 3. Carga de datos (funciones)
| Función | Endpoint | Límite | Agregación |
|---------|----------|--------|------------|
| `cargar_pib()` | `kgyi-qc7j` | 50.000 | Suma de todas las actividades (precios constantes) |
| `cargar_homicidios()` | `m8fd-ahd9` | 150.000 | Suma de víctimas por depto-año |
| `cargar_dnp()` | `wc86-9j4a` | 80.000 | Suma de valor pagado/obligado/comprometido |

### 4. Construcción del DataFrame maestro
```python
DF = construir_maestro()
```
Concatena las tres fuentes, convierte el año a entero y ordena por indicador → departamento → año.

### 5. Funciones de análisis
- `filtrar()` → aplica filtros de departamento, indicador y rango de años.
- `stats()` → genera tabla de estadísticas en Markdown.
- `tendencia()` → gráfico de líneas.
- `barras()` → gráfico de barras horizontales (Top 15).

### 6. Interfaz Gradio
- Tema `Soft` (azul).
- Controles: Dropdowns, Sliders y botón “Actualizar análisis”.
- Pestañas: Tendencias · Barras comparativas · Datos.
- Se ejecuta automáticamente al cargar (`demo.load`) y al hacer clic en el botón.

---

## Cómo ejecutar

1. Abrir en **Google Colab** o entorno local con Python 3.8+.
2. Ejecutar todas las celdas (o el archivo completo).
3. La aplicación se lanza con `demo.launch(share=True)` y genera un enlace público temporal.

**Requisitos:**
```bash
pip install gradio plotly pandas requests
```

---

## Notas importantes

- Los datos se obtienen **en vivo** desde datos.gov.co. Si alguna fuente falla, el resto sigue funcionando.
- Se usa `warnings.filterwarnings("ignore")` para silenciar avisos de pandas/plotly.
- El DataFrame maestro se construye una sola vez al inicio. Si se desea actualizar los datos, hay que reiniciar el runtime.
- El nombre “Nariño” en el archivo original es solo identificativo; la aplicación cubre **todos** los departamentos de Colombia.

---

## Autor / Origen
Código generado automáticamente desde un notebook de Google Colab.  
Adaptado y documentado para uso local o compartido.

---

*Última actualización de la documentación: octubre 2026*
