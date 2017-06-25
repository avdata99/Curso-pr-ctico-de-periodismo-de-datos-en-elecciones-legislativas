# Procesar CSV para Geolocalizar

Tomar el CSV con los datos de la carta marina y subirla a Google Drive.  
Una vez subido darle click derecho y elegir _abrir con_ y luego _hoja de calculo de Google_.  
Esto nos dará un entorno de planilla de cálculo online para mejorar los datos antes de su geolocalización.  

![csv en drive](../img/csv-en-gdrive.png)

Nótese que una de las columnas incluye más de un dato. Esto es un _error_ (a veces intencional) que dificulta el análisis de datos. La columna _Escuela_ incluye:
 - El nombre de la escuela
 - La dirección de la escuela
 - En algunos casos el barrio

Google Sheets (la planilla de cálculo incluida en Google Drive) incluye una herramienta para estos casos. Excel y LibreOffice seguramente ofrecen posibilidades similares en este sentido.  

Como vamos a procesar la columna _Escuela_ no es mala idea hacer una copia de ella para referencias futuras en la medición de la efectividad de los procesos que vamos a usar.  

Hacemos un click en el encabezado de la columna _Escuela_ para seleccionarla. En el menú _datos_ elegimos la opción _dividir texto en columnas_. No ofrecera como _separador_ predeterminado a la coma

![dividir](../img/dividir-texto-en-col.png)

Nosotros cambiamos eso por un separador _personalizado_. Elegimos _" - "_ (espacio, guión, espacio) que es el que detectamos. Se requiere una segunda división sobre la segunda columna resultante ya que algunas dirección es incluyen el barrio (que no ayuda a la geolocalización) con el prefijo _"B°"_. Dividir entonces esta columna con el separador _"B°"_.  

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

[Ver Mapa](https://fusiontables.google.com/embedviz?q=select+col11+from+1Se7MLXEFxIPOExxpSfEUNoMmY2p3Kh-AV3jWQS-e&viz=MAP&h=false&lat=-32.730273776177484&lng=-61.927968202880834&t=1&z=6&l=col11&y=3&tmplt=5&hml=GEOCODABLE)

```
<iframe width="500" height="300" 
    scrolling="no" 
    frameborder="no" 
    src="https://fusiontables.google.com/embedviz?q=select+col11+from+1Se7MLXEFxIPOExxpSfEUNoMmY2p3Kh-AV3jWQS-e&amp;viz=MAP&amp;h=false&amp;lat=-32.730273776177484&amp;lng=-61.927968202880834&amp;t=1&amp;z=6&amp;l=col11&amp;y=3&amp;tmplt=5&amp;hml=GEOCODABLE">
</iframe>
```

**Problemas**: 
 - No permite exportar coordenadas
 - Muy limitada capacidad de estilos según datos.
 - La herramienta no recibe actualizaciones hace mucho. Pareciera que Google no la va a continuar

![mapa-fusion](../img/mapa-fusion-tables.png)

### Geolocalziando con Google MyMaps

[Ver mapa](https://www.google.com/maps/d/view?mid=1zKL3m91IkHFJBXvDcE1kaVQJvfo&ll=-31.861778787428463%2C-63.61520641928098&z=7)

**Pros**: 
 - Permite iconos variados según algún campo.
 - La edición colaborativa es muy interesante.

**Problemas**: 
 - No permite exportar coordenadas.

![mymaps](../img/mapa-google-mymaps.png)

### Geolocalizando en CartoDB

[Ver mapa](https://hudson.carto.com/builder/170fae5b-d302-4482-aa4d-13b67df9209b/embed)
Código para embeber
```
<iframe 
    width="100%" 
    height="520" 
    frameborder="0" 
    src="https://hudson.carto.com/builder/170fae5b-d302-4482-aa4d-13b67df9209b/embed" 
    allowfullscreen 
    webkitallowfullscreen 
    mozallowfullscreen 
    oallowfullscreen 
    msallowfullscreen>
</iframe>
```
**Pros**:
 - Amplias posibilidades para dar estilos a los puntos segun variables.
 - Si ya se cuentan con las coordenadas es muy potente.

**Problemas**: 
 - Servicio de Geolocalización pago. Límite gratuito muy bajo


![mymaps](../img/mapa-carto.png)

## Geolocalizar direcciones es complejo

Para desarrolladores se recomiendan usar los webservices de Google u OpenStreetMaps con scripts que gradualmente releven los datos necesarios.  

Una solución intermedia es usar un script (similar a los macros de Excel) en Google Sheets que permite hasta 1000 Geolocalizaciones por día. Es muy útil y funciona. Más info [aquí](https://www.datavizforall.org/transform/geocode/).  

Básicamente este metodo es un pequeño programa que se puede anexar a cualquier planilla de Google Drive. Para agregarlo desde el menú _Herramientas_ se elige _Editor de secuencia de comandos_ y se copia el siguiente texto.  

```
var ui = SpreadsheetApp.getUi();
var addressColumn = 1;
var latColumn = 2;
var lngColumn = 3;
var foundAddressColumn = 4;
var qualityColumn = 5;
var sourceColumn = 6;

googleGeocoder = Maps.newGeocoder().setRegion(
  PropertiesService.getDocumentProperties().getProperty('GEOCODING_REGION') || 'ar'
);

function geocode(source) {
  var sheet = SpreadsheetApp.getActiveSheet();
  var cells = sheet.getActiveRange();

  if (cells.getNumColumns() != 6) {
    ui.alert(
      'Warning',
      'You must select 6 columns: Location, Latitude, Longitude, Found, Quality, Source',
      ui.ButtonSet.OK
    );
    return;
  }

  var nAll = 0;
  var nIgnores = 0;
  var nFailure = 0;
  var quality;
  var printComplete = true;

  for (addressRow = 1; addressRow <= cells.getNumRows(); addressRow++) {
    var address = cells.getCell(addressRow, addressColumn).getValue();

    if (!address)
        {continue}
    
    // ignorar los que ya están
    var lat = cells.getCell(addressRow, latColumn).getValue();
    if (lat !== '') {
          nIgnores++;
          continue;}
    
    nAll++;
    
    if (source == 'US Census') {
      nFailure += withUSCensus(cells, addressRow, address);
    } else {
      nFailure += withGoogle(cells, addressRow, address);
      Utilities.sleep(1100);
    }
  }

  if (printComplete) {
    ui.alert('Completado!', 'Geocodificados: ' + (nAll - nFailure)
    + '\nFallados: ' + nFailure + ' Ignorados: ' + nIgnores, ui.ButtonSet.OK);
  }

}

/**
 * Geocode address with Google Apps https://developers.google.com/apps-script/reference/maps/geocoder
 */
function withGoogle(cells, row, address) {
  Logger.log('Geolocalizando %s', address);
  try {
      location = googleGeocoder.geocode(address);
      } 
  catch (e) {
    msg = e.message;
    Logger.log('Error Google %s', msg);
    location = {'status': 'SCRIPT ERROR'};
  }
  
  if (location.status == 'SCRIPT ERROR') {
    insertDataIntoSheet(cells, row, [
      [foundAddressColumn, ''], [latColumn, ''], [lngColumn, ''], [qualityColumn, 'FAILED SCRIPT: ' + msg], [sourceColumn, 'Google']
    ]);

    return 1;
  }
  
  if (location.status !== 'OK') {
    insertDataIntoSheet(cells, row, [
      [foundAddressColumn, ''], [latColumn, ''], [lngColumn, ''], [qualityColumn, 'No Match'], [sourceColumn, 'Google']
    ]);

    return 1;
  }

  lat = location['results'][0]['geometry']['location']['lat'];
  lng = location['results'][0]['geometry']['location']['lng'];
  foundAddress = location['results'][0]['formatted_address'];

  var quality;
  if (location['results'][0]['partial_match']) {
    quality = 'Partial Match';
  } else {
    quality = 'Match';
  }

  insertDataIntoSheet(cells, row, [
    [foundAddressColumn, foundAddress],
    [latColumn, lat],
    [lngColumn, lng],
    [qualityColumn, quality],
    [sourceColumn, 'Google']
  ]);

  return 0;
}

/**
 * Geocoding with US Census Geocoder https://geocoding.geo.census.gov/geocoder/
 */
function withUSCensus(cells, row, address) {
  var url = 'https://geocoding.geo.census.gov/'
          + 'geocoder/locations/onelineaddress?address='
          + encodeURIComponent(address)
          + '&benchmark=Public_AR_Current&format=json';

  var response = JSON.parse(UrlFetchApp.fetch(url));
  var matches = (response.result.addressMatches.length > 0) ? 'Match' : 'No Match';

  if (matches !== 'Match') {
    insertDataIntoSheet(cells, row, [
      [foundAddressColumn, ''],
      [latColumn, ''],
      [lngColumn, ''],
      [qualityColumn, 'No Match'],
      [sourceColumn, 'US Census']
    ]);
    return 1;
  }

  var z = response.result.addressMatches[0];

  var quality;
  if (address.toLowerCase().replace(/[,\']/g, '') ==
      z.matchedAddress.toLowerCase().replace(/[,\']/g, '')) {
        quality = 'Exact';
  } else {
    quality = 'Match';
  }

  insertDataIntoSheet(cells, row, [
    [foundAddressColumn, z.matchedAddress],
    [latColumn, z.coordinates.y],
    [lngColumn, z.coordinates.x],
    [qualityColumn, quality],
    [sourceColumn, 'US Census']
  ]);

  return 0;
}


/**
 * Sets cells from a 'row' to values in data
 */
function insertDataIntoSheet(cells, row, data) {
  for (d in data) {
    cells.getCell(row, data[d][0]).setValue(data[d][1]);
  }
}

function censusAddressToPosition() {
  geocode('US Census');
}

function googleAddressToPosition() {
  geocode('Google');
}

function onOpen() {
  ui.createMenu('Geocoder')
   .addItem('with US Census', 'censusAddressToPosition')
   .addItem('with Google (limit 1000 per day)', 'googleAddressToPosition')
   .addToUi();
}
```

Se requieren cinco columnas en blanco a la derecha de la que incluye nuestro campo completo (direccion, ciudad, provincia, país) ya que este programa completara además de la latitud y la longitud algunos datos útiles más.  

![gf](/img/google-sheets-geocoder-census-geographies.gif)

Finalemente de esta forma se pudieron obtener la gran mayoría de las geolocalizaciones.  
[CSV FINAL CON COORDENADAS](../recursos/escuelas-elecciones-2015-cordoba-FINAL-CON-GEO.csv)

Ahora Carto no requiere interferir la geolocalizazión y detecta a la primera las columnas que representan las coordenadas.  

[Ver mapa](https://hudson.carto.com/builder/9f30c071-f758-4286-b408-8f8fa2db5c10/embed).  

Embeber: 
```
<iframe width="100%" height="520" 
    frameborder="0" 
    src="https://hudson.carto.com/builder/9f30c071-f758-4286-b408-8f8fa2db5c10/embed" 
    allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen>
</iframe>
```

![Mapa OK carto](../img/carto2-ok.png)

Usar este CSV con latitud y longitud incluida agiliza la carga en Google MyMaps o cualquier otra plataforma ya que no será necesario mapear las direcciones.  
