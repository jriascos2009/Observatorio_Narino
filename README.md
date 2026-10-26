# 🇨🇴 Observatorio de Actividad Económica y Social por Departamentos

Aplicación web interactiva construida con **Gradio** que permite explorar, filtrar y visualizar indicadores oficiales de los departamentos de Colombia.

Fuentes de datos reales (datos.gov.co / SODA API):

| Indicador | Fuente | Endpoint |
|-----------|--------|----------|
| PIB (miles de millones, constantes 2015) | DANE / datos.gov.co | `kgyi-qc7j` |
| Homicidios (víctimas) | MinDefensa | `m8fd-ahd9` |
| Recursos públicos DNP (COP) | Departamento Nacional de Planeación | `wc86-9j4a` |

> Los datos se cargan en tiempo real desde las APIs oficiales. No se inventan ni se almacenan localmente.

---

## Características

- **Filtros** por departamento(s), indicador y rango de años
- **Estadísticas descriptivas** (media, mediana, desviación, min, max, n)
- **Gráfico de tendencias** (líneas por departamento)
- **Barras comparativas** (Top 15 departamentos por año)
- **Tabla de datos** filtrados
- Normalización robusta de nombres de departamentos (sin tildes, mayúsculas, variantes de Bogotá, Valle, San Andrés, etc.)

---

## Requisitos

- Python 3.9+
- Conexión a internet (para descargar los datos de datos.gov.co)

---

## Instalación

```bash
# Clonar o copiar el proyecto
cd observatorio-departamental

# Crear entorno virtual (recomendado)
python -m venv venv
source venv/bin/activate          # Linux / macOS
# venv\Scripts\activate           # Windows

# Instalar dependencias
pip install -r requirements.txt
```

---

## Uso

```bash
python observatorio_nariñoipynb.py
```

La aplicación se abrirá automáticamente en el navegador (o mostrará la URL local y el enlace público de Gradio si `share=True`).

### Controles de la interfaz

1. **Departamento(s)**: selección múltiple (o “Todos”)
2. **Indicador**: PIB, Homicidios o Recursos DNP
3. **Año inicial / Año final**: rango temporal
4. **Año para barras**: año específico del ranking Top 15
5. Botón **Actualizar análisis**

---

## Estructura del código

```
observatorio_nariñoipynb.py
├── Normalización de departamentos (quitar_tildes + diccionario canónico)
├── Carga de datos
│   ├── cargar_pib()
│   ├── cargar_homicidios()
│   └── cargar_dnp()
├── construir_maestro()          → DataFrame unificado
├── Funciones de análisis
│   ├── filtrar()
│   ├── stats()
│   ├── tendencia()
│   └── barras()
└── Interfaz Gradio (Blocks)
```

---

## Notas importantes

- La primera carga puede tardar 30–90 segundos porque se descargan hasta ~150 000 registros.
- Si alguna fuente falla, la aplicación continúa con las que sí respondieron.
- Los valores del PIB están en **miles de millones de pesos constantes 2015**.
- Los homicidios se agregan por año-departamento sumando la columna `cantidad` (o 1 por registro si no existe).

---

## Licencia y atribución

Datos públicos de:

- [datos.gov.co](https://www.datos.gov.co)
- Ministerio de Defensa Nacional
- Departamento Nacional de Planeación (DNP)

Código liberado para uso académico y de análisis público.
