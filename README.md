# yolov8MobileApp

Repositorio del desarrollo del Proyecto de la clase de **Tópicos Especiales 2024B1**.

## Indicaciones Generales de Navegación
Para navegar por el repositorio suponemos que la ubicación por defecto es la carpeta root o raíz de este repositorio (.). A partir de dicha localidad se definen las diferentes carpetas y archivos. Por ejemplo, para encontrar un Jupyter Notebook se irá a la ruta `./yolov8mobileapp/notebooks`.

El proyecto se divide en dos partes:
- yolov8mibileapp: contiene información relevante al proyecto de datascience. E.g. obtención del modelo Yolo en TFLITE. Se lo ha preparado de esta manera para en caso de desear hacer un *fine tuning* al modelo con otros datos, colocar los archivos necesarios para hacerlo. La estructura de este folder se ha obtenido con cookiecutter desde la template para datascience `ccds`.
- android_app: contiene los archivos relevantes a la aplicación Android. Se sugiere revisar el proyecto en Android Studio.
## Obtención de Modelo TF-LTE
1. Se genera el 