# Modelo LeNet con Límite de Precisión del 90%

## Informe de Hallazgos

Al realizar varias pruebas con el modelo LeNet, se observa que no se supera un *accuracy* del 90%. Inicialmente, se pensó que este comportamiento podría deberse a que el modelo LeNet es relativamente simple. Aunque es adecuado para el reconocimiento de dígitos del conjunto de datos MNIST, su arquitectura podría ser demasiado limitada cuando se trata de encontrar patrones más complejos. A medida que el modelo se entrena, parece alcanzar un límite en su capacidad para generalizar más allá del 90% de precisión, lo que indica que no puede aprender características más complejas de los datos.

Otro factor que podría estar influyendo en este estancamiento es la cantidad de épocas definida en el código. Aunque aumentar el número de épocas podría dar más tiempo al modelo para aprender, en este caso no se observó un cambio significativo en la precisión, lo que sugiere que el modelo no está mejorando con el tiempo debido a otros factores más allá del número de iteraciones.

Además, el uso del optimizador **SGD** (Descenso por Gradiente Estocástico) con una tasa de aprendizaje fija podría ser otra de las razones detrás del estancamiento en la precisión. El optimizador SGD es bastante simple y no adapta la tasa de aprendizaje durante el entrenamiento, lo que puede dificultar que el modelo encuentre una solución óptima. Este tipo de optimizador puede ser ineficaz para mejorar la precisión más allá del límite observado, ya que no ajusta dinámicamente la tasa de aprendizaje en función del progreso del entrenamiento, lo que podría hacer que el modelo tarde más en converger o se quede atrapado en un mínimo local.

Por lo tanto, la combinación de un modelo simple, una cantidad de épocas limitada y un optimizador básico como el SGD puede estar contribuyendo al estancamiento en la precisión del 90%, limitando el potencial de mejora del modelo.

## Resultados del Experimento

Para mejorar el rendimiento del modelo, se intentaron varias modificaciones:

### Aumento del número de épocas

Inicialmente se aumentó el número de épocas para permitir que el modelo tuviera más tiempo para aprender. Sin embargo, esto no resultó en un aumento de la precisión, ya que se estabilizó en el 90%, como se muestra en los siguientes resultados:

| Épocas | Loss   | Precisión (Accuracy) |
|--------|--------|----------------------|
| 1      | 0.3527 | 0.8998               |
| 2      | 0.3527 | 0.8998               |
| 3      | 0.3270 | 0.9000               |
| 4      | 0.3266 | 0.9000               |
| 20     | 0.3265 | 0.9000               |

Como se observa, la precisión se mantuvo estancada en torno al 90%, independientemente de las épocas adicionales.

### Cambio de Función de Activación

Luego de observar que no se lograban mejoras con el aumento de épocas, se cambió la función de activación de **tanh** a **ReLU**. La activación **ReLU** es más adecuada para evitar el problema de desvanecimiento del gradiente, ya que permite que los gradientes se mantengan mayores y más estables durante la retropropagación, lo que facilita el aprendizaje en redes profundas. Este cambio tiene el potencial de mejorar el aprendizaje del modelo y superar el límite de 90% de precisión.

#### Resultados con ReLU:

Después de cambiar la función de activación de **tanh** a **ReLU**, los resultados fueron los siguientes:

| Épocas | Loss   | Precisión (Accuracy) |
|--------|--------|----------------------|
| 1      | 0.3822 | 0.8997               |
| 2      | 0.3285 | 0.9000               |
| 3      | 0.3275 | 0.9000               |
| 4      | 0.3276 | 0.9000               |

A pesar de haber cambiado la función de activación, la precisión no mejoró significativamente, manteniéndose alrededor del 90%. Esto indica que la activación ReLU ayudó a estabilizar el aprendizaje, pero aún existen limitaciones en la capacidad del modelo.

### Optimización con Adam

También se probó cambiar el optimizador a **Adam**, ya que es más avanzado que el optimizador **SGD** y ajusta dinámicamente la tasa de aprendizaje durante el entrenamiento, adaptando los gradientes. Sin embargo, al revisar los resultados, ni la tasa de pérdida ni la precisión cambiaron significativamente, lo que sugiere que el optimizador no logró mejorar el rendimiento del modelo.

### Aumento de la Profundidad de la Red

De igual manera, se aumentó la profundidad de la red agregando más capas convolucionales o completamente conectadas, lo que permitiría que el modelo aprenda representaciones más complejas y sutiles de los datos, mejorando así su capacidad de generalización. Sin embargo, este cambio tampoco produjo una mejora significativa en los resultados, y la precisión se mantuvo estancada alrededor del 90%.

## Cambiar Tipo de Red

Con base en los experimentos realizados, parece que las modificaciones en el optimizador, la función de activación y la profundidad de la red no fueron suficientes para superar el límite de precisión del 90%. Una posible solución sería explorar una arquitectura de red más compleja, como redes neuronales convolucionales más profundas o modelos más avanzados, como redes residuales (**ResNet**) o redes más sofisticadas como **DenseNet**, que pueden aprender representaciones más complejas y, posiblemente, superar este límite.

Además, se podrían probar técnicas adicionales como el **aprendizaje transferido**, utilizando modelos preentrenados en tareas similares y luego ajustándolos al conjunto de datos de MNIST. Este enfoque podría permitir que el modelo aproveche características ya aprendidas y mejore la capacidad de generalización.

Otra alternativa sería aplicar técnicas de **regularización** para evitar el sobreajuste y mejorar el rendimiento, como **Dropout** o **Data Augmentation**, que crearían más variabilidad en los datos de entrenamiento y podrían ayudar a que el modelo generalice mejor en datos no vistos.
