# 6. Caso práctico

Enunciado



El ejercicio de evaluación final va a tratar sobre clasificación de imagen. Para esta ocasión, se van a crear múltiples clasificadores sobre imágenes que representen escenas o paisajes. Los datos que se van a emplear fueron publicados por Intel para competiciones de clasificación de imagen. La fuente original de los datos puede encontrarse aquí: https://www.kaggle.com/datasets/puneet6060/intel-image-classification 



Los modelos realizarán clasificación multiclase, ya que cada imagen puede contener seis etiquetas diferentes que son las que siguen:



0 Buildings.
1 Forest.
2 Glacier.
3 Mountain.
4 Sea.
5 Street.


La estructura de archivos que se pueden encontrar en los datos de la práctica final es la siguiente:



-Archive
Seg_pred
Seg_pred (7301 archivos)
3.jpg
5.jpg
…
24333.jpg
Seg_test
Seg_test
Buildings (437 archivos)
20057.jpg
…
Forest (474 archivos)
Glacier (553 archivos)
Mountain (525 archivos)
Sea (510 archivos)
Street (501 archivos)
Seg_train
Seg_train
Buildings (2191 archivos)
Forest (2271 archivos)
Glacier (2404 archivos)
Mountain (2512 archivos)
Sea (2274 archivos)
Street (2382 archivos)


Los datos totales sobre las imágenes pertenecen a 14 000 imágenes para train, 3000 para test y 7000 en predicción (aunque, al no estar provista de etiqueta, no se va a hacer uso de esta última); por lo tanto, el conjunto de datos es de 24 000 imágenes.



Respecto a cada imagen, todas tienen una estructura de 150 x 150 píxeles. La idea con los múltiples clasificadores es poner en práctica lo que se ha aprendido durante el módulo, es decir, aplicar redes convolucionales, realizar transfer learning, saber optimizar y seleccionar los parámetros de una red y, adicionalmente, superar un nuevo reto que no se ha trabajado de forma automática en el módulo: aumentar los datos.



Durante el módulo, se ha trabajado con OpenCV, por lo que el alumnado debería saber manejar los tres channels de una imagen; pues se ha explicado cómo aplicar filtros para cambiar el color, recortar, redimensionar, rotar y, en definitiva, modificar una imagen. Si a cada imagen se le hacen varias modificaciones y se agrega tanto la original como las modificadas al dataset, se estaría realizando un aumento de datos, no obstante. Hay funciones que hacen esto de forma automática, por lo que, habrá que aprender en la práctica final a utilizar una nueva función de Keras, ImageDataGenerator (https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator). Puesto que es muy habitual en proyectos reales, tener que adaptarse y comprender nuevas funciones de estos frameworks de deep learning, la idea es que el alumnado simule una situación de proyectos reales).



Se pide



Por lo tanto, el objetivo final del caso práctico es establecer una comparativa de modelos para ver cuál ofrece mayor rendimiento (accuracy) en test y en validación; la elección de cuántos modelos implementar y sus configuraciones es libre, pero como mínimo han de implementarse los siguientes cinco modelos sobre los que establecer la comparativa:



Modelo base basado en redes neuronales convolucionales: la arquitectura CNN de este modelo es libre, pero al menos debe tener una capa convolucional y una capa pooling (de libre elección el tipo de pooling) antes de la capa densa de salida. En el siguiente apartado se implementará de forma más compleja.
Modelo aumentado sobre el modelo base: es decir, que añade más capas a la arquitectura convolucional anterior.
Modelo basado en hyperparameter tuning a través de keras_tuner: en este modelo se pueden trabajar parámetros de regularización (L1 y L2), inicialización de pesos, funciones de activación, añadir sí o no dropout, etc. La elección de los hiperparámetros es libre, pero se aplicará al segundo modelo, es decir, al que se ha añadido más capas sobre el modelo base.
Modelo basado en transfer learning: el modelo preentrenado sobre el que cargar los pesos de la red es libre, siempre y cuando se elija cualquiera deKeras (https://keras.io/api/applications/). Se puede escoger realizar tanto featureextraction como fine-tuning, aunque se valorará más positivamente (dadala complejidad de congelar capas de entrenamiento) realizar fine-tuning.
Modelo basado en aumento de datos sobre cualquiera de los cuatro modelos implementados anteriormente (se aconseja sobre el modelo que mayor score haya devuelto la función evaluate). Para el aumento de datos, debe emplearse lafunción ImageDataGenerator, la elección de parámetros en ImageDataGeneratores libre.
Cualquier otro modelo que el alumno desee implementar y añadir a la comparativa(opcional).


Se aconseja, para todos los modelos, mostrar predicciones (desde imshow).



Para todos los modelos, se tomará un 20 % de la muestra como conjunto de validación (se puede generar un nuevo conjunto de validación o tomar el parámetro validation_split = 0,20).



De forma obligatoria todas las imágenes de seg_test y seg_train (no se cargará seg_pred) formarán el conjunto completo de datos, una vez que se tengan estos, se tomará un 75 % para train y un 25 % para test. 



Igualmente, los modelos del uno al cinco han de implementarse desde Keras; si el alumnado desea realizar más modelos opcionales puede utilizar, además de esta, otras librerías de deep learning para Python, como TensorFlow en su abstracción a bajo nivel, Theano, PyTorch o similares.



Al finalizar el ejercicio se mostrará una gráfica comparativa como la siguiente sobre el validation accuracy, obtenido por los modelos:



cp1.png


Y una tabla comparativa como la siguiente con el resultado de la función evaluate en test de todos los modelos:



cp2.png


Nota:



Los parámetros de configuración de la red como número de filtros en capas convolucionales, tamaño del kernel, desplazamiento del kernel o stride son libres, así como los parámetros de las capas de pooling y valores de padding. Por supuesto, también es libre la elección del número de neuronas de las capas densas al aplanar resultados, antes de la capa de salida final, y el optimizador también es de elección libre. No obstante, dada la naturaleza del ejercicio, para la función de coste (loss), dependiendo del preprocesamiento de datos, se tomará o bien categorical_crossentropy o sparse cateoricalcross entropy.
Los parámetros de batch size onúmero de etapas (epochs) también son libres, ya que dependen en gran medida de la disponibilidad de recursos como GPU o RAM.


Consejos importantes para la práctica



Es una práctica exigente en RAM; para no realizar excesivas epochs se puede hacer uso de callbacks deKeras como la parada temprana (early stopping).
Si no se dispone de mucha RAM o se quiere tener acceso a GPU gratuita (con ciertas restricciones), se puede hacer uso de Google Colab.
Si no es posible realizar toda la práctica en un solo notebook, debido a problemas de recursos, se puede hacer lo siguiente:


En un notebook cargar las imágenes, obtener los tensores preprocesados, cualquier array de NumPy puede guardarse con la función np.save(con extensión “.npy”); posteriormente, generar un notebook por modelo de red neuronal, en cada modelo hacer uso de la función np.load y asíevitar tener que cargar las imágenes, pues se estarían cargando directamente lostensores.
Implementar un modelo por cada notebook (en cada cual se realizará np.load para cargar los tensores). Después de entrenar el modelo, se pondrá la atención en dos elementos para después poder generar la gráfica comparativa.


El history del modelo, es un JSON que puede exportarse con JSON dumps o con la función write.
El modelo completo puede exportarse como “.h5” con la función save (https://www.tensorflow.org/guide/keras/save_and_serialize?hl=es-419). Después de guardar el modelo, enotro notebook se puede cargar y realizar posteriormente la función evaluate en test (también se pueden exportar los modelos como JSON; es posible ver ejemplos de ello en el enlace anterior).
De esta forma, cuando se implemente un modelo, se guardará, se apagará el notebook y se liberará RAM. Posteriormente, se deberá cargar un modelo, que es menos costoso en recursos que entrenarlo de forma completa.
Si se siguen teniendo problemas de RAM, ya que los tensores son considerables en memoria y el alumnado no tiene recursos para cargar las 17 000 imágenes de entrenamiento, se pueden cargar únicamente 3000, por ejemplo, 500 imágenes de cada label de seg_train(se recomienda probar Google Colab antes de realizar este paso, y los modelos se pueden guardar en Drive).


Entregable



Una carpeta comprimida (".rar” o “.zip”), en la cual estén los notebooks tanto en formato “.ipynb” como en “.html” al guardar el notebook. 



Nota: no es necesario entregar modelos guardados en “.h5” ni arrays en NPY ni los archivos de origen con las imágenes; únicamente se entregan los notebooks.



 

Anexo



Ejemplo de uso de ImageDataGenerator.



datagen = ImageDataGenerator(

       rotation_range=40,

       width_shift_range=0.2,

       height_shift_range=0.2,

       validation_split  = 0.2

       rescale=1./255,

       shear_range=0.2,

       zoom_range=0.2,

       horizontal_flip=True,

       fill_mode='nearest')

 

datagen.fit(X_train)

 

train_generator = datagen.flow(X_train, y_train, batch_size=32, subset='training')

 

validation_generator = datagen.flow(X_train, y_train, batch_size=32, subset='validation')

 

data_aug = model.fit(train_generator, 

                    epochs=EPOCHS, 

                    validation_data=validation_generator,

                    batch_size=BATCH_SIZE)

