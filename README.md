# Respuesta a Test

## Pregunta 1

El promedio ponderado sirve para calcular el promedio cuando uno o más de los elementos tienen un peso (proporción) diferente en el cálculo. En el caso de la clase Transaccion no hay un campo que represente esa proporción, por lo tanto la función propuesta va a calcular únicamente el promedio.

[Solución aquí](https://gist.github.com/javierseixas/3f955d0f42e1574f21d5f91f61a34df2)


## Pregunta 2

### Servidor A

El servidor A presenta una API REST con el Recurso Transaction. Tiene que tener accesibles los siguientes endpoints:

```
GET /transactions
GET /transactions/{id}
POST /transactions
PUT /transactions/{id}
PATCH / transactions/{id}
DELETE /transactions/{id}
```

El ingreso manual de datos mediante formulario se realiza a través de la API REST y su endpoint `POST /transactions`
El ingreso de datos mediante scraping se realizaran mediante comandos si el sistema de scraping se encuentra en el mismo servidor o mediante la API REST si se encuantra en otro servidor, mediante `POST /transactions`.

### Servidor B

Obtiene la información sobre transacciones a través del endpoint `GET /transactions`. Para obtener un bulk de transacciones evitando repetir datos, se utiliza un parámetro `offset_date` para conocer la última transacción obtenida, y obtener a partir de la misma en la siguiente petición.
Cuando el servidor B obtiene los datos los almacena para su correspondiente proceso y verificación.

Cuando los datos están verificados, se informa al servidor A del resultado mediante la petición `PUT /transactions/{id}` o `PATCH /transactions/{id}` actualizando su estado.
Para que un registro erróneo no se volviera a obtener cuando se solicita `GET /transactions`, se utiliza un parámetro `not_erroneous=true` para filtrar los que son erróneos.

Para modificar datos en el servidor A, desde el servidor B se utilizaría las peticiones `PUT /transactions/{id}` y `PATCH /transactions/{id}` para hacer las modificaciones necesarias.

### Reportes en Servidor A

El servidor A ofrece reportes de mercado o sobre alguna propiedad en particular. La API REST propuesta es:

```
GET /transaction-reports/{id}
GET /market-reports
```

El recurso `/transaction-reports` ofrece el reporte por una transacción, dando por hecho que quién lo solicita conoce el `{id}` de la transacción
El recurso `/market-reports` ofrece un informe de mercado que englobe los datos que se consideren, y utiliza los parámetros `init-date` y `end-date` para marcar el rango de fechas sobre el que se quiere obtener el informe.

En función de la naturaleza del informe, ambos tipos podrían depender de un mismo recurso, sin embargo, sin más conocimiento del contexto del problema, se ha preferido separarlos en recursos diferentes para esta solución.


### Diagrama

![Diagrama servidores](https://github.com/javierseixas/cb-test-answers/blob/master/doc/transaction-servers.png)


### Aclaraciones

Los endpoints propuestos inicialmente `GET /transactions/{id}` y `DELETE /transactions/{id}` no se utilizan para los requisitos del problema, pero un sistema de estas características en entorno real muy probablemente los necesitaría.