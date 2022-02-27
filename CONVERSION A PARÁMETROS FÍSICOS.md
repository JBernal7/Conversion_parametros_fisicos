Sensores: preprocesado, corrección y fusión de imágenes (III)
# CONVERSION A PARÁMETROS FÍSICOS

Jessica Bernal Borrego
27/02/22

##### Este ejercicio muestra la conversión los valores de los Niveles Digitales (ND) en distintos parámetros físicos en función de la longitud de onda de las bandas. Antes de efectuar estas operaciones, es conveniente indicar que para transformar ND a magnitudes físicas se habría de realizar primero una corrección atmosférica (Ver Parte II).
##### Landsat ofrece distintos tipos de productos para su análisis, incluyendo dos niveles de procesamiento de las imágenes:
#####  - Level 1: imágenes que ofrecen ND. Estos datos se pueden convertir en radiancia o reflectancia en el sensor utilizando parámetros de escala aditivos y multiplicativos.
##### - Level 2: imágenes que muestran la reflectancia en la zona baja de la atmósfera (BOA).  En este caso, la escena ha sufrido un proceso de corrección atmosférica ya.

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f32.png)



### 1. Radiancia
##### La transformación de ND a radiancia se puede hacer mediante el método de ganancia y sesgo. Este método, que convierte los valores de los ND de la imagen en valores de radiancia espectral (Lλ), utiliza la siguiente ecuación: 
> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f33.png)


##### Los valores de la ganancia (*gain*) y sesgo (*offset*) se pueden consultar en los metadatos de las imágenes. Para nuestro caso son los siguientes:

~~~
  GROUP = LEVEL1_RADIOMETRIC_RESCALING
    RADIANCE_MULT_BAND_1 = 1.2866E-02
    RADIANCE_MULT_BAND_2 = 1.3175E-02
    RADIANCE_MULT_BAND_3 = 1.2140E-02
    RADIANCE_MULT_BAND_4 = 1.0237E-02
    RADIANCE_MULT_BAND_5 = 6.2648E-03
    RADIANCE_MULT_BAND_6 = 1.5580E-03
    RADIANCE_MULT_BAND_7 = 5.2513E-04
    RADIANCE_MULT_BAND_8 = 1.1586E-02
    RADIANCE_MULT_BAND_9 = 2.4484E-03
    RADIANCE_MULT_BAND_10 = 3.3420E-04
    RADIANCE_MULT_BAND_11 = 3.3420E-04
    RADIANCE_ADD_BAND_1 = -64.32843
    RADIANCE_ADD_BAND_2 = -65.87310
    RADIANCE_ADD_BAND_3 = -60.70150
    RADIANCE_ADD_BAND_4 = -51.18692
    RADIANCE_ADD_BAND_5 = -31.32385
    RADIANCE_ADD_BAND_6 = -7.78996
    RADIANCE_ADD_BAND_7 = -2.62563
    RADIANCE_ADD_BAND_8 = -57.92951
    RADIANCE_ADD_BAND_9 = -12.24207
    RADIANCE_ADD_BAND_10 = 0.10000
    RADIANCE_ADD_BAND_11 = 0.10000

~~~
##### Como vemos, cada banda posee unos parámetros de conversión de radiancia, con lo que las operaciones de conversión han de hacerse de forma independiente para cada una de ellas. Como ejemplo utilizaremos la banda 5, para la que se considera una ganancia de 0.0062648 y un sesgo de -31.32385.
##### Accedemos a la Calculadora Ráster desde el menú *Ráster → Calculadora ráster*:

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f34.png)


##### La imagen resultante es visualmente similar a la imagen original, ya que lo que varía son los valores radiométricos. Si se comparan los valores de un mismo píxel de la banda original y la banda de radiancia, observamos como la banda original presenta valores  enteros (ND) y la banda de radiancia valores reales (radiancia = energía).
 
![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f35.png)


### 2. Reflectancia
##### Este paso se suele calcular a partir de los valores de radiancia, para la mayoría de los sensores se realiza siguiendo la ecuación:
 
> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f36.png)


##### Donde,
##### • 𝜌𝑠𝑒𝑛𝑠𝑜𝑟 es la reflectancia en el sensor (imagen a generar).
##### • Lλ es la radiancia espectral en el sensor 
##### • π =3.14159
##### • d es la distancia entre la tierra y el sol, en unidades astronómicas, que se puede calcular usando la ecuación (Eva and Lambin, 1998). 

> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f37.png)


##### • ESunλ es la irradiancia solar en la banda de interés (λ) medida en el tope de la  atmósfera. 
##### • 𝜃𝑖 es el ángulo cenital solar (ángulo cenital = 90 – ángulo de elevación solar).

##### En sensores antiguos es necesario hacer una transformación de ND a radiancia para posteriormente convertir radiancia en reflectancia. Sin embargo, en Landsat 8 la conversión a reflectancia puede hacerse directamente desde la imagen original. 
##### Podemos realizar una primera aproximación de reflectancia utilizando la ecuación del sesgo y la ganancia adaptada: 

> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f38.png)


##### Más preciso es utilizar la corrección del ángulo solar de los valores obtenidos. USGS ofrece herramientas que permiten obtener el ángulo solar de cada píxel para hacer una corrección avanzada del ángulo solar de cada píxel. No obstante, en nuestro ejemplo se usará el valor del ángulo de elevación solar del centro de la escena como aproximación a toda la imagen.

> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f39.png)


##### Los parámetros los podemos encontrar consultando los metadatos de la imagen, que en nuestro caso son:
~~~
REFLECTANCE_MULT_BAND_1 = 2.0000E-05
    REFLECTANCE_MULT_BAND_2 = 2.0000E-05
    REFLECTANCE_MULT_BAND_3 = 2.0000E-05
    REFLECTANCE_MULT_BAND_4 = 2.0000E-05
    REFLECTANCE_MULT_BAND_5 = 2.0000E-05
    REFLECTANCE_MULT_BAND_6 = 2.0000E-05
    REFLECTANCE_MULT_BAND_7 = 2.0000E-05
    REFLECTANCE_MULT_BAND_8 = 2.0000E-05
    REFLECTANCE_MULT_BAND_9 = 2.0000E-05
    REFLECTANCE_ADD_BAND_1 = -0.100000
    REFLECTANCE_ADD_BAND_2 = -0.100000
    REFLECTANCE_ADD_BAND_3 = -0.100000
    REFLECTANCE_ADD_BAND_4 = -0.100000
    REFLECTANCE_ADD_BAND_5 = -0.100000
    REFLECTANCE_ADD_BAND_6 = -0.100000
    REFLECTANCE_ADD_BAND_7 = -0.100000
    REFLECTANCE_ADD_BAND_8 = -0.100000
    REFLECTANCE_ADD_BAND_9 = -0.100000
~~~
~~~
SUN_ELEVATION = 36.48435176
~~~
##### Como para la radiancia, a cada banda se le aplicaría unos parámetros de conversión de reflectancia utilizando los valores sesgo y ganancia correspondientes.
##### Realizamos la operación para la banda 5, que presenta una ganancia de 0.00002, un sesgo de -0.1 y un ángulo de elevación solar de 36.48435176 (*Ráster --> Calculadora ráster*)

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f3a.png)


##### Comparamos los valores de un mismo píxel en la banda original y la banda de reflectancia, y observamos como la banda original presenta valores enteros (ND) y la banda de reflectancia valores reales que van de 0 (0% de reflectancia) a 1 (100% de reflectancia).

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f3b.png)



### 3. Temperatura
##### Landsat 8 también capta información del infrarrojo térmico, lo que permite obtener información de la temperatura de la superficie terrestre. Hay que indicar que este paso es dependiente de que previamente se realice una correcta corrección atmosférica, de lo contrario se obtendría la temperatura en la parte alta de la atmósfera.
##### La transformación de ND a temperatura se hace en dos pasos. En primer lugar, es necesario convertir los valores ND de la imagen en valores de radiancia espectral (Lλ). A continuación se calcularía la temperatura.
##### Para el cálculo de la radiancia, se utiliza el método de sesgo y ganancia. Los valores de sesgo y ganancia obtenidos de los metadatos son los siguientes:

~~~
RADIANCE_MULT_BAND_10 = 3.3420E-04
RADIANCE_MULT_BAND_11 = 3.3420E-04
RADIANCE_ADD_BAND_10 = 0.10000
RADIANCE_ADD_BAND_11 = 0.10000

~~~
##### Poniendo como ejemplo el cálculo de temperatura de la banda 11, el valor de ganancia será 0.0003342 y el valor de sesgo será 0.1.
 
![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f3c.png)



##### Comprobamos la transformación de los valores de radiométricos. Los nuevos valores de la banda de radiancia son valores reales (radiancia = energía):
 
![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f3d.png)


##### Una vez que se ha obtenido la radiancia, se procede a obtener la temperatura mediante la siguiente ecuación:

> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f3e.png)


##### Los valores de las constantes K1 y K2 se obtienen del archivo de metadatos:

~~~
GROUP = LEVEL1_THERMAL_CONSTANTS
    K1_CONSTANT_BAND_10 = 774.8853
    K2_CONSTANT_BAND_10 = 1321.0789
    K1_CONSTANT_BAND_11 = 480.8883
    K2_CONSTANT_BAND_11 = 1201.1442

~~~
##### Para la Banda 10, el valor de K1 es 774.8853 y el de K2 es 1321.0789. Procedemos a ejecutar la función en la calculadora ráster:

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f3f.png)



##### Comprobamos cómo han variado los valores de radiométricos, siendo los nuevos valores de la banda de radiancia valores reales:

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f40.png)





##### Esta temperatura corresponde a grados Kelvin, si queremos convertir los valores de temperatura de Kelvin a Celsius, sólo tenemos que restar a la imagen -273.15,
 

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f41.png)



![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f42.png)



 





