# Creación de una base de imágenes para una CNN

En este cuaderno vas a encontrar una opción simple para poder crear una base de datos de imágenes del tipo que quieras y luego etiquetarla para poder usarla en tu CNN.

>:traffic_light: _Notita: Esto se recomienda para la creación de bases relativamente pequeñas, menos de 500 imágenes por categoría_


1. **Buscar imágenes sin etiquetado:**

Para la búsqueda de estas imágenes se usó Google Imágenes, pues la mayoría son imágenes públicas y sin etiquetar. Esta búsqueda la puedes realizar para cada una de las diferentes categorías de imágenes que necesites.

<image src="+/image-3-1.png">

2. **Descarga de imágenes:**

Una vez encontradas las imágenes se procede con la descarga, para ello se va a usar una de las extensiones de Chrome, *Image Downloader - Save pictures*. Esta extensión lo que hace es encontrar todas las imágenes que aparecen en la página donde nos encontramos, en este caso Google Imágenes. 

En primera instancia, al activar la extensión vemos que se reconocen 231 imágenes, pero tras aplicar un filtro que seleccione únicamente las imágenes de extensión .JPG, este número se reduce a 131 imágenes. Luego hacemos click en **Select all** lo que activa la opción de **Download** para descargar todas las imágenes selecionadas.

>:traffic_light: _Notita: Esta extensión no me ha pagado por la mensión, si encuentras otra extensión que sea mejor, también la puedes usar._

<image src="+/image-1.png">

<p><center>Image Downloader - Save pictures sin filtro</center></p>
<image src="+/image-2-1.png">

<p><center>Image Downloader - Save pictures con filtro</center></p>
<image src="+/image-4-1.png">

<image src="+/image-5-1.png">

>:traffic_light: _Notita: Recomiendo que te tomes unos minutos para revisar las imágenes descargadas, con el fin de detectar la presencia de imágenes que no correspondan a lo que se estaba buscando._

3. **Etiquetado de imágenes:**

Como podrás ver, las imágenes que se descargaron tienen nombres diferentes.

<image src="+/image-6-1.png">

Por ello, el siguiente código va a realizar el trabajo de etiquetar. Lo primero que se hace es crear la función *rename_files_in_folder* que se encarga de tomar cada una de las imágenes que se encuentran dentro de la carpeta que se especifique, en este caso Descargas, y una por una le va a cambiar el nombre utilizando el _prefix_ que le especifiquemos, en este caso será *ensalada_*, a este prefijo se le adiciona un número empezando desde 1 hasta n (número total de imágenes en la carpeta), quedando así renombrada cada imagen.

### **Código en _python_**
```py
import os

def rename_files_in_folder(folder_path, prefix='ensalada_'):
    # Verifica si la carpeta existe
    if not os.path.exists(folder_path):
        print(f"La carpeta {folder_path} no existe.")
        return
    
    # Obtiene la lista de archivos en la carpeta
    files = [f for f in os.listdir(folder_path) if f.lower().endswith('.jpg')]
    files.sort()  # Ordena los archivos para renombrarlos secuencialmente
    
    # Renombra los archivos
    for i, filename in enumerate(files, start=1):
        # start indica desde qué número va a comenzar a etiquetar las imagenes
        new_name = f"{prefix}{i:03}.jpg"
        # el prefix, es el nombre en comun que van a tener todas las imagenes
        # el 03 indica la cantidad de digitos que tendran los numeros de las imagenes
        old_path = os.path.join(folder_path, filename)
        new_path = os.path.join(folder_path, new_name)
        os.rename(old_path, new_path)
        print(f"Archivo {filename} renombrado a {new_name}")

# Especifica la ruta de la carpeta que contiene las imágenes
folder_path = r'...\Downloads'

# Llama a la función para renombrar los archivos
rename_files_in_folder(folder_path)
```
______________________________________

¡Listo! Ya tienes una base etiqueta por ti. :blush:

> Es importante mencionar que se puede complejizar o personalizar el código. En caso de que se desee incluir más de 3 o 4 categorías en nuestra base, sería más conveniente automatizar el proceso de etiquetado. 
