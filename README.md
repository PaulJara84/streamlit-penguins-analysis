# Actividad No. 1
## Laboratorio GIT aplicado a una aplicación Streamlit para análisis de datos

# Grupo 4
## 🚀 Integrantes
| Nro. | Nombre | Link |
|------|---------|---------|
| 1 | Giovanni Xavier Baño Jaya | https://github.com/Giovanni26101982/streamlit-penguins-analysis |
| 2 | Jara Pauta Cesar Paúl | https://github.com/PaulJara84/streamlit-penguins-analysis |

---

## 📖 1. Configuración Inicial y GitFlow

El primer paso es establecer la estructura de ramas adecuada. GitFlow utiliza ramas específicas para desarrollo y características, asegurando que la rama **main** (o **master**) contenga solo código estable.

1.1. **Inicialización del Repositorio**

Crea un directorio para tu proyecto e inicializa Git. Es crucial crear la rama develop desde el inicio, ya que todo el desarrollo ocurrirá allí. 


```bash
# Crear carpeta y entrar
mkdir streamlit-penguins-analysis
cd streamlit-penguins-analysis

# Inicializar git
git init

# Crear archivo README.md inicial y hacer el primer commit en main
echo "# Análisis de Datos Penguins con Streamlit" > README.md
git add README.md
git commit -m "Initial commit"

# Crear rama develop y cambiar a ella
git branch develop
git checkout develop   
```
Paso 1: 

<img width="670" height="633" alt="image" src="https://github.com/user-attachments/assets/1ff589a0-48c7-4538-9c8e-eb7363d25a03" />

Paso 2:

<img width="669" height="254" alt="image" src="https://github.com/user-attachments/assets/f306a898-b1d0-49be-b14e-2f1ddad55d44" />

Paso 3: 

<img width="660" height="165" alt="image" src="https://github.com/user-attachments/assets/8d06a804-54c3-4c14-b860-06bdea3bee64" />


---

1.2. **Estrategia de Ramas para las Actividades**

Para cumplir con los requisitos sin generar conflictos ni usar hotfixes, se crearán ramas de característica (**feature/**) individuales desde **develop** para cada tarea solicitada:

1. **feature/validacion-datos**: Para las funciones de validación.
2. **feature/app-principal**: Para la interfaz base de Streamlit.
3. **feature/viz-tabular**: Para la tabla de datos interactiva.
4. **feature/pruebas-pytest**: Para los tests unitarios. 

---

## 2. Desarrollo de Funcionalidades

A continuación se detalla el código para cada rama de característica. Asegúrate de hacer **git checkout develop** y luego **git checkout -b feature/nombre** antes de empezar cada bloque. 

2.1 **Funciones de Validación (feature/validacion-datos)**

Crea un archivo **utils/validation.py**. Estas funciones aseguran la integridad del dataset **penguins.csv** antes de procesarlo.

```bash
# utils/validation.py
import pandas as pd

def validate_dataframe(df: pd.DataFrame) -> bool:
    """Valida que el dataframe no esté vacío y tenga las columnas esperadas."""
    required_columns = ['species', 'island', 'bill_length_mm', 'bill_depth_mm', 'flipper_length_mm', 'body_mass_g', 'sex']
    if df.empty:
        return False
    return all(col in df.columns for col in required_columns)

def validate_no_nulls(df: pd.DataFrame, columns: list) -> int:
    """Retorna la cantidad de nulos en las columnas especificadas."""
    return df[columns].isnull().sum().sum()   
```

2.2. **Aplicación Principal y Visualización Tabular**

Puedes combinar la app principal y la visualización en un solo archivo **app.py** o separarlos. Aquí se presenta una estructura modular en **app.py** que importa las validaciones y muestra los datos. 

```bash
     # app.py
      import streamlit as st
      import pandas as pd
      from utils.validation import validate_dataframe, validate_no_nulls
      
      # Configuración de página
      st.set_page_config(page_title="Análisis Penguins", layout="wide")
      
      st.title("🐧 Análisis de Datos: Palmer Penguins")
      
      # Carga de datos
      @st.cache_data
      def load_data():
          try:
              df = pd.read_csv("penguins.csv")
              return df
          except FileNotFoundError:
              st.error("No se encontró el archivo penguins.csv")
              return None
      
      df = load_data()
      
      if df is not None:
          # Validación
          if validate_dataframe(df):
              st.success("✅ Datos validados correctamente.")
              
              # KPIs básicos
              col1, col2 = st.columns(2)
              with col1:
                  st.metric("Total Registros", len(df))
              with col2:
                  nulos = validate_no_nulls(df, df.columns)
                  st.metric("Valores Nulos Detectados", nulos)
      
              # Visualización Tabular
              st.subheader("📊 Visualización Tabular de Datos")
              st.dataframe(df.style.highlight_max(axis=0), use_container_width=True)
              
              # Gráfica de ejemplo (Distribución de masa corporal)
              st.subheader("Distribución de Masa Corporal por Especie")
              chart_data = df.groupby('species')['body_mass_g'].mean()
              st.bar_chart(chart_data)
          else:
              st.error("❌ El archivo no tiene el formato esperado.")
      else:
          st.warning("Esperando carga de datos...")   
          
```
---

## 3. Pruebas con Pytest (feature/pruebas-pytest)

Crea un directorio **tests** y un archivo **test_validation.py**. Esto asegura que las funciones de validación funcionen antes de hacer el release.

 ```bash
# tests/test_validation.py
import pandas as pd
from utils.validation import validate_dataframe, validate_no_nulls

def test_validate_dataframe_success():
    data = {'species': ['Adelie'], 'island': ['Torgersen'], 'bill_length_mm': [39.1]}
    df = pd.DataFrame(data)
    # Nota: En un caso real, el DF debe tener todas las columnas requeridas
    # Aquí simulamos un DF mínimo para el ejemplo lógico
    assert validate_dataframe(df) == True # Ajustar lógica según columnas reales

def test_validate_no_nulls():
    data = {'col1': [1, None], 'col2': [5, 6]}
    df = pd.DataFrame(data)
    assert validate_no_nulls(df, ['col1']) == 1   

 ```

Ejecuta las pruebas localmente para verificar:

```bash
     pytest tests/ -v
```

<img width="664" height="216" alt="image" src="https://github.com/user-attachments/assets/07b89dc8-a279-4d46-8870-bebb857c2925" />


---

## 4. Lanzamiento y Documentación (Release)

Una vez completadas todas las funcionalidades, fusiona las ramas a **develop** y luego a **main** para crear el release.

4.1. **Fusión de Ramas**

```bash
# Desde cada rama feature, después de commitear:
git checkout develop
git merge feature/validacion-datos --no-ff
git merge feature/app-principal --no-ff
git merge feature/viz-tabular --no-ff
git merge feature/pruebas-pytest --no-ff

# Verificar que todo funcione en develop antes de pasar a main
```
<img width="664" height="216" alt="image" src="https://github.com/user-attachments/assets/db7e508b-7d40-417c-a046-b4bbc6cddfea" />


4.2. **Creación del Release y Tags**

El release marca una versión estable. Se crea desde la rama **main**.

```bash
git checkout main
git merge develop --no-ff

# Crear tag de versión
git tag -a v1.0.0 -m "Release inicial: App Streamlit con validación y tests"

# Subir todo a GitHub (asumiendo remoto configurado como 'origin')
git push origin main develop --tags   
```
PAso 1: Merge

<img width="663" height="238" alt="image" src="https://github.com/user-attachments/assets/dad280f7-32fd-4712-ad60-6130c4e66b4a" />

Paso 2: Versionamiento - Para nuestro caso tomamos la codificación: v101.1.0 inicial

<img width="666" height="109" alt="image" src="https://github.com/user-attachments/assets/53d7e646-d3da-450f-b994-112d981f4f14" />


---

## Documentación del Repositorio

El repositorio debe incluir dos archivos Markdown clave:

1. **README.md** : Describe el propósito.
   
- Contenido sugerido: "Este proyecto es una aplicación de análisis de datos construida con Streamlit para explorar el dataset Palmer Penguins. Incluye validación de datos automatizada, visualización interactiva de tablas y gráficos, y un suite de pruebas unitarias con pytest."
  
2. **INFORME_LABORATORIO.md**: Detalla el procedimiento.

- Contenido requerido:

    - Explicación del flujo GitFlow utilizado (ramas creadas).
    - Capturas de pantalla (gráficas) de la ejecución del dashboard principal.
    - Captura de la tabla de datos interactiva.
    - Captura de la terminal mostrando la ejecución exitosa de pytest (con los checks en verde).

---

## Checklist Final de Entrega

Antes de considerar la tarea finalizada, verifica en tu repositorio remoto de GitHub:

- [ ] Existencia de la rama main y develop.
- [ ] Presencia del tag v101.1.0 (o similar) en la sección de "Releases" o "Tags".
- [ ] Archivo penguins.csv cargado en el repositorio.
- [ ] Archivos README.md e INFORME_LABORATORIO.md visibles y con contenido.
- [ ] Historial de commits que refleje el uso de ramas feature (ej. "Merge branch 'feature/validacion-datos'").

## 📂 Estructura del Proyecto
El repositorio se organiza de la siguiente manera para separar la lógica, las pruebas y la interfaz:

```text
streamlit-penguins-analysis/
├── .git/                  # Repositorio Git (ramas main, develop, features)
├── utils/                 # Lógica de negocio
│   └── validation.py      # Funciones de validación de datos
├── tests/                 # Pruebas unitarias
│   └── test_validation.py # Tests para validación
├── data/                  # Datos
│   └── penguins.csv       # Dataset de pingüinos
├── app.py                 # Punto de entrada de la aplicación Streamlit
├── requirements.txt       # Dependencias (streamlit, pandas, pytest)
├── README.md              # Descripción del proyecto
└── INFORME_LABORATORIO.md # Informe
```
---

## ✅ Conclusiones

1. **Estandarización del Flujo de Trabajo con GitFlow**: La implementación de ramas feature aisladas demostró ser una estrategia efectiva para desarrollar funcionalidades complejas (validación, visualización, pruebas) de manera ordenada, eliminando la necesidad de correcciones de emergencia (hotfix) y garantizando un historial de versiones limpio y trazable.

2. **Importancia de la Validación Temprana de Datos**: La integración de funciones de validación (utils/validation.py) antes de la visualización confirmó que asegurar la integridad del dataset (verificación de columnas y nulos) es crítico para evitar errores en tiempo de ejecución en aplicaciones de datos, mejorando la robustez del dashboard frente a inconsistencias en el archivo penguins.csv.
   
3. **Calidad del Código mediante Pruebas Automatizadas**: El uso de pytest permitió verificar la lógica de negocio de forma unitaria antes del despliegue, demostrando que la automatización de pruebas es esencial para detectar errores lógicos en etapas tempranas del desarrollo, facilitando un release confidente y estable (v101.1.4 ultimo versión desarrollada).

