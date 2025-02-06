# GAN para Fashion MNIST 👗🎨

![Fashion MNIST GAN](https://github.com/warc0s/Fashion_MNIST_GAN/blob/main/logo.jpg?raw=true)

Este repositorio contiene dos implementaciones de una Red Generativa Antagónica (GAN) entrenada sobre el dataset **Fashion MNIST**. Se han desarrollado dos versiones utilizando **TensorFlow** y **PyTorch**, permitiendo comparar el rendimiento y tiempos de entrenamiento entre CPU y GPU.

---

## Índice

- [Introducción](#introducción)
- [Características](#características)
- [Arquitectura de la GAN](#arquitectura-de-la-gan)
- [Detalles del Entrenamiento](#detalles-del-entrenamiento)
- [Estructura del Repositorio](#estructura-del-repositorio)
- [Cómo Ejecutarlo](#cómo-ejecutarlo)
- [Resultados](#resultados)
- [Licencia](#licencia)

---

## Introducción

En este proyecto se ha implementado una GAN con el objetivo de generar imágenes similares a las del dataset Fashion MNIST. La GAN se compone de dos redes neuronales:  
- **Generador:** Encargado de crear imágenes falsas a partir de un vector de ruido (espacio latente).  
- **Discriminador:** Su función es distinguir entre imágenes reales y las generadas.

Se han creado dos versiones del entrenamiento:
- **TensorFlow**: Ejecutado en CPU (Ryzen 7 8700G).
- **PyTorch**: Ejecutado en GPU (NVIDIA T4 en Google Colab).

---

## Características

- **Implementación Dual:** Tanto en TensorFlow como en PyTorch, permitiendo comparar diferencias en sintaxis y rendimiento.
- **Entrenamiento en Diferentes Hardwares:**  
  - **TensorFlow (CPU - Ryzen 7 8700G):**  
    - Tiempo por época: *4m 30s*  
    - Tiempo total (100 épocas): *7h 30m*
  - **PyTorch (GPU - T4 en Google Colab):**  
    - Tiempo por época: *26s*  
    - Tiempo total (100 épocas): *44m*
- **Resultados Comparables:** A pesar de las diferencias en hardware y tiempos de entrenamiento, el resultado final de ambas implementaciones es prácticamente idéntico.
- **Modelos y Visualizaciones:** Se subirán únicamente los modelos entrenados al **epoch 100** de ambas versiones. Además, se han guardado imágenes generadas cada 5 épocas durante el entrenamiento.

---

## Arquitectura de la GAN

La arquitectura utilizada en ambas implementaciones (TensorFlow y PyTorch) es la siguiente:

### Generador
- **Entrada:** Un vector de ruido de dimensión 100 (espacio latente).
- **Capas:**
  1. **Capa Densa + Normalización:** Transforma el vector de entrada a un mapa de características de dimensión *7x7x256*.
  2. **Reshape:** Se reorganiza la salida en una forma de tensor 7x7x256.
  3. **Convoluciones Transpuestas:**  
     - **Primera capa:** Incrementa la dimensión de 7x7 a 14x14 utilizando una convolución transpuesta, seguida de normalización y activación *LeakyReLU*.
     - **Segunda capa:** Aumenta la dimensión de 14x14 a 28x28, también con normalización y *LeakyReLU*.
  4. **Capa Final:** Genera una imagen de 28x28x1, usando la función de activación *tanh* para obtener valores en el rango [-1, 1].

### Discriminador
- **Entrada:** Imágenes de tamaño 28x28x1.
- **Capas:**
  1. **Convoluciones:** Se aplican dos capas convolucionales con *LeakyReLU* y *Dropout* para evitar sobreajuste.
  2. **Flatten:** Aplana el tensor para preparar la salida de la red.
  3. **Capa Densa Final:** Produce una salida única con activación *sigmoid*, que indica la probabilidad de que la imagen sea real.

> **Nota:** En PyTorch, la definición y estructuración de estas capas es análoga, utilizando las correspondientes clases y métodos propios del framework.

---

## Detalles del Entrenamiento

- **Dataset:** Fashion MNIST, preprocesado para normalizar las imágenes en el rango [-1, 1] y con el formato adecuado (28x28x1).
- **Optimización:**  
  - **Función de Pérdida:** Entropía cruzada binaria.  
  - **Optimizadores:** Se utiliza Adam con tasa de aprendizaje de 1e-4 tanto para el generador como para el discriminador.
- **Batch Size:** 32.
- **Épocas:** 100 en total.  
  - **Guardado de Modelos e Imágenes:**  
    - Cada 5 épocas se guardan los modelos en formato `.keras` o `.pt` en Pytorch, y se generan imágenes de ejemplo en una cuadrícula 3x3, que se guardan para seguimiento visual del entrenamiento.
- **Comparativa de Rendimiento:**  
  - *TensorFlow en CPU* es considerablemente más lento que *PyTorch en GPU*, pero los resultados finales son comparables.

---

## Estructura del Repositorio

```plaintext
.
├── models/                  # Modelos entrenados (solo epoch 100)
├── img_train_epochs/        # Imágenes generadas cada 5 épocas hasta el epoch 100
├── mnist_gan_tensorflow.ipynb  # Notebook de TensorFlow
├── mnist_gan_pytorch.ipynb       # Notebook de PyTorch
└── README.md                # Este archivo
```

- **Notebooks:**  
  Los notebooks contienen el código completo para entrenar la GAN en cada framework, con comentarios sobre el preprocesamiento, la arquitectura y el entrenamiento.
  
- **Modelos:**  
  Solo se suben los modelos correspondientes a la época 100 de cada implementación.
  
- **Imágenes de Ejemplo:**  
  En la carpeta `img_train_epochs` se guardan imágenes generadas durante el entrenamiento (cada 5 épocas) para visualizar el progreso del entrenamiento la GAN.

---

## Cómo Ejecutarlo

1. **Clonar el repositorio:**

   ```bash
   git clone https://github.com/warc0s/Fashion_MNIST_GAN
   cd Fashion_MNIST_GAN
   ```

2. **Instalar Dependencias:**

   - Para **TensorFlow**:
     ```bash
     pip install tensorflow matplotlib numpy
     ```
   - Para **PyTorch**:
     ```bash
     pip install torch torchvision matplotlib numpy
     ```
   
3. **Ejecutar las Celdas:**

   Ejecuta todas las celdas siguiendo el orden para reproducir el entrenamiento y generar los modelos y las imágenes.

---

## Resultados

- **Calidad de las Imágenes:**  
  A lo largo del entrenamiento, se pueden observar mejoras en la calidad de las imágenes generadas. Las imágenes guardadas en `img_train_epochs` muestran la evolución del generador cada 5 épocas.

- **Comparación entre Implementaciones:**  
  Aunque la versión en TensorFlow tarda más (usando CPU) en comparación con la versión en PyTorch (usando GPU), ambos enfoques logran resultados casi idénticos en la generación de imágenes.

---

## Licencia

Este proyecto se distribuye bajo la licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.
