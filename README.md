  # Actividad No. 1
# Laboratorio GIT aplicado a una aplicación Streamlit para análisis de datos
**Fecha:** 11 de julio de 2026
**Autor:** Giovanni Baño / Paúl Jara

# Grupo 4
## 🚀 Integrantes
| Nro. | Nombre | Link Proyecto |
|------|---------|---------|
| 1 | Giovanni Xavier Baño Jaya | https://github.com/Giovanni26101982/streamlit-penguins-analysis |
| 2 | Jara Pauta Cesar Paúl | https://github.com/PaulJara84/streamlit-penguins-analysis |


---

# 📖 1. Resumen Proyecto 

|Este proyecto consiste en el diseño y desarrollo de una aplicación web interactiva para el análisis exploratorio de datos, construida con el framework *Streamlit* en lenguaje Python. La plataforma automatiza la carga y visualización del dataset biográfico de los pingüinos de Palmer (penguins.csv), procesando la información a través de una arquitectura de software modular.|
|:---|

---

# 🛠️ 2. Problema

|¿Cómo desarrollar una aplicación en Streamlit que permita validar y visualizar el conjunto de datos penguins.csv, aplicando la metodología Gitflow para garantizar un desarrollo colaborativo, organizado y con control de versiones?|
|:---|

# 🎯 3. Objetivo

|Establecer la estructura de ramas adecuada, mediante GitFlow con la utilización de ramas específicas para desarrollo y producción, asegurando que la rama **main** (o **master**) contenga solo código estable.|
|:---|

## 🎯 3.1. Objetivo Específico

|Desarrollar una aplicación en Streamlit para validar y visualizar datos de un archivo CSV, aplicando la metodología Gitflow para gestionar el desarrollo y el control de versiones del proyecto.|
|:---|

---

# ✅ 4. Resultados a obtener

* Aplicación funcional en Streamlit para cargar, validar y visualizar datos de penguins.csv.
* Proyecto gestionado con Gitflow, incluyendo las ramas main, develop, feature y una release con su correspondiente tag.
* Pruebas unitarias implementadas con pytest.
* Repositorio en GitHub con el código fuente, README.md, informe.md y el archivo penguins.csv.
* Documentación y evidencia del funcionamiento de la aplicación y las pruebas realizadas.

---

# 📂 5. Estructura del Proyecto
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

