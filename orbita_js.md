Funciones Javascript para fórmulas en sistema Orbita 
------

### Objeto utiles

- [logJson](#log_json) 
- [floatNum](#float_num)
- [intNum](#int_num)
- [isDate](#is_date)
- [formatDate](#format_date)
- [firstDateOffWeek](#first_date_off_week)
- [firstDateOffMonth](#first_date_off_month)
- [previousWeek](#previous_week)
- [previousMonth](#previous_month)
- [currentYear](#current_year)
- [previousYear](#previous_year)
- [getExcelDate](#get_excel_date)
- [getStrExcelDate](#get_str_excel_date)
  
- [leerArchivo](#leer_archivo)
- [leerExcel](#leer_excel)
  
- [localRest](#local_rest)
- [localRestPaginado](#local_rest_paginado)
- [Rest](#rest)
  
- [leerConexionesValores](#leer_conexiones_valores)
- [leerNovedadesValores](#leer_novedades_valores)
- [grabarNovedadesValores](#grabar_novedades_valores)
- [leerConexion](#leer_conexion)
  
<a id="log_json"></a>
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

<a id="float_num"></a>
#### floatNum(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |objeto string a convetir a float|

Devuelve un valor float o cero si da error la conversión

```javascript
	var total = utiles.floatNum("100.25");
```

<a id="int_num"></a>
#### intNum(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |objeto string a convetir a integer|

Devuelve un valor integer o cero si da error la conversión

```javascript
	var cantidad = utiles.intNum("2512");
```
<a id="is_date"></a>
#### isDate(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |fecha a validar|

Devuelve true/false según si el valor es una fecha válida o no.

```javascript
	var fecha_valida = utiles.isDate("01/01/2024");
```

<a id="format_date"></a>
#### formatDate(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |fecha|

Devuelve un string con la fecha en formato dd/mm/yyyy

```javascript
	var strFecha = utiles.formatDate(new Date(2024,0,12));
	console.log(strFecha);
```
```
	> 12/01/2024
```

<a id="first_date_off_week"></a>
#### firstDateOffWeek()

Devuelve la fecha del primer día de la semana. Si la fecha actual es 03/05/2024, la función devolvería 28/04/2024

<a id="first_date_off_month"></a>
#### firstDateOffMonth()

Devuelve la fecha del primer día del mes. Si la fecha actual es 03/05/2024, la función devolvería 01/05/2024

<a id="previous_week"></a>
#### previousWeek()

Devuelve dos fechas, una con el valor del primer día de la semana anterior y otra con el último día de la semana anterior, siempre tomando como referencia la fecha actual.

<a id="previous_month"></a>
#### previousMonth()

Devuelve dos fechas, una con el valor del primer día del mes anterior y otra con el último día del mes anterior, siempre tomando como referencia la fecha actual.

<a id="current_year"></a>
#### currentYear()

Devuelve dos fechas, una con el valor del primer día del año actual y otra con la fecha actual, siempre tomando como referencia la fecha actual.

<a id="previous_year"></a>
#### previousYear()

Devuelve dos fechas, una con el valor del primer día del año anterior y otra con el último día del año anterior, siempre tomando como referencia la fecha actual.

<a id="getExcel_date"></a>
#### getExcelDate(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |valor numérico a transformar en fecha|

Devuelve un objeto date según el valor numérico pasado como parámetro o el valor 1899-12-30 si da error la conversión.

```javascript
	var fecha = utiles.getExcelDate(45316);
	console.log(fecha);
```
```
	> 2024-01-25
```

<a id="get_str_excel_date"></a>
#### getStrExcelDate(valor)

| Parámetros     | Explicación|
| -------------- | ---------- |
| valor |valor numérico a transformar en fecha|

Devuelve una objeto string con formato dd/mm/yyyy según el valor numérico pasado como parámetro o el valor 30/12/1899 si da error la conversión.

```javascript
	var fecha = utiles.getExcelDate(45316);
	console.log(fecha);
```
```
	> 25/01/2024
```

<a id="leer_archivo"></a>
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

<a id="leer_excel"></a>
#### leerExcel(filename)

| Parámetros     | Explicación|
| -------------- | ---------- |
| filename | ruta y nombre archivo excel|

Devuelve un array con los datos de la planilla excel.

```javascript
const file = utiles.leerExcel("c:/planillas/ejemplo.xlsx");
```

<a id="local_rest"></a>
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

<a id="local_rest_paginado"></a>
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

<a id="rest"></a>
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

<a id="leer_conexiones_valores"></a>
#### leerConexionesValores(cod_conexion, cod_propiedad, periodo)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_conexion | código conexión |
| cod_propiedad | código novedad |
| periodo | periodo |

Devuelve un array con los datos de valores de propiedades del periodo consultado de una conexión específica.

<a id="leer_novedades_valores"></a>
#### leerNovedadesValores(cod_novedad, cod_conexion, periodo)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_novedad | código novedad |
| cod_conexion | código conexión |
| periodo | periodo |

Devuelve un array con los datos de valores de novedades PENDIENTES del periodo consultado de una conexión específica.

<a id="grabar_novedades_valores"></a>
#### grabarNovedadesValores(json)

| Parámetros     | Explicación|
| -------------- | ---------- |
| json | json con la novedad a grabar |

Graba un valor de novedad en el periodo actual.

```javascript
// buscar datos conexion
const conexion = utiles.leerConexion("CN01");
if ( conexion !== null )  {
	const json = {
	    cod_novedad: param.novedad.cod_novedad,
	    cod_conexion: conexion.cod_conexion,
	    periodo: param.periodo,
            estado: "PENDIENTE",
	    valores: {
		importe: 1250.25
	    }            
	}
	
	// Grabar novedad.
	const [error, result] = utiles.grabarNovedadesValores(json);
	if ( !result ) {          
	  console.log("error al grabar novedad");
	  console.log(error.error);
	  utiles.logJson( json );
	  console.log("----------------");
	}
}
```
<a id="leer_conexion"></a>
#### leerConexion(cod_conexion)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_conexion | código conexión a consultar |

Devuelve un objeto json con los datos de la conexión a consultar.

	

