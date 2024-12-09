# yolov8MobileApp

Repositorio del desarrollo del Proyecto de la clase de **Tópicos Especiales 2024B1**.

## Indicaciones Generales de Navegación
Para navegar por el repositorio suponemos que la ubicación por defecto es la carpeta root o raíz de este repositorio (.). A partir de dicha localidad se definen las diferentes carpetas y archivos. Por ejemplo, para encontrar un Jupyter Notebook se irá a la ruta `./yolov8mobileapp/notebooks`.

El proyecto se divide en dos partes:
- yolov8mibileapp: contiene información relevante al proyecto de datascience. E.g. obtención del modelo Yolo en TFLITE. Se lo ha preparado de esta manera para en caso de desear hacer un *fine tuning* al modelo con otros datos, colocar los archivos necesarios para hacerlo. La estructura de este folder se ha obtenido con cookiecutter desde la template para datascience `ccds`.
- android_app: contiene los archivos relevantes a la aplicación Android. Se sugiere revisar el proyecto en Android Studio.
## Obtención de Modelo TF-LTE
Los pasos se encuentran en el notebook `yolov8mobileapp\notebooks\yolo_explore.ipynb` y son
1. Clonar el repositorio de Yolo
2. Instalar los paquetes en Colab a partir de `requirements.txt`
3. Se hicieron pruebas de benchmark del Modelo
4. Se usó el script de `export.py` para salvar los pesos en formato de TFLITE
5. Se realizó una validación del modelo exportado en TFLITE alcanzando un mAP50 de 0.71
6. Se ejecutó una prueba sobre una imagen de muestra
7. Se copia el modelo a la carpeta de assets

    cp yolov8mobileapp\models\yolov5s-fp16.tflite  android_app\android_app\app\src\main\assets\

## Aplicación Android Studio
1. Se coloca el modelo en la carpeta de assets
2. Se coloca el archivo de labels en la carpeta de assets
3. Se ajusta las direcciones en el archivo `android_app/android_app/app/src/main/java/com/surendramaran/yolov8tflite/Constants.kt`
4. Reconstruir la aplicación y ejecutar