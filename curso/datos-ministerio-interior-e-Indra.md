## Análisis de datos de Indra

Para 2017 (al igual que 2015) los interesados pueden recibir datos de la elección durante el día de la misma y posteriormente. Estos datos se entregan en formatos reutilizables.  

Si desean formar parte del grupo que recibe esta información técnica es necesario manifestarlo por email a Yanina Sandra Martinez <ymartinez@mininterior.gob.ar>. Esta es la forma que el Ministerio del Interior provee esta información.  

Para las elecciones 2017 todos estos datos se compartieron [aquí](https://github.com/avdata99/datos-indra-dia-eleccion-paso-2017-AR/blob/master/info-previa-DINE.md).  

![repo-mininterior](../img/repo-mininterior.png)

El documento indica cuales y en que formatos están los documentos. El proceso a 
continuación es el uso de estos datos para construir resumenes y visualizaciones útiles.  

Para procecar estos daton abrimos una planilla de cálculo nueva de Google Drive.  

### Archivos incluidos

#### Tipos de elección

El primer dato es la lista de códigos para los cargos electivos. No esta en un CSV, es un texto suelto.  
Copiamos el texto y lo pegamos en la planilla. 

![dato-texto-01-indra](../img/dato-texto-01-indra.png)

Luego seleccionamos las celdas con texto y usamos la funcionalidad de _Dividir texto en columnas_ (en el menú _datos_) con el separador " = " (espacio, signo igual, espacio). Además le agregamos encabezado a las columnas. 

![dato-texto-02-indra](../img/dato-texto-02-indra.png)

**Nota para todos los datos:** El código 999 se refiere a totales provinciales.  

#### Ámbitos electorales

El alcance máximo de estos datos son las secciones electorales (en Córdoba, Departamentos). La lista de estos departamentos esta en el archivo [_ambitos_00.csv_](https://github.com/avdata99/datos-indra-dia-eleccion-paso-2017-AR/blob/master/recursos/DATOS-MUESTRA-2017-08-01/generales_00/ambitos_00.csv).  

Tip: Este archivo en este caso está en la plataforma _GitHub_ (la más usadas por los programadores para compartir código y datos). Para poder ver esto en su formato real y descargarlo o copiarlo le agregamos _?raw=true_ al final de link en el navegador.  

Tip II: Notó que separa los campos con _punto y coma_? Es _Excel Friendly_.  

![raw-true](../img/raw-true.png)

Allí el texto se podrá mostrar tal cual es. Lo copiamos y pegamos en una **hoja nueva** de nuestra planilla de drive. Será necesario nuevamente _dividir el texto en columnas_. Esta vez el separador es el punto y coma.  

Hay que agregar los nombres de las columnas: Provincia, Sección y Nombre Sección.  
Aquí uno puede _adivinar_ que Córdoba es la número 4, este dato nos va a servir para los datos a continuación.  

#### Totales

Hay un archivo de totales por cada provincia. Con _totales_ se refiere a datos generales de cada departamento, a los votos generales (positivos, válidos, en blanco, etc), no a los votos que recibió cada lista.  
En este caso, el de Córdoba es [_totales_04.csv_](https://github.com/avdata99/datos-indra-dia-eleccion-paso-2017-AR/blob/master/recursos/DATOS-MUESTRA-2017-08-01/DATOS_89822634/totales_04.csv).  

Al igual que el anterior agregamos _?raw=true_ al final para poder llevarlo a una nueva hoja en la planilla de Google Drive.  

```
3;04;001;03254;00790;02428;01109229;00228008;02056;08472;00269120;02426;00197652;08669;00172862;07581;00024790;01087;00011052;00485;00019304;00847;07;29;14;15;
3;04;002;00163;00113;06933;00051517;00029735;05772;08481;00035062;06806;00025773;08668;00022539;07580;00003234;01088;00001438;00484;00002524;00849;07;29;14;15;
3;04;003;00653;00289;04426;00222846;00083426;03744;08491;00098250;04409;00072380;08676;00063305;07588;00009075;01088;00004044;00485;00007002;00839;07;29;14;10;
3;04;004;00159;00101;06352;00049037;00026358;05375;08419;00031309;06385;00022837;08664;00019974;07578;00002863;01086;00001270;00482;00002251;00854;07;29;14;20;
3;04;005;00090;00072;08000;00028384;00019032;06705;08454;00022512;07931;00016502;08671;00014433;07584;00002069;01087;00000920;00483;00001610;00846;07;29;13;45;
```
Según el documento se indica que los campos: 

|DESCRIPCIÓN | LONGITUD|
|------------|---------|
|Código de elección | 1|
|Código de provincia | 2|
|Código de comuna | 3|
|Total de mesas | 5|
|Mesas escrutadas | 5|
|Porcentaje de mesas escrutadas | 5|
|Electores | 8|
|Total de votantes | 8|
|Participación sobre padrón | 5|
|Participación sobre escrutado | 5|
|Electores escrutados | 8|
|Porcentaje de electores escrutados | 5|
|Votos válidos | 8|
|Porcentaje de votos válidos | 5|
|Votos positivos | 8|
|Porcentaje de votos positivos | 5|
|Votos en blanco | 8|
|Porcentaje de votos en blanco | 5|
|Votos nulos | 8|
|Porcentaje de votos nulos | 5|
|Votos recurridos, impugnados y comando | 8|
|Porcentaje de votos recurridos e impugnados | 5|
|Día | 2|
|Hora | 2|
|Minuto | 2|

La _Longitud_ es la cantidad de caractares (letras) que el campo de la tabla podrá tener. Es un método que usaban nuestros **ancestros** para optimizar el espacio en disco de los datos.  

Tip: Una forma fácil de usar estos datos como titulos de las columnas es copiar la tabla, pegarla Google Drive, volver a copiarla y hacer el pegado especial que se llama _Pegado con transposición de datos_. Esto transforma filas en columnas.  

Debería verse así:

![totales-04-indra](../img/totales-04-indra.png)

#### Totales por listas

Esta lista si incluye los totales de cada lista junto a las agrupaciones que compiten en internas. 
A los fines de conservar todos estos datos en una sola tabla simple se agregan al final tres columnas 
todas las veces que sean necesarias para mostrar las agrupaciones internas que compiten dentro de cada lista.  

|DESCRIPCIÓN | LONGITUD|
|------------|---------|
|Código de elección | 1|
|Código de provincia | 2|
|Código de sección/ comuna | 3|
|Día | 2|
|Hora | 2|
|Minuto | 2|
|Código de la agrupación política | 4|
|Votos a la agrupación política | 8|
|Porcentaje de votos a la agrupación política | 5|
|**Tabla de 10 elementos, correspondientes a las listas propuestas por cada partido**| |
|Código de lista | 4|
|Votos al lista | 8|
|Porcentaje de votos a la lista | 5|

Haciendo el mismo proceso con el archivo de [_totales por listas_ para Córdoba](https://github.com/avdata99/datos-indra-dia-eleccion-paso-2017-AR/blob/master/recursos/DATOS-MUESTRA-2017-08-01/DATOS_89822634/totaleslistas_04.csv) se debe llegar a esta lista:  

![totales-listas-04-indra](../img/totales-listas-04-indra.png)

#### Listas participantes

Así como la de ámbitos se liberan las listas de todas las agrupaciones participantes.  
Este archivo puede usarse como dato accesorio para ponerle nombre a los datos anteriores.  

Al igual que los demas datos es necesario descargar [este archivo](https://github.com/avdata99/datos-indra-dia-eleccion-paso-2017-AR/blob/master/recursos/DATOS-MUESTRA-2017-08-01/generales_00/listas_00.csv) y pasarlo a Google Drive.  

Según la documentación, las tres columnas representan _Codigo, Siglas y Denominación_.  
Debe quedar así:

![listas](../img/listas.png)

Cómo se ve todavía el dato no está erminado y son datos de prueba.  


## Trabajo terminado abierto

Finalmente todo este proceso [esta libre y disponible en Google Drive](https://docs.google.com/spreadsheets/d/1k8fHbUGyQo5NzW46C2RD1N_Jk9QRcUJHRf8Dh_gP5Do/edit?usp=sharing).  

[>> Continuar con el análisis y la visualización de estos datos >>](analizar-datos-ministerio-interior-e-Indra.md)