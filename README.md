# yolov8MobileApp

Repositorio del desarrollo del Proyecto de la clase de **Tópicos Especiales 2024B1**.

## Integrantes:

- Lenin G. Falconí
- Mario Moreno
- Jonathan Zea


## Indicaciones Generales del Proyecto
Para navegar por el repositorio suponemos que la ubicación por defecto es la carpeta root o raíz de este repositorio (.) a partir de la cual se definen las diferentes rutas. Por ejemplo, para encontrar los Jupyter Notebooks se irá a la ruta `./yolov8mobileapp/notebooks`. Se proveerán rutas para facilitar la navegación a diferentes sitios de interés de este documento e.g. [notebooks](./yolov8mobileapp/notebooks).

El proyecto se divide en dos partes:
1. **yolov8mibileapp**: contiene información relevante al proyecto de
   ciencias de datos. Es decir, se describe la obtención del modelo
   Yolo en TFLITE. La estructura de archivos se ha obtenido con
   [cookiecutter](https://www.cookiecutter.io/) desde la template para
   datascience `ccds`.
2. **android_app**: contiene los archivos relevantes a la aplicación
   Android. Se sugiere revisar el proyecto en Android Studio.


## 1. Obtención de Modelo TF-LTE
Los pasos se encuentran en el
[yolo_explore.ipynb](./yolov8mobileapp/notebooks/yolo_explore.ipynb) y
en [yoloV8.ipynb](./yolov8mobileapp/notebooks/yoloV8.ipynb). Los pasos
principales son:
1. Clonar el repositorio de Yolo
2. Instalar los paquetes en Colab a partir de `requirements.txt`
3. Se hicieron pruebas de benchmark del Modelo
4. Se usó el script de `export.py` para salvar los pesos en formato de TFLITE
5. Se realizó una validación del modelo exportado en TFLITE alcanzando un mAP50 de 0.71
6. Se ejecutó una prueba sobre una imagen de muestra
7. Se copia el modelo a la carpeta de assets
``` bash
    cp yolov8mobileapp\models\yolov5s-fp16.tflite  android_app\android_app\app\src\main\assets\
```
## 2. Configuración Modelo YOLO
android_app\app\src\main\assets
android_app\app\src\main\java\com\surendramaran\yolov8tflite\Constants.kt
1. Se coloca el modelo en la carpeta de [assets](./android_app/app/src/main/assets)
2. Se coloca el archivo de labels en la carpeta de assets
3. Se ajusta las direcciones en el archivo [Constants.kt](https://github.com/LeninGF/yolov8MobileApp/blob/main/android_app/app/src/main/java/com/surendramaran/yolov8tflite/Constants.kt)
4. Reconstruir la aplicación y ejecutar

## 3. Optimización de Modelos para móviles
Los scripts de exportación de formato por defecto generan versiones de
16 y 32 bits. Los modelos se encuentran en:

- Modelo de 16 bits: [yolov8n_float16.tflite](./yolov8mobileapp/models/yolov8n_savedmodel/yolov8n_float16.tflite) de 65 MBytes
- Modelo de 32 bits: [yolov8n_float32.tflite](./yolov8mobileapp/models/yolov8n_savedmodel/yolov8n_float32.tflite) de 129 MBytes

Puede observarse que el modelo de 16 bits es más ligero y portable
para dispositivos móviles. Adicionalmente se utilizaron los scripts de
evaluación para conocer el rendimiento del modelo de 16 bits. 

``` python
python val.py --weights yolov5s-fp16.tflite
```

Los resultados de clasificación se pueden observar en las Figuras
siguientes. En general el modelo tiene una velocidad promedio de 0.4
ms para pre procesamiento y 268.4 ms para inferencia. La Precisión es
de 0.675, el Recall de 0.675 y el mAP50 de 0.71.

### Matriz de Confusión
![Matriz de Confusión](https://github.com/LeninGF/yolov8MobileApp/blob/main/yolov8mobileapp/reports/figures/validation_exported_model/confusion_matrix.png)
### Precision vs Recall
![Precision-Recall](https://github.com/LeninGF/yolov8MobileApp/blob/main/yolov8mobileapp/reports/figures/validation_exported_model/PR_curve.png)

Más imágenes se pueden consultar en la carpeta de [reports](https://github.com/LeninGF/yolov8MobileApp/tree/main/yolov8mobileapp/reports/figures/validation_exported_model)

## 4. Captura de Imágenes en Tiempo Real 
Varias capturas se han llevado a cabo con la Aplicación YOLO EPN Tflite. Éstas se encuentran en la carpeta 


![Captura1](https://github.com/LeninGF/yolov8MobileApp/blob/main/yolov8mobileapp/reports/Capturas/capture1.jpg)
![Captura2](https://github.com/LeninGF/yolov8MobileApp/blob/main/yolov8mobileapp/reports/Capturas/capture3.jpg)
![Captura3](https://github.com/LeninGF/yolov8MobileApp/blob/main/yolov8mobileapp/reports/Capturas/capture4.jpg)

A pesar de que el modelo es bastante económico para un dispositivo
móvil, depende un tanto del hardware del equipo un procesamiento
rápido. Por esta razón en eventos como el partido de fútbol o autos en
movimiento, la detección se da pero con un retraso o lag. La
aplicación de hecho muestra que la inferencia se realiza en el orden
de 300 mili segundos.

## 5. Pruebas
La aplicación con el modelo de YOLO responde bastante bien para
ambientes estáticos. Existen falsos positivos en algunos casos. Se ha
de observar, que el modelo utiliza un total de 80 categorías de
predicción. Es factible que el hardware del dispositivo influya en el
rendimiento del procesamiento. No obstante, el tamaño de la aplicación
es bastante amigable siendo de 197 MBytes.

## 6. Instalación
Para la instalación:

1. Descargue el app_v1.apk desde la carpeta [out](https://github.com/LeninGF/yolov8MobileApp/tree/main/android_app/out)
2. Instale en su equipo dando permisos de funcionamiento según convenga
3. Abra la aplicación.
4. Acepte el acceso a la cámara.
4. Apunte la cámara hacia los objetos que sean de su interés explorar. La lista de categorías puede ver en el siguiente [enlace](https://github.com/LeninGF/yolov8MobileApp/blob/main/android_app/app/src/main/assets/labels_es.txt)

## 7. Retos y soluciones
Uno de los principales retos al momento de desarrollar la apk fue la
integración entre el modelo de machine learning y el lenguaje de
programación Kotlin utilizado por Google en Android
Studio. Adicionalmente, otro desafío fue la interpretación de los
tensores de salida del Modelo YOLO pues, la salida de diferentes
versiones del modelo varía significativamente.
