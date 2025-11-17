# Proyecto: Pipeline de Detección y Clasificación de Imágenes en el Navegador

Este proyecto es una aplicación web que implementa un pipeline de visión por computadora de dos etapas directamente en el navegador del usuario. Utiliza la librería MediaPipe Tasks Vision de Google para realizar las siguientes acciones:

1.  **Detección de Objetos:** Analiza una imagen proporcionada por el usuario para encontrar y localizar objetos genéricos.
2.  **Clasificación de Objetos:** Recorta cada objeto detectado y lo envía a un segundo modelo, más específico, para obtener una clasificación detallada.

Todo el procesamiento se realiza en el lado del cliente, lo que garantiza la privacidad del usuario y una respuesta rápida sin necesidad de un servidor.

## 🚀 Cómo Usar

El proyecto está contenido en un único archivo HTML. No requiere instalación ni un servidor web.

1.  **Guarda el código:** Asegúrate de tener el archivo `index.html` (el código que proporcionaste) en tu computadora.
2.  **Ábrelo en un navegador:** Haz doble clic en el archivo `index.html` para abrirlo en un navegador web moderno (como Chrome, Firefox, Edge, etc.).
3.  **Selecciona una imagen:** Haz clic en el botón "Seleccionar archivo" y elige una imagen de tu computadora que contenga objetos.
4.  **Observa el resultado:** La aplicación dibujará un cuadro sobre cada objeto detectado con una etiqueta que muestra tanto la detección inicial como la clasificación final.

## 🛠️ Tecnologías Utilizadas

-   **HTML5 / CSS3:** Para la estructura y el estilo de la página.
-   **JavaScript (ES Modules):** Para la lógica de la aplicación.
-   **MediaPipe Tasks Vision:** La librería de Google para ejecutar modelos de machine learning en el navegador.

### Modelos de Machine Learning

Este pipeline utiliza dos modelos pre-entrenados y alojados por MediaPipe:

1.  **Detector de Objetos:** `EfficientDet-Lite0 (float16)` - Un modelo rápido y eficiente para localizar objetos generales.
2.  **Clasificador de Imágenes:** `EfficientNet-Lite0 (float32)` - Un modelo de alta precisión para clasificar imágenes.

## 🧠 Lógica del Pipeline (Detectar → Recortar → Clasificar)

El núcleo de esta aplicación es su flujo de trabajo en dos etapas:

1.  **Detección Global:** El modelo `ObjectDetector` recibe la imagen completa y devuelve las coordenadas (`bounding box`) de cada objeto que encuentra (ej: "dog", "chair").
2.  **Recorte Dinámico:** Para cada `bounding box` encontrada, se crea un `<canvas>` temporal en memoria. Se utiliza para "recortar" esa sección específica de la imagen original.
3.  **Clasificación Específica:** Cada imagen recortada (en su canvas temporal) se envía al `ImageClassifier`. Este modelo, al estar enfocado en una sola cosa, puede dar un resultado mucho más preciso (ej: "Golden Retriever", "Silla de oficina").
4.  **Visualización:** Finalmente, los resultados de ambos modelos se combinan y se dibujan sobre la imagen original para que el usuario pueda verlos.

## ⚠️ Nota Importante: Desafíos con TFLite y la Transición a LiteRT

Durante el desarrollo de este proyecto, se exploró la posibilidad de entrenar y utilizar un modelo personalizado en formato **TFLite**. Sin embargo, se encontraron importantes desafíos técnicos:

-   **Herramientas Obsoletas:** La librería `mediapipe-model-maker`, una herramienta clave para añadir los metadatos necesarios a los modelos TFLite para su uso con MediaPipe Tasks, presenta problemas de soporte y dependencias, lo que dificulta su uso en entornos locales.
-   **Falta de Soporte y Documentación:** La documentación y los ejemplos para el proceso de conversión de modelos Keras a un formato TFLite compatible con MediaPipe no están actualizados, lo que lleva a un proceso de prueba y error muy complejo.
-   **Evolución del Ecosistema:** Google está en proceso de transición de TensorFlow Lite a un nuevo runtime llamado **LiteRT**, optimizado para la inferencia en dispositivos de borde (edge). Esto significa que el soporte y las herramientas para el flujo de trabajo tradicional de TFLite están siendo progresivamente depreciados.

Debido a estas dificultades, se tomó la decisión de utilizar los modelos pre-entrenados y mantenidos por el equipo de MediaPipe para asegurar la funcionalidad y estabilidad del proyecto.

### Proceso de Entrenamiento (Modelo Personalizado)

A pesar de no poder integrarlo, se realizó el proceso de entrenamiento de un modelo personalizado con datos propios. El cuaderno de trabajo que documenta este proceso se puede encontrar en Google Colab:

-   **Cuaderno de Entrenamiento:** [Ver en Google Colab](https://colab.research.google.com/drive/1MFl2d2vjGq1cmlr-Putl_ZF_YGGjf9H9?usp=sharing)

### 📚 Para saber más sobre LiteRT

Para aquellos interesados en el futuro de la inferencia en el dispositivo, se recomienda consultar la documentación oficial sobre LiteRT:

1.  **Anuncio Oficial:** [TensorFlow Lite is now LiteRT](https://developers.googleblog.com/en/tensorflow-lite-is-now-litert/)
2.  **Guía General:** [LiteRT Overview](https://ai.google.dev/edge/litert/guide)
3.  **Guía de Migración:** [Migrate to LiteRT from TensorFlow Lite](https://ai.google.dev/edge/litert/migration)