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
3. Se ajusta las direcciones en el archivo [Constants.kt](./android_app/app/src/main/java/com/surendramaran/yolo8tflite/Constants.kt)
4. Reconstruir la aplicación y ejecutar

## 3. Optimización de Modelos para móviles
Los scripts de exportación de formato por defecto generan versiones de
16 y 32 bits. Los modelos se encuentran en:

- Modelo de 16 bits: [yolov8n_float16.tflite](./yolov8mobileapp/models/yolov8n_savedmodel/yolov8n_float16.tflite) de 65 MBytes
- Modelo de 32 bits: [yolov8n_float32.tflite](./yolov8mobileapp/models/yolov8n_savedmodel/yolov8n_float32.tflite) de 129 MBytes

Puede observarse que el modelo de 16 bits es más ligero y portable
para dispositivos móviles. Adicionalmente se utilizaron los scripts de
evaluación para conocer el rendimiento del modelo de 16 bits. 

```python
python val.py --weights yolov5s-fp16.tflite
```

Los resultados de clasificación se pueden observar en las Figuras
siguientes. En general el modelo tiene una velocidad promedio de 0.4
ms para pre procesamiento y 268.4 ms para inferencia. La Precisión es
de 0.675, el Recall de 0.675 y el mAP50 de 0.71.






