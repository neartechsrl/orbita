Funciones Javascript para f�rmulas en sistema Orbita 
------

### Objeto utiles

####floatNum(valor)

| Par�metros     | Explicaci�n|
| -------------- | ---------- |
| valor |objeto string a convetir a float|

Devuelve un valor float o cero si da error la conversi�n
ejemplo

```javascript
	var total = utiles.floatNum("100.25");
```
####intNum(valor)

| Par�metros     | Explicaci�n|
| -------------- | ---------- |
| valor |objeto string a convetir a integer|

Devuelve un valor integer o cero si da error la conversi�n
ejemplo

```javascript
	var cantidad = utiles.intNum("2512");
```

####leerExcel(filename)

| Par�metros     | Explicaci�n|
| -------------- | ---------- |
| filename | ruta y nombre archivo excel|

Devuelve un array con los datos de la planilla excel.

```javascript
const file = utiles.leerExcel("c:/planillas/ejemplo.xlsx");
```
####logJson(objeto)

| Par�metros     | Explicaci�n|
| -------------- | ---------- |
| objeto |objeto a mostrar en consola|

Muestra por consola un objeto en formato json.

```javascript
const datos = {
	codigo: "001",
	importe: 125.22
} ;

utiles.logJson(datos);
```
resultado

```json
{
  "codigo": "001",
  "importe": 125.22
}

```

####localRest(metodo, endpoint, json)

| Par�metros     | Explicaci�n|
| -------------- | ---------- |
| metodo |GET POST PUT DELETE|
| endpoint |endpoint|
| sjson |string json o string vacio|

Ejecuta un endpoint REST localmente y devuelve el resultado de la siguiente forma:
+ status code: 200 o 400
+ header: objeto con el encabezado del requerimiento con los siguientes datos: 
	* current_count:	total de registros devueltos
	* per_page: total de registros por p�gina
	* total_count: total de registros
	* total_pages: total de p�ginas
+ objeto: objeto array que devuelve el endpoint o vacio

ejemplo

```javascript
var page = 1;
var pages = 1;
do {
	const [status_code, header, conexiones] = utiles.localRest("GET", "/coop/conexion?page=" + page, "");      
	if ( status_code === 200 ) {
		pages = header.total_pages;
		utiles.logJson(conexiones);        
	}    
	page ++;
} while (page < pages);
```
resultado en consola

```json
[
  {
    "cod_calculo": 0,
    "cod_cliente": "000001",
    "cod_conexion": "000001",
	...
  },
  ...
]
```

####localRestPaginado(metodo, endpoint, json)

| Par�metros     | Explicaci�n|
| -------------- | ---------- |
| metodo |GET POST PUT DELETE|
| endpoint |endpoint|
| sjson |string json o string vacio|

Ejecuta un endpoint REST localmente y crea un objeto que se puede recorrer p�gina por p�gina.
El m�todo objeto.Next() permite iterar sobre las p�ginas.
Desde la propiedad objeto.json se obtiene los datos de la p�gina activa.

ejemplo

```javascript
	var rs = new utiles.localRestPaginado("GET", "/coop/conexion?cod_conexion=000001", "");
	while ( rs.Next() ) {
		rs.json.forEach( function(cn) {
			utiles.logJson(cn);
		});      
	}
```
resultado en consola

```json
{
"cod_calculo": 0,
"cod_cliente": "000001",
"cod_conexion": "000001",
...
}
```
