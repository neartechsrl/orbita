Objeto orbita para usar en fórmulas en javascript en sistema Orbita 
------

### Objeto orbita

- [param](#param)
- [leerConexion](#leer_conexion)
- [leerPropiedadConexion](#leer_propiedad_conexion)
- [grabarPropiedadConexion](#grabar_propiedad_conexion)
- [leerNovedadValor](#leer_novedad_valor)
- [grabarNovedadValor](#grabar_novedad_valor)
- [leerTareaValor](#leer_tarea_valor)   
- [grabarTareaValor](#grabar_tarea_valores)

<a id="param"></a>
#### param

Este objeto se usa para pasar datos a la fórmula de javascript con información relacionada al proceso que se está ejecutando.

```javascript
utiles.logJson(orbita.param);
```

Por ejemplo en la ejecución de una tarea el objeto param contiene:

- Código de tarea
- Periodo activo
- Parámetros definidos en la tarea

```json
{
  "periodo": "20240501",
  "cod_tarea": "AVISO",
  "parametro_1": "valor_1"
}
```

Por ejemplo en la ejecución de lote el objeto param contiene:

- Código de lote
- Periodo activo
- Periodo actual
- Código concepto
- Variables definidas en concepto
- Código conexión que se está calculando
- Array con propiedades de la conexión
- Array con los conceptos previos calculados.	

```json
{
  "periodo": "20240501",
  "periodo_actual": "20240501",
  "cod_lote": 1,
  "cod_concepto": "CargoFijo",
  "cod_conexion": "C0001",
  "propiedades": [
    {
      ...
    }
  ],
  "conceptos":[
    {
      ...
    }
  ]
}
```

<a id="leer_conexion"></a>
#### leerConexion(cod_conexion)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_conexion | código conexión a consultar |

Devuelve un objeto json con los datos de la conexión a consultar.

```javascript
    const conexion = orbita.leerConexion(reg.cod_conexion);
    utiles.logJson(conexion);
```

<a id="leer_propiedad_conexion"></a>
#### leerPropiedadConexion(cod_conexion, cod_propiedad, periodo)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_conexion | código conexión a consultar |
| cod_propiedad | código novedad |
| periodo | periodo. Opcional. |

Devuelve un objeto con los datos de valores de la propiedad de la conexión. Sino se indica periodo, se toma el periodo activo.

```javascript
    const propiedad = orbita.leerPropiedadConexion(orbita.param.cod_conexion, "JUBILADO");
    utiles.logJson(propiedad);
```

<a id="leer_tarea_valor"></a>
#### leerTareaValor(cod_tarea, periodo)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_tarea | código tarea |
| periodo | periodo. Opcional. |

Devuelve un objeto con los datos de valores de tarea. Sino se indica periodo, se toma el periodo activo.

```javascript
    // Leer cuadro tarifario.
    const cuadro = orbita.leerTareaValor("CUADRO_TARIFARIO");
    utiles.logJson(cuadro);
```

<a id="grabar_tarea_valor"></a>
#### grabarTareaValor(json)

| Parámetros     | Explicación|
| -------------- | ---------- |
| json | json con la tarea/novedad a grabar |

Graba un valor de una tarea en el periodo actual.

```javascript
const json = {
    cod_tarea: orbita.param.cod_tarea,
    periodo: orbita.param.periodo,
    valores: {
	importe: 1250.25
    }            
}

// Grabar tarea.
const [error, result] = orbita.grabarTareaValor(json);
if ( !result ) {          
  console.error("error al grabar tarea");
  console.error(error.error);
  utiles.errorJson( json );
}
```

<a id="grabar_propiedades_conexion"></a>
#### grabarPropiedadConexion(json)

| Parámetros     | Explicación|
| -------------- | ---------- |
| json | json con los datos de la propiedad a grabar |

Graba un valor de propiedad de una conexión en el periodo actual.

```javascript
const json = {
 cod_propiedad: "C000010003",
 cod_conexion: conexion.cod_conexion,
 valores: {
   valor: reg.valor
 },
 periodo: utiles.getStrExcelDate(reg.periodo)
};

// Grabar propiedad.
const [error, result] = orbita.grabarPropiedadConexion(json);
if ( !result ) {          
 console.error("error al grabar propiedad");
 console.error(error.error);
 utiles.errorJson( json );
}
```

<a id="leer_novedad_valor"></a>
#### leerNovedadValor(cod_novedad, cod_conexion, estado, periodo)

| Parámetros     | Explicación|
| -------------- | ---------- |
| cod_novedad | código novedad |
| cod_conexion | código conexion o string vacio |
| estado | estado de la novedad. Opcional. |
| periodo | periodo. Opcional. |

Devuelve un objeto con los datos de valores de la novedad.
Sino se indica estado trae las novedades en estado PENDIENTE. 
Sino se indica periodo, se toma el periodo activo.

```javascript
    // Leer novedad
    const aviso = orbita.leerNovedadValor(orbita.param.cod_conexion, "aviso");
    utiles.logJson(aviso);
```

<a id="grabar_novedad_valor"></a>
#### grabarNovedadValor(json)

| Parámetros     | Explicación|
| -------------- | ---------- |
| json | json con los datos de la novedad a grabar |

Graba un valor de novedad en el periodo actual.

```javascript
const json = {
 cod_novedad: "C000010003",
 cod_conexion: conexion.cod_conexion,
 periodo: orbita.param.periodo,
 estado: "PENDIENTE",
 valores: {
  tipo_comprobante: reg.t_comp,
  numero_comprobante: reg.n_comp,
  fecha: utiles.getStrExcelDate(reg.fecha),
  importe: utiles.floatNum( reg.importe )
 }   
};

// Grabar novedad
const [error, result] = orbita.grabarNovedadValor(json);
if ( !result ) {          
 console.error("error al grabar novedad");
 console.error(error.error);
 utiles.errorJson( json );
}
```	
