# Catfishing-Sextortion PLN Filter: Research Excerpt

## Descripción General

Este repositorio contiene un **fragmento de una investigación en desarrollo** sobre detección y clasificación de fraudes cibernéticos mediante técnicas de Procesamiento de Lenguaje Natural (PLN). El objetivo principal es identificar y clasificar mensajes de estafas en comunicaciones digitales.

### Categorías de Clasificación

El sistema clasifica mensajes de texto en tres categorías:

1. **Catfishing** - Creación de identidades falsas para engañar emocionalmente a víctimas, solicitando dinero, información personal o generando dependencia emocional
2. **Sextortion** - Chantaje sexual donde el atacante amenaza con publicar contenido íntimo a menos que se cumplan demandas (generalmente monetarias)
3. **Harmless** - Comunicaciones legítimas sin intención fraudulenta

---

## Propósito de la Investigación

Evaluar y comparar el desempeño de modelos de lenguaje (LLMs) locales en la identificación de patrones de fraude cibernético, utilizando dos enfoques fundamentales en PLN:

- **Zero-Shot Learning**: Clasificación sin ejemplos previos, basándose únicamente en definiciones de categorías
- **Few-Shot Learning**: Clasificación con ejemplos sintéticos para mejorar la comprensión contextual

---

## Estructura del Repositorio

```
├── ModelsClassificationTests.ipynb    # Notebook principal de evaluación de modelos
├── CSV-Generator.py                   # Script para consolidar resultados en reportes CSV
├── scam-examples/
│   └── sintetic/                      # Ejemplos sintéticos (formato JSON)
└── LMMs-Classification-Test-Results/  # Directorio de salida de evaluaciones (generado en ejecución)
    └── Few-Shot-Sintetic-Aproach/     # Resultados de evaluaciones
```

---

## 🔧 Requisitos de Ejecución

### Dependencias Principales

El proyecto requiere las siguientes herramientas y librerías:

#### 1. **Entorno Python**
- Python 3.7 o superior

#### 2. **Librerías Requeridas**

##### Cliente Ollama: `ollama-python`
```bash
pip install ollama
```
- **Nombre técnico**: `ollama`
- **Propósito**: Proporciona el cliente Python para interactuar con modelos de lenguaje locales ejecutándose en Ollama
- **Función en el proyecto**: Conecta con la instancia local de Ollama (por defecto en `http://localhost:11434`) para enviar prompts y obtener predicciones de los modelos

##### Métricas de Clasificación: `scikit-learn`
```bash
pip install scikit-learn
```
- **Nombre técnico**: `sklearn` (nombre de importación) / `scikit-learn` (nombre del paquete)
- **Módulos específicos utilizados**:
  - `sklearn.metrics.classification_report`: Genera reportes detallados con métricas por clase (precision, recall, f1-score, support)
  - `sklearn.metrics.accuracy_score`: Calcula la precisión general del modelo
- **Propósito en el proyecto**: Evaluar el desempeño de cada modelo mediante métricas estándar de clasificación

##### Librerías Estándar (incluidas en Python):
- `json`: Lectura/escritura de archivos JSON con configuraciones y resultados
- `csv`: Generación de reportes CSV
- `os` y `pathlib`: Manejo de directorios y rutas
- `time` y `statistics`: Medición de tiempos de evaluación
- `subprocess`: Interacción con comandos del sistema (ej: `ollama list`)
- `re` (regex): Procesamiento y filtrado de respuestas de modelos
- `datetime`: Timestamps de evaluaciones

#### 3. **Herramienta Requerida: Ollama**

**Ollama** es un framework que permite ejecutar modelos de lenguaje de código abierto de forma local:

- **Descarga**: https://ollama.ai/
- **Requisito de ejecución**: Debe estar instalado y ejecutándose antes de correr el notebook
- **Verificación de instalación**:
  ```bash
  ollama --version
  ```
- **Verificación de disponibilidad**:
  ```bash
  ollama list  # Muestra los modelos disponibles
  ```
- **Puerto por defecto**: `http://localhost:11434` (configurable en el notebook)

---

## Dataset de Prueba

El proyecto utiliza **dataset sintético** generado específicamente para esta investigación. Los datos sintéticos están organizados en archivos JSON dentro del directorio `scam-examples/sintetic/`.

### Características del Dataset Sintético

- **Formato**: Archivos JSON estructurados con ejemplos de mensajes por categoría
- **Estructura esperada**: 
  ```json
  {
    "examples": [
      {
        "text": "mensaje de prueba aquí",
        "category": "catfishing|sextortion|harmless"
      }
    ]
  }
  ```
- **Propósito**: Permitir pruebas reproducibles y evaluaciones consistentes sin dependencia de datos reales

---

## Instalación y Configuración

### Paso 1: Clonar el Repositorio
```bash
git clone https://github.com/MisterPi14/Catfishing-Sextortion-PLN-Filter-Research-Excerpt.git
cd Catfishing-Sextortion-PLN-Filter-Research-Excerpt
```

### Paso 2: Crear Entorno Virtual (Recomendado)
```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

### Paso 3: Instalar Dependencias
```bash
pip install ollama scikit-learn
```

### Paso 4: Verificar Ollama
```bash
ollama list  # Verificar modelos disponibles
```

### Paso 5: Ejecutar el Notebook

Abrir `ModelsClassificationTests.ipynb` en Jupyter Notebook o JupyterLab:
```bash
jupyter notebook ModelsClassificationTests.ipynb
```

O en Jupyter Lab:
```bash
jupyter lab ModelsClassificationTests.ipynb
```

---

## 📖 Uso

### Workflow Principal

#### 1. **ModelsClassificationTests.ipynb**

Notebook que ejecuta la evaluación completa de modelos:

- **Configuración inicial**: Define parámetros de ejecución (enfoques, directorio de ejemplos, modelos a evaluar)
- **Carga de ejemplos**: Lee archivos JSON sintéticos de `scam-examples/sintetic/`
- **Inicialización de cliente Ollama**: Establece conexión con la instancia local
- **Evaluación iterativa**: Para cada modelo, realiza predicciones sobre el dataset de prueba
- **Generación de reportes**: Exporta resultados en formato JSON con:
  - Metadatos: Nombre del modelo, tamaño, fecha, versión del prompt
  - Predicciones: Respuestas raw del modelo
  - Métricas: Accuracy, precision, recall, f1-score por clase
  - Tiempos: Duración total y por predicción

**Salida**: Archivos JSON en `LMMs-Classification-Test-Results/Few-Shot-Sintetic-Aproach/`

#### 2. **CSV-Generator.py**

Script de post-procesamiento que consolida resultados:

```bash
python CSV-Generator.py
```

**Funciones**:
- Lee todos los archivos JSON generados por el notebook
- Extrae y redondea métricas (4 decimales)
- Convierte formato numérico a estándar europeo (coma decimal)
- Genera CSV con separador `;` (semicolon-delimited)
- Organiza columnas: Modelo → Accuracy → Métricas por clase → Métricas agregadas

**Salida**: Archivo CSV en `LMMs-Classification-Test-Results/` con nombre según la carpeta evaluada

---

## Configuración Personalizada

Dentro de `ModelsClassificationTests.ipynb`, editar variables de configuración:

```python
# Directorio de salida de resultados
OUTPUT_DIR = 'LMMs-Classification-Test-Results/Few-Shot-Sintetic-Aproach'

# Versión del prompt utilizado
PROMPT_VERSION = 'v1.3'

# Activar/Desactivar características
DEFINITIONS = False      # Incluir definiciones en el prompt
DICTIONARY = False       # Incluir diccionario de regionalismos
EXAMPLES = True          # Incluir ejemplos sintéticos (Few-Shot)

# Directorio con ejemplos y diccionarios JSON
EXAMPLES_DIR = 'scam-examples/sintetic'

# Selección de modelos específicos
SELECTED_MODELS_ONLY = True
MODELS_SELECTION = ["nombre-del-modelo:tamaño"]
```

---

## Salidas y Resultados

### Formato JSON de Evaluación

```json
{
  "metadata": {
    "model_name": "nombre-modelo",
    "model_size": "tamaño",
    "evaluation_date": "ISO-8601-format",
    "prompt_version": "v1.3",
    "training_dataset_size": "X samples (desglose por categoría)",
    "examples_directory": "ruta/de/ejemplos",
    "test_dataset_size": "X samples (10 de cada categoría)"
  },
  "results": {
    "classification_report": {
      "accuracy": 0.8333,
      "catfishing": {
        "precision": 0.8,
        "recall": 0.8,
        "f1-score": 0.8,
        "support": 10
      },
      "sextortion": {
        "precision": 0.9,
        "recall": 0.9,
        "f1-score": 0.9,
        "support": 10
      },
      "harmless": {
        "precision": 0.8,
        "recall": 0.8,
        "f1-score": 0.8,
        "support": 10
      }
    },
    "timing_metrics": {
      "total_evaluation_time_seconds": 125.34,
      "average_time_per_prediction_seconds": 4.18,
      "samples_per_second": 0.24
    }
  }
}
```

### Formato CSV Consolidado

| Model | accuracy | catfishing_precision | catfishing_recall | ... | weighted avg_f1-score |
|-------|----------|----------------------|-------------------|-----|----------------------|
| modelo_v1 (size) | 0,8333 | 0,8 | 0,8 | ... | 0,8333 |
| modelo_v2 (size) | 0,9167 | 0,9 | 0,9 | ... | 0,9167 |

---

## Enfoque Metodológico

### Zero-Shot Learning
Clasifica mensajes basándose únicamente en definiciones de categorías sin ejemplos previos. Útil para evaluar la capacidad de generalización del modelo.

### Few-Shot Learning
Proporciona ejemplos sintéticos de cada categoría en el prompt para mejorar la comprensión contextual. Demuestra cómo ejemplos representativos mejoran la precisión de clasificación.

---

## Notas sobre el Dataset Sintético

- **Generación**: Los ejemplos fueron creados mediante técnicas de síntesis de datos
- **Propósito**: Garantizar reproducibilidad y consistencia en las evaluaciones
- **Ventaja**: Permite pruebas sin depender de datos reales sensibles
- **Limitación**: Puede no capturar toda la complejidad del mundo real

---

## Limitaciones y Consideraciones

- Este es un **fragmento de una investigación en desarrollo**, no la solución final
- Los resultados son específicos al enfoque Few-Shot con ejemplos sintéticos
- El rendimiento puede variar según el modelo LLM seleccionado
- Requiere Ollama ejecutándose localmente con modelos instalados
- El dataset de prueba es limitado (10 muestras por categoría)

---

## Trabajo Futuro

- Evaluación con datasets más grandes y diversificados
- Comparación entre Zero-Shot y Few-Shot de forma cuantitativa
- Integración de datos reales (respetando privacidad)
- Fine-tuning de modelos específicamente para esta tarea
- Optimización del tamaño y estructura de prompts
- Análisis de errores y casos edge

---

## Licencia

Especificar según corresponda a tu proyecto

---

## Contacto

**Autor**: MisterPi14  
**Proyecto**: Catfishing-Sextortion PLN Filter Research  
**Estado**: Investigación en desarrollo

---

### Referencias Técnicas

- **Ollama**: https://ollama.ai/
- **Scikit-learn Classification Report**: https://scikit-learn.org/stable/modules/model_evaluation.html#classification-report
- **Zero-Shot Learning**: Brown et al. (2020) - Language Models are Few-Shot Learners
- **Few-Shot Learning**: Contexto dinámico mediante ejemplos sintéticos
