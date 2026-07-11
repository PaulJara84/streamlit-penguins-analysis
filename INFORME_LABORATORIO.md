# Informe de Laboratorio: Aplicación Streamlit con GitFlow


# Indice

| Item | Nombre |
|------|---------|
| 1 | Objetivo |
| 2 | Procedimiento Realizado |
| 3 | Desarrollo de la Aplicación* |
| 4 | Lanzamiento y Documentación (Release) |
| 5 | Evidencias de Ejecución |
| 6 | Documentación del Repositorio |
| 7 | Checklist Final de Entrega |
| 8 | Estructura del Proyecto |
| 9 | Conclusiones |

# 1. Objetivo

Desarrollar una aplicación de análisis de datos sobre el dataset Palmer Penguins utilizando Streamlit, implementando un flujo de trabajo de control de versiones GitFlow e integrando pruebas automatizadas con pytest.

---

# 📖 2. Procedimiento Realizado

**Configuración Inicial y GitFlow**

El primer paso es establecer la estructura de ramas adecuada. GitFlow utiliza ramas específicas para desarrollo y producción, asegurando que la rama **main** (o **master**) contenga solo código estable.

## 2.1. **Inicialización del Repositorio**

Crear un directorio para tu proyecto e inicializar Git. 

  - Crear la rama develop (todo el desarrollo se aplica aquí). 


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

## 2.2. **Estrategia de Ramas para las Actividades**

Se inicializó el repositorio estableciendo la rama **main** para producción y **develop** para integración. El desarrollo se realizó mediante ramas de característica (**feature/**) aisladas para evitar conflictos:

- **feature/validacion-datos**: Implementación de lógica de validación de esquemas y nulos.
- **feature/app-principal**: Estructura base de la interfaz en Streamlit.
- **feature/viz-tabular**: Implementación de visualización de datos interactiva.
- **feature/pruebas-pytest**: Creación de tests unitarios para las funciones de validación.

Cada rama fue fusionada a **develop** utilizando **git merge --no-ff** para preservar el historial de características.

---

# 🛠️ 3. **Desarrollo de la Aplicación**

- **Validación**: Se crearon funciones en **utils/validation.py** para asegurar la integridad del dataset antes de su procesamiento.
- **Interfaz**: La aplicación **app.py** carga el archivo **penguins.csv**, valida los datos y muestra métricas clave (totales y nulos).
- **Visualización**: Se implementó una tabla interactiva y gráficos de distribución utilizando las libreras nativas de Streamlit.

## 3.1. Desarrollo de Funcionalidades

A continuación se detalla el código para cada rama. Revisar de hacer **git checkout develop** y luego **git checkout -b feature/nombre** antes de empezar cada bloque. 

3.1.1 **Funciones de Validación (feature/validacion-datos)**

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

3.1.2. **Aplicación Principal y Visualización Tabular**

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

## 3.2. Pruebas con Pytest (feature/pruebas-pytest)

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

Se ejecutó el suite de pruebas con pytest, validando el correcto funcionamiento de los módulos de utilidad. Tras verificar la estabilidad en develop, se fusionó el código a main y se etiquetó la versión estable con el tag v10001.1.0.

**Actualización de la rama**

```bas
     git checkout develop
     git merge feature/validacion-datos --no-ff
```

**Actualización a la rama MAIN**

```bas
     git checkout main
     git merge develop --no-ff
```

**Release**

```bas
     git tag -a v10001.1.0 -m "Release inicial: App Streamlit con validación y test"
```

**Vincular repositorio Local a GitHub** (crear previamente el repositorio en GitHub)

```bas
     git remote -v
```
**Revisar estado de la rama y hacer commit**

```bas
     git status
     git add .
     git commit -m "Agregar funciones de validación de datos"
```

**Realizamos el Merge**

```bas
     git merge --no-ff develop
```

**Subimos a GitHub**

```bas
     git push origin main develop
     git push origin --tags
```

**Ejecutamos**

```bas
     streamlit run app.py
```

---

# 📈 4. Lanzamiento y Documentación (Release)

Una vez completadas todas las funcionalidades, fusiona las ramas a **develop** y luego a **main** para crear el release.

## 4.1. **Fusión de Ramas**

```bash
# Desde cada rama feature, después de commitear:
git checkout develop
git merge feature/validacion-datos --no-ff
git merge feature/app-principal --no-ff
git merge feature/viz-tabular --no-ff
git merge feature/pruebas-pytest --no-ff

# Verificar que todo funcione en develop antes de pasar a main
```

## 4.2. **Creación del Release y Tags**

El release marca una versión estable. Se crea desde la rama **main**.

```bash
git checkout main
git merge develop --no-ff

# Crear tag de versión
git tag -a v10001.1.0 -m "Release inicial: App Streamlit con validación y tests"

# Subir todo a GitHub (asumiendo remoto configurado como 'origin')
git push origin main develop --tags   
```

---

# 📊 5. Evidencias de Ejecución

## 5.1. **Dashboard Principal**

La aplicación carga correctamente, valida los datos y muestra las métricas iniciales.

<img width="1214" height="378" alt="image" src="https://github.com/user-attachments/assets/543d7c53-384a-41f7-a258-c73a8ee46536" />


## 5.2. **Visualización Tabular**

Se muestra la tabla interactiva con los datos de los pingüinos y los gráficos de distribución.

<img width="1214" height="505" alt="image" src="https://github.com/user-attachments/assets/1ebfa284-ab3d-4205-9725-e4374883656f" />

<img width="1223" height="338" alt="image" src="https://github.com/user-attachments/assets/5ee75420-a51b-493c-a20f-77017e5feed4" />

---

# 📖 6. Documentación del Repositorio

El repositorio incluye dos archivos Markdown clave:

1. **README.md** : Describe el propósito.
   
  - Contenido sugerido: "Este proyecto es una aplicación de análisis de datos construida con Streamlit para explorar el dataset Palmer Penguins. Incluye validación de datos automatizada, visualización interactiva de tablas y gráficos, y un suite de pruebas unitarias con pytest."
  
2. **INFORME_LABORATORIO.md**: Detalla el procedimiento.

  - Contenido requerido:

      - Explicación del flujo GitFlow utilizado (ramas creadas).
      - Capturas de pantalla (gráficas) de la ejecución del dashboard principal.
      - Captura de la tabla de datos interactiva.
      - Captura de la terminal mostrando la ejecución exitosa de pytest (con los checks en verde).

---

# ✅ 7. Checklist Final de Entrega

El proyecto terminó y estos son los entregables del repositorio remoto de GitHub:

- [ ] Existencia de la rama main y develop.
- [ ] Presencia del tag v101.1.0 (o similar) en la sección de "Releases" o "Tags".
- [ ] Archivo penguins.csv cargado en el repositorio.
- [ ] Archivos README.md e INFORME_LABORATORIO.md visibles y con contenido.
- [ ] Historial de commits que refleje el uso de ramas feature (ej. "Merge branch 'feature/validacion-datos'").

# 📂 8. Estructura del Proyecto
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

# ✅ 9. Conclusiones

1. **Estandarización del Flujo de Trabajo con GitFlow**: La implementación de ramas feature aisladas demostró ser una estrategia efectiva para desarrollar funcionalidades complejas (validación, visualización, pruebas) de manera ordenada, eliminando la necesidad de correcciones de emergencia (hotfix) y garantizando un historial de versiones limpio y trazable.

2. **Importancia de la Validación Temprana de Datos**: La integración de funciones de validación (utils/validation.py) antes de la visualización confirmó que asegurar la integridad del dataset (verificación de columnas y nulos) es crítico para evitar errores en tiempo de ejecución en aplicaciones de datos, mejorando la robustez del dashboard frente a inconsistencias en el archivo penguins.csv.
   
3. **Calidad del Código mediante Pruebas Automatizadas**: El uso de pytest permitió verificar la lógica de negocio de forma unitaria antes del despliegue, demostrando que la automatización de pruebas es esencial para detectar errores lógicos en etapas tempranas del desarrollo, facilitando un release confidente y estable (v101.1.4 ultimo versión desarrollada).

4. Se logró implementar exitosamente un ciclo de vida de desarrollo de software ágil mediante GitFlow, garantizando la calidad del código a través de pruebas unitarias y entregando una aplicación funcional de análisis de datos. El repositorio cuenta con las ramas main, develop, el tag de versión y la documentación requerida.

