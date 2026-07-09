# Actividad No. 1
## Laboratorio GIT aplicado a una aplicación Streamlit para análisis de datos

# Grupo 4
## 🚀 Integrantes
| Nro. | Nombre | Link |
|------|---------|---------|
| 1 | Giovanni Xavier Baño Jaya | https://github.com/Giovanni26101982/Grupo4_Docker_TareaFinal |
| 2 | Jara Pauta Cesar Paúl | https://github.com/PaulJara84/Grupo4_Docker_TareaFinal |

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


## 🖥️ Descarga del repositorio

   - Descargar el repositorio
     ```bash
     git clone https://github.com/Giovanni26101982/Grupo4_Docker_TareaFinal.git
     ```
<img width="1125" height="305" alt="image" src="https://github.com/user-attachments/assets/3b02a6b7-49a1-42c2-a1af-cdb5f3eb8703" />

   - Navegar a la carpeta descargada 
     ```bash
     cd Grupo4_Docker_TareaFinal/
     ```
<img width="1123" height="64" alt="tarea final" src="https://github.com/user-attachments/assets/96636ef1-0f24-421d-a876-0c1f684a148c" />


---

## 🚀 Requisitos previos
- Docker instalado
- Docker Compose instalado

---

## 📂 Estructura
```bash
flowise-postgres/
│── docker-compose.yml
│── .env
└── README.md
```

---


## ✅ Conclusiones

1. **Compatibilidad de versión de Compose**

   -	La instrucción name: solo es reconocida en la especificación moderna de Compose (ejecutando docker compose en lugar de docker-compose).
     <img width="886" height="117" alt="image" src="https://github.com/user-attachments/assets/9d6de6da-b703-4fc5-a47b-2afc23410a30" />

   -	Se detectó que version: ya es obsoleto en Compose V2, por lo que puede omitirse para evitar advertencias.
     <img width="711" height="441" alt="image" src="https://github.com/user-attachments/assets/a68e5832-6cc0-4540-a947-db94d9bbd159" />

2. **Gestión de espacio en disco**

   - Durante la descarga de la imagen flowiseai/flowise:1.6.3 se produjo un error de “no space left on device”.
   - Se resolvió mediante limpieza de imágenes, volúmenes y contenedores no utilizados, confirmando la importancia de tener espacio disponible antes de la instalación.

3.	**Ejecución del contenedor Flowise**
   
      - El contenedor entraba en bucle de reinicios porque la imagen oficial requiere ejecutar explícitamente el comando flowise start.
      - Al añadir command: ["flowise", "start"] en el docker-compose.yml, el servicio se inicializó correctamente y quedó accesible en el puerto configurado.
     
4.	**Base de datos y persistencia**

      - El contenedor PostgreSQL respondió como healthy, lo que confirma que la configuración de credenciales (POSTGRES_USER, POSTGRES_PASSWORD, POSTGRES_DB) fue 
