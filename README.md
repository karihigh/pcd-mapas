# 🌏 Taller Guiado de Mapas Web (PCD Quito 2023)



### 1. Configuración básica

1.1. Incluir el archivo CSS de Leaflet en el head del proyecto en **index.html**
   ```
   <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.3/dist/leaflet.css" integrity="sha256-kLaT2GOSpHechhsozzB+flnD+zUyjE2LlfWPgU04xyI=" crossorigin=""/>
   ```

1.2. Incluir el archivo JavaScript de Leaflet después del CSS
   ```
   <script src="https://unpkg.com/leaflet@1.9.3/dist/leaflet.js" integrity="sha256-WBkoXOwTeyKclOHuWtc+i2uENFpDZ9YPdf5Hf+D7ewM=" crossorigin=""></script>
   ```

1.3. Agregar un elemento div con un id donde quieres que aparezca el mapa
   ```
   <div id="mapa"></div>
   ```

1.4. Fijar una altura al contenedor del mapa en **style.css**:
   ```javascript
   #mapa { height: 360px; }
   ```

### 2. Inicializar el mapa

2.1. Crear la variable "mimapa" y ejecutar la función de Leaflet. También definiremos el punto donde nuestro mapa empezará y el zoom.
  ```javascript
  let mimapa = L.map('mapa').setView([-33.45, -70.59], 14);
  ```

2.2. Agregar la base del mapa (tiles)

  - Opción A (OpenStreetMap)
  ```javascript
  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
  }).addTo(map);
  ```

  - Opción B (Mapbox) *(requiere un [token de acceso](https://docs.mapbox.com/help/getting-started/access-tokens/))*
  ```javascript
  L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', {
        attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
        maxZoom: 18,
        id: 'mapbox/outdoors-v11',
        tileSize: 512,
        zoomOffset: -1,
        accessToken: mitoken
    }).addTo(mimapa);
  ```
  
2.3. En caso de ocupar Mapbox, agregar el token al inicio del código. Pueden encontrarlos en el archivo **token.js**
  ```javascript
  let mitoken = 
  ```

2.4. Agregaremos un marcador
  ```javascript
  L.marker([-33.456, -70.594]).addTo(mimapa);
  ```

### 3. Puntos desde una base de datos

3.1. Agregar datos al inicio de **script.js** en el siguiente formato:
  ```javascript
  let midata = [
    {
      "Lugar": "Quito",
      "Latitude": 51.5073509,
      "Longitude": -0.1277583,
      "Fecha": 2023
    },
  ]
  ```

3.2. Crear función para cada dato en mi listado:
  ```javascript
  for (let i = 0; i < midata.length; i++) {
    let milugar = midata[i];
    L.marker([milugar.Latitude, milugar.Longitude]).addTo(mimapa);
  }
  ```

### 4. Cambiar el marcador

4.1. Para estilar el marcador deberás crear una variable antes del loop
  ```javascript
  let flagIcon = L.icon({
    iconUrl: 'https://cdn.glitch.com/25b5e6be-ae39-454a-8198-ac955e888873%2F1F6A9_color.png?v=1591912202480',
    iconSize: [20, 20],
    iconAnchor: [10, 10],
    popupAnchor: [0, 0],
  })
  ```

4.2. Luego, agregarla en la función **L.marker**
  ```javascript
  L.marker([milugar.Latitude, milugar.Longitude], {icon: flagIcon}).addTo(mimapa);
  ```
  
### 5. Agregar un popup

5.1. Crear la variable de popup **dentro del loop**. En este caso agregamos la columna de Lugar como título y la columna de Fecha.
  ```javascript
  let popupText = "<h3>" + milugar.Place + "</h3>" + " " + milugar.Fecha;
  ```

5.2. Finalmente, agregamos el popup dentro de la función **L.marker**
  ```javascript
  L.marker([milugar.Latitude, milugar.Longitude], {icon: flagIcon}).bindPopup(popupText).addTo(mimapa);
  ```
