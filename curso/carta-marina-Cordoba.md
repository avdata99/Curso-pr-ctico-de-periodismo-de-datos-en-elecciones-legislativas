# Procesar la carta marina Córdoba 2015

Una búsqueda en Google dará con una lista de [recursos en el poder judicial de Nación sobre CORDOBA](https://www.pjn.gov.ar/cne/secelec/secciones/otros/otros_view.php?oID=674&dID=4). Hay de todas las provincias. Lamentablemente son PDF difíciles de procesar en general.  

Allí se encuentra la [Carta Marina Córdoba 2015 en PDF](https://www.pjn.gov.ar/cne/secelec/document/otros/4-Carta%20Marina%202015.pdf).  

![carta](../img/carta-marina-pdf.png)

Una vez descargado se debe abrir con [Tabula PDF](http://tabula.technology/). Este producto libre y gratuito permite pasar tablas a formatos reutilizables de datos.  

Como primera prueba se puede seleccionar directamente una parte de la tabla para analizar si el resultado es útil.  

![marca](../img/marcando-zona-en-tabula.png)

Luego del proceso se ven los resultados pasados a una tabla de datos reutilizable.  

![intento](../img/primer-intento-tabula.png)

Se ve que no tomo correctamente las celdas como uno podría imaginar. Se requiere que cada dato esté en una celda individual. El que construyo el PDF no lo hizo construyendo una tabla ordenadamente sino que dejo varias líneas de texto en cada _celda_.  

Como prueba adicional se puede usar el detector automático de tablas en Tabula y ver los resultados. 

![prueba2](../img/prueba-2-tabula.png)

Como se ve, la falla es la misma. Este PDF no es procesable de manera que entregue resultados listos para usar. En estos casos las opciones son:
 - Si el PDF no es muy largo, tomar estos resultados (o copiar el texto del PDF y pasarlos a un editor de texto) para limpiarlos a mano.
 - Si el PDF es muy largo se puede contar con la colaboración de algún programador que procese automáticamente y obtenga el resultado deseado.
 - Si se tienen contactos con algún partido político o con funcionarios de la junta electoral se puede solicitar una versión más amigable (Excel, CSV o similares).

En 2015 este problema ya se presentó con esta carta marina, [se resolvió](https://github.com/OpenDataCordoba/elecciones2015/tree/master/resources/carta-marina) y se dejaron liberados los resultados:

[Carta Marina 2015 CSV](https://github.com/OpenDataCordoba/elecciones2015/blob/master/resources/carta-marina/escuelas-elecciones-2015-cordoba.csv?raw=true) ([copia local](../recursos/escuelas-elecciones-2015-cordoba.csv)).  

[PROCESAR ESTE CSV >>](geolocalizar-csv.md) 