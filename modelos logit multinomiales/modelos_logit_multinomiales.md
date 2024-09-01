# Modelos *logit* multinomiales

En este cuaderno encontrarás código en R y Python para implementar este tipo de modelos multinomiales, además de una breve explicación sobre su funcionamiento.

>:traffic_light: _**Notita**: Aquí no se va a ahondar en la parte teórica de los modelos._

Para fines explicativos se va a usar los datos de [ebook reader](http://www.preferencelab.com/data/Ebook_Reader.csv).

## *mlogit*

Vamos a empezar con R, para poder realizar la aplicación del modelo, nos ayudaremos de la *librería* ***mlogit***, este es un paquete en R que permite la estimación de los ***modelos logit multinomiales*** con variables específicas individuales y/o alternativas.

En cuyo caso no tengas instalado el paquete, primero se debe de instalar.
```R
install.packages("mlogit")
```
Una vez instalado el paquete, vamos a iniciar con la generación del modelo.
```R
# Cargar la librería
library(mlogit)

# Cargar datos (simulados) sobre "ebook readers"
cbc <- read.csv(url("http://www.preferencelab.com/data/Ebook_Reader.csv"))

# Convertir datos para "mlogit"
cbc <- mlogit.data(cbc, choice="Selected", shape="long", alt.var="Alt_id", id.var = "Resp_id")

# Generación del modelo
ml1 <- mlogit(Selected ~ Storage_4GB + Storage_8GB + Screen.size_5inch + Screen.size_6inch + Color_black + Color_white + Price_79 + Price_99 + Price_119 + None | 0, cbc)

summary(ml1)
```
<image src="+/image.png">

:eyes: **Ojito**:  Si te diste cuenta al final del modelo, después del ```None``` aparece un ```| 0```. Esto se debe a que por defecto, se añade un intercepto al modelo, y para eliminar ese adicional se puede utilizar 0 o -1 en la segunda parte. Por lo que también lo podrías hacer el modelo de la siguiente manera:

```R
ml1 <- mlogit(Selected ~ Storage_4GB + Storage_8GB + Screen.size_5inch + Screen.size_6inch + Color_black + Color_white + Price_79 + Price_99 + Price_119 + None | -1, cbc)

summary(ml1)
```

## *xlogit*

Para el caso de Python se va a usar el paquete **xlogit**. El cual es un paquete de Python de código abierto que permite estimar modelos de ***Logit Mixto*** de manera eficiente utilizando aceleración por *GPU*. Pero si en caso no tengas *GPU*, también puedes usar este paquete, sin embargo, si necesitas acelerar la estimación de tu modelo, existen varias opciones para acceder a los recursos de la *GPU* en la nube, tales como: Google Colab, Amazon Sagemaker Studio Lab, Google Cloud platform, etc.

En cuyo caso no tengas instalado el paquete, primero se debe de instalar.
```python
pip install xlogit
```
Una vez instalado el paquete, ya podemos empezar con la generación del modelo.
```python
import pandas as pd
import numpy as np
from xlogit import MultinomialLogit

# Lectura de datos
url = "http://www.preferencelab.com/data/Ebook_Reader.csv"
df = pd.read_csv(url)

# Generación del modelo
df.info()

# Variables independientes que se van a usar
varnames=['Storage_4GB', 'Storage_8GB', 'Screen.size_5inch', 'Screen.size_6inch', 'Color_black', 'Color_white', 'Price_79', 'Price_99', 'Price_119', 'None']

# Crear una instancia del modelo logit multinomial
model = MultinomialLogit()
model.fit(X=df[varnames], y=df['Selected'], varnames=varnames, ids=df['Set_id'], alts=df['Alt_id'])

model.summary()
```
<image src="+/image-1.png">

___________________

¡Listo! Ya pudiste generar tu propio modelo multinomial. :blush:

>:traffic_light: _**Notita**: Para mayor información o entendimiento sobre el tema y códigos utilizados puedes revisar las **Referencias** que estoy dejando aquí abajo._

### :books: Referencias:
* [Choice-Based Conjoint Analysis](https://pure.rug.nl/ws/portalfiles/portal/198628555/Eggers2022_ReferenceWorkEntry_Choice_BasedConjointAnalysis.pdf)
* [Ejemplo "The ebook reader and estimated models"](http://www.preferencelab.com/data/CBC.R)
* [Estimation of multinomial logit models in R: The mlogit Packages [8,9]](https://www-eio.upc.edu/teaching/madt/apunts/mlogit_teoria.pdf)
* [Multinomial Logit](https://xlogit.readthedocs.io/en/latest/notebooks/multinomial_model.html)

