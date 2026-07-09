# Informe de Laboratorio: Aplicación Streamlit con GitFlow

## 1. Objetivo

Desarrollar una aplicación de análisis de datos sobre el dataset Palmer Penguins utilizando Streamlit, implementando un flujo de trabajo de control de versiones GitFlow e integrando pruebas automatizadas con pytest.

## 2. Procedimiento Realizado

2.1. **Configuración del Repositorio y GitFlow**

Se inicializó el repositorio estableciendo la rama **main** para producción y **develop** para integración. El desarrollo se realizó mediante ramas de característica (**feature/**) aisladas para evitar conflictos:

- **feature/validacion-datos**: Implementación de lógica de validación de esquemas y nulos.
- **feature/app-principal**: Estructura base de la interfaz en Streamlit.
- **feature/viz-tabular**: Implementación de visualización de datos interactiva.
- **feature/pruebas-pytest**: Creación de tests unitarios para las funciones de validación.

Cada rama fue fusionada a **develop** utilizando **git merge --no-ff** para preservar el historial de características.

2.2. **Desarrollo de la Aplicación**

- **Validación**: Se crearon funciones en **utils/validation.py** para asegurar la integridad del dataset antes de su procesamiento.
- **Interfaz**: La aplicación **app.py** carga el archivo **penguins.csv**, valida los datos y muestra métricas clave (totales y nulos).
- **Visualización**: Se implementó una tabla interactiva y gráficos de distribución utilizando las libreras nativas de Streamlit.

2.3. Pruebas y Release
Se ejecutó el suite de pruebas con pytest, validando el correcto funcionamiento de los módulos de utilidad. Tras verificar la estabilidad en develop, se fusionó el código a main y se etiquetó la versión estable con el tag v1.0.0.

---

## 3. Evidencias de Ejecución

3.1. **Dashboard Principal**

La aplicación carga correctamente, valida los datos y muestra las métricas iniciales.

[INSERTAR AQUÍ CAPTURA DE PANTALLA DEL DASHBOARD PRINCIPAL CON MÉTRICAS]

3.2. **Visualización Tabular**

Se muestra la tabla interactiva con los datos de los pingüinos y los gráficos de distribución.

[INSERTAR AQUÍ CAPTURA DE PANTALLA DE LA TABLA DE DATOS Y GRÁFICOS]

3.3. **Pruebas con Pytest**

Ejecución exitosa de los tests unitarios en la terminal, confirmando la validez del código.

[INSERTAR AQUÍ CAPTURA DE PANTALLA DE LA TERMINAL CON EL RESULTADO DE PYTEST (verde)]

---

## 4. Conclusión

- Se logró implementar exitosamente un ciclo de vida de desarrollo de software ágil mediante GitFlow, garantizando la calidad del código a través de pruebas unitarias y entregando una aplicación funcional de análisis de datos. El repositorio cuenta con las ramas main, develop, el tag de versión y la documentación requerida.

