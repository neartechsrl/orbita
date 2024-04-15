Funciones Javascript para fórmulas en sistema Orbita 
------

### Objeto utiles

- [floatNum](#float_num)
- [intNum](#int_num)
- [leerArchivo](#leer_archivo)
- [leerExcel](#leer_excel)
- [localRest](#local_rest)
- [localRestPaginado](#local_rest_paginado)
- [Rest](#rest)
  
<a id="float_num"></a>
#### floatNum(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |objeto string a convetir a float|

Devuelve un valor float o cero si da error la conversión
ejemplo

```javascript
	var total = utiles.floatNum("100.25");
```
#### intNum(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |objeto string a convetir a integer|

Devuelve un valor integer o cero si da error la conversión
ejemplo

```javascript
	var cantidad = utiles.intNum("2512");
```
#### leerArchivo(param)

| Parámetros     | Explicación|
| -------------- | ---------- |
| param | Parámetros para leer archivo de texto o excel |

Devuelve un array con los datos del archivo leido.

```javascript

// ejemplo leer archivo de texto
var txt_param = {
        archivo: "c:/articulos.txt", // ruta y nombre de archivo
        tipo: "txt", // txt = archivo de texto
        separador: "tab", // tab , o ;
        fila_inicial: 1,
        campos: [
            {
                titulo: "codigo",
                tipo: "texto",
                orden: 0
            },
            {
                titulo: "descripcion",
                tipo: "texto",
                orden: 1
            },
            {
                titulo: "fecha",
                tipo: "date",
                orden: 2
            },
            {
                titulo: "cantidad",
                tipo: "numero",
                orden: 3
            },
            {
                titulo: "precio",
                tipo: "numero",
                orden: 4
            }
        ]	
    }	
const txtfile = utiles.leerArchivo(txt_param);

// ejemplo leer planilla excel
var excel_param = {
	archivo: "c:/Libro1.xlsx", // ruta y nombre de planilla excel
	tipo: "xls", // xls = excel
	hojas: [
	    {
		"nombre": "Hoja1"
	    }
	],
	campos: [
	    {
		titulo: "codigo",
		tipo: "texto"
	    },
	    {
		titulo: "descripcion",
		tipo: "texto"
	    },
	    {
		titulo: "fecha",
		tipo: "date"
	    },
	    {
		titulo: "cantidad",
		tipo: "numero"
	    },
	    {
		titulo: "precio",
		tipo: "numero"
	    }
	]	
}	

const excel = utiles.leerArchivo(excel_param);

```

#### leerExcel(filename)

| Parámetros     | Explicación|
| -------------- | ---------- |
| filename | ruta y nombre archivo excel|

Devuelve un array con los datos de la planilla excel.

```javascript
const file = utiles.leerExcel("c:/planillas/ejemplo.xlsx");
```
#### logJson(objeto)

| Parámetros     | Explicación|
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

#### localRest(metodo, endpoint, objeto)

| Parámetros     | Explicación|
| -------------- | ---------- |
| metodo |GET POST PUT DELETE|
| endpoint |endpoint|
| objeto |array, objeto o null. Si es válido se transforma a json para enviar en el requerimiento|

Ejecuta un endpoint REST localmente y devuelve el resultado de la siguiente forma:
+ status code: 200 o 400
+ header: objeto con el encabezado de la respuesta con los siguientes datos: 
	* current_count: total de registros devueltos
	* per_page: total de registros por página
	* total_count: total de registros
	* total_pages: total de páginas
+ objeto: objeto, array o null que devuelve el endpoint

ejemplo

```javascript
var page = 1;
var pages = 1;
do {
	const [status_code, header, conexiones] = utiles.localRest("GET", "/coop/conexion?page=" + page, null);      
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
    "cod_conexion": "000001"
  }
]
```

#### localRestPaginado(metodo, endpoint, objeto)

| Parámetros     | Explicación|
| -------------- | ---------- |
| metodo |GET POST PUT DELETE|
| endpoint |endpoint|
| objeto |array, objeto o null. Si es válido se transforma a json para enviar en el requerimiento|

Ejecuta un endpoint REST localmente y crea un objeto que se puede recorrer página por página.
El método objeto.Next() permite iterar sobre las páginas.
Desde la propiedad objeto.json se obtiene los datos de la página activa.

ejemplo

```javascript
	var rs = new utiles.localRestPaginado("GET", "/coop/conexion?cod_conexion=000001", null);
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
"cod_conexion": "000001"
}
```

#### Rest(metodo, url, objeto, header)

| Parámetros     | Explicación|
| -------------- | ---------- |
| metodo |GET POST PUT DELETE|
| url |endpoint|
| objeto |array, objeto o null. Si es válido se transforma a json para enviar en el requerimiento|
| header |objeto o null. Se usa para pasar encabezado de requerimiento personalizado|

Ejecuta un endpoint REST remoto y devuelve el resultado de la siguiente forma:
+ status code: 200 o 400
+ header: objeto con el encabezado de la respuesta
+ objeto: objeto, array o null que devuelve el endpoint

ejemplo

```javascript
var headerReq = {
	"Accept": "application/json",
	"Content-Type": "application/json; charset=UTF-8",
	"Authorization": "Basic " + nt.EncodeBase64("supervisor" + ":" + "123456") 
}
const [status_code, headerResp, cliente] = utiles.Rest("GET", "https://api.neartech.com.ar:4567/cliente?codigo_perfil=1&nombre_base=ejemplo1&codigo_cliente=000001", null, headerReq);
```
