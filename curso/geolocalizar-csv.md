# Procesar CSV para Geolocalizar

Tomar el CSV con los datos de la carta marina y subirlo a Google Drive.  
Una vez subido darle click derecho y elegir _abrir con_ y luego _hoja de calculo de Google_.  
Esto nos dará un entorno de planilla de cálculo online para mejorar los datos antes de su geolocalización.  

![csv en drive](../img/csv-en-gdrive.png)

Nótese que una de las columnas incluye más de un dato. Esto es un _error_ (a veces intencional) que dificulta el análisis de datos. La columna _Escuela_ incluye:
 - El nombre de la escuela
 - La dirección de la escuela
 - En algunos casos el barrio

Google Sheets (la planilla de cálculo incluida en Google Drive) incluye una herramienta para estos casos. Excel y LibreOffice seguramente ofrecen posibilidades similares en este sentido.  

Como vamos a procesar la columna _Escuela_ no es mala idea hacer una copia de ella para referencias futuras en la medición de la efectividad de los procesos que vamos a usar.  

Hacemos un click en el encabezado de la columna _Escuela_ para seleccionarla. En el menú _datos_ elegimos la opción _dividir texto en columnas_. Nos ofrecera como _separador_ predeterminado la coma.  

![dividir](../img/dividir-texto-en-col.png)

Nosotros cambiamos eso por un separador _personalizado_. Elegimos _" - "_ (espacio, guión, espacio) que es el que detectamosmirando los datos.  
Se requiere una segunda división sobre la segunda columna resultante ya que algunas dirección es incluyen el barrio (que no ayuda a la geolocalización) con el prefijo _"B°"_. Dividir entonces esta columna con el separador _"B°"_.  

Agregar en la planilla de cálculo las columnas _Ciudad_, _Provincia_ y _Pais_. A la columna Ciudad igualarla a la columna. Si bien no siembre es necesario la provincia y el país ya que algunos entornos de geolocalización permiten especificarlo nunca está de más.   

La columna Ciudad no es tan facil de llenar. En Córdoba los circuitos son localidades. Esto ayuda en general salvo en la Ciudad de Córdoba donde los circuitos electorales tienen nombres no formales vinculados algunas veces al barrio pero no son útiles a los fines de la geolocalización. Todas las escuelas de la seccional 1, sin importar el circuito, deben decir Córdoba en la columna _Ciudad_.  

En definitiva, la columna _Ciudad_ debe decir _Córdoba_ para la seccional _"Capital"_ y copiar la columna _Circuito Nombre_ en todos los otros casos. Esto se puede programar con la función: 

```
=if(B2="CAPITAL";"Córdoba";D2)
```

Para algunos sitios se requiere que todos los datos de una geolocalización este en una sola columna. Es util por esto agregar una columna más al final que se llame por ejemplo _Geo_ y que agrupe a las columnas
 - Direccion
 - Ciudad
 - Provincia
 - País
 - NO USAR BARRIO salvo que algún entorno mejore la geolocalización, en general esto no sucede

```
=CONCATENATE(F2;", ";H2;", ";I2;", ";J2)
```

Descargar esta planilla como CSV para llevar a alguna plataforma de geolozalización.  
Descargar trabajo hecho: [csv para geo](../recursos/escuelas-elecciones-2015-cordoba-FINAL-PARA-GEO.csv).  

### Geolocalizando en Fusion Tables

Desde Google Drive se puede crear un nuevo archivo de Fusion Tables. Este requiere que se suba un archivo CSV o directamente se le indique una planilla de Google (la que hicimos recién sirve).  

Una vez subido se debe dar click derecho a la columna con todos los datos geográficos juntos (la última que se hizo con la función _contatenate_) para indicar que es de tipo geográfico.  
Esto hará que se pinte de amarillo y nos permitirá inciar la geolocalización (para estas 1186 escuelas demorará una o más horas).  

Finalmente queda así: [Ver Mapa](https://fusiontables.google.com/embedviz?q=select+col11+from+1Se7MLXEFxIPOExxpSfEUNoMmY2p3Kh-AV3jWQS-e&viz=MAP&h=false&lat=-32.730273776177484&lng=-61.927968202880834&t=1&z=6&l=col11&y=3&tmplt=5&hml=GEOCODABLE).  

![mapa-fusion](../img/mapa-fusion-tables.png)

**Pros**:
 - Permite otros tipos de gráficos además de mapas.
 - Estos gráficos se pueden publicar y embeber en ortos sitios. 
  
**Problemas**: 
 - No permite exportar coordenadas
 - Muy limitada capacidad de estilos según datos.
 - La herramienta no recibe actualizaciones hace mucho. Pareciera que Google no la va a continuar


### Geolocalizando con Google MyMaps

Con [Google MyMpas](https://www.google.com/maps/d/) se pueden construir mapas gráficamente superadores de Fusion Tables y el entorno colaborativo es interesante (con la misma lógica que Google Drive).  

Al igual que Fusion Tables permite GeoLocalización automática sin coordenadas y solo con datos de texto. La geolocalización aparenta ser bastante más rápida que Fusion Tables.  

Finalmente queda así: [Ver mapa](https://www.google.com/maps/d/view?mid=1zKL3m91IkHFJBXvDcE1kaVQJvfo&ll=-31.861778787428463%2C-63.61520641928098&z=7)

![mymaps](../img/mapa-google-mymaps.png)

**Pros**: 
 - Permite iconos variados según algún campo.
 - La edición colaborativa es muy interesante.
 - Permite múltiples capas de datos geográficos.

**Problemas**: 
 - No permite exportar coordenadas.


### Geolocalizando en CartoDB

Carto genera mapas más elegantes y mucho más potentes a la hora de dar estilos por valores de la tabla. Permite trabajar gratis con un límite interesante en general pero mas limitado en lo que hace a la geo referenciación. Teniendo las coordenadas (que aún no tenemos ni pudimos exportar de las otras dos plataformas) es la solución mas completa para visualizar mapas de este tipo.  

Finalmente queda así: [Ver mapa](https://hudson.carto.com/builder/170fae5b-d302-4482-aa4d-13b67df9209b/embed)

![mymaps](../img/mapa-carto.png)

**Pros**:
 - Amplias posibilidades para dar estilos a los puntos segun variables.
 - Si ya se cuentan con las coordenadas es muy potente.
 - Permite múltiples capas de datos geográficos.

**Problemas**: 
 - Servicio de Geolocalización pago. Límite gratuito muy bajo.
 - La geolocalización funcionó muy pobremente a pesar del límite. 


## Geolocalizar con el API de Google sin ser desarrollador

Una solución intermedia es usar un script (similar a los macros de Excel) en Google Sheets que permite hasta 1000 Geolocalizaciones por día. Es muy útil y funciona. Más info [aquí](https://www.datavizforall.org/transform/geocode/).  
Este metodo es un pequeño programa que se puede anexar a cualquier planilla de Google Drive. Para agregarlo desde el menú _Herramientas_ se elige _Editor de secuencia de comandos_ y se copia el texto del archivo geolocalizar.gs del [repositorio de la Municipalidad de Córdoba](https://github.com/ModernizacionMuniCBA/muni-google-util-app-scripts/tree/master/geolocalizar%20desde%20direccion).  


Una vez grabado, aparecerá un nuevo menú en la planilla de Google.  
Para usar este nuevo menú se requieren cinco columnas en blanco a la derecha de la que incluye nuestro campo completo (direccion, ciudad, provincia, país) ya que este programa completará en esos espacios. Además de la latitud y la longitud se entregan algunos datos útiles más.  

![gf](/img/google-sheets-geocoder-census-geographies.gif)

La efectividad no es excelente pero es bastante buena. En los casos en que no es exacta, lo indica. De esta forma se puede entonces pasar por un proceso manual solo para los casos en los que sea necesario.  
Finalemente de esta forma se pudieron obtener la gran mayoría de las geolocalizaciones.  
[CSV FINAL CON COORDENADAS](../recursos/escuelas-elecciones-2015-cordoba-FINAL-CON-GEO.csv)

Ahora Carto no requiere interferir la geolocalizazión y detecta a la primera las columnas que representan las coordenadas. Con este archivo se puede hacer un nuevo mapa en Carto con resultados muy superiores.  

Finalmente queda así: [Ver mapa](https://hudson.carto.com/builder/9f30c071-f758-4286-b408-8f8fa2db5c10/embed).  

![Mapa OK carto](../img/carto2-ok.png)

Código para embeber:  
```
<iframe width="100%" height="520" 
    frameborder="0" 
    src="https://hudson.carto.com/builder/9f30c071-f758-4286-b408-8f8fa2db5c10/embed" 
    allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen>
</iframe>
```

Usar este CSV con latitud y longitud incluida agiliza la carga en Google MyMaps o cualquier otra plataforma ya que no será necesario mapear las direcciones.  


## Geolocalizar direcciones es complejo

Para desarrolladores se recomiendan usar los webservices de Google u OpenStreetMaps con scripts que gradualmente releven los datos necesarios.  

Otra posibilidad con mayor complejidad técnica es seguir los pasos que describió Manuel Aristarán para cruzar estos datos con la base de datos de escuelas argentinas realizada en 2013. Con esta aplicación [Donde voto?](https://github.com/jazzido/dondevoto) es posible hacer match entre los nombres de las escuelas oficiales y los establecimientos de una carta marina. Si bien no es perfecto supera la efectividad en la geolocalización de otros métodos.  
En base a este trabajo quedo disponible tambien [otro resumen de datos de las escuelas argentinas](https://github.com/avdata99/escuelas-argentinas) con mapas incluidos.  

Un caso muy interesante con un producto similar al buscado aquí se hizo para las elecciones 2013 en Córdoba. Se realizó durante un hackathon y se llamó Democracia con códigos.    
Se puede ver el código abierto [aquí](http://democraciaconcodigos.github.io/) y el mapa [aquí](http://democraciaconcodigos.github.io/election-2013/).  

![democracia-con-codigos](../img/democracia-con-codigos.png)  