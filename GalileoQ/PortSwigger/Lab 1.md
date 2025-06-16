al realizar una solicitud de cambio de email podemos ver el PATH de la ruta que sigue la api. esto es un método que normalmente no esta securizado lo cual nos permite hacer solicitudes a diferentes endpoints de la api.

### [Métodos de solicitudes en api almacenables en caché](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods#safe_idempotent_and_cacheable_request_methods)

La siguiente tabla enumera los métodos de solicitud HTTP y su categorización en términos de seguridad, capacidad de almacenamiento en caché e idempotencia.

|Método|Seguro|Idempotente|Almacenable en caché|
|---|---|---|---|
|[`GET`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/GET)|Sí|Sí|Sí|
|[`HEAD`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/HEAD)|Sí|Sí|Sí|
|[`OPTIONS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/OPTIONS)|Sí|Sí|No|
|[`TRACE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/TRACE)|Sí|Sí|No|
|[`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PUT)|No|Sí|No|
|[`DELETE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/DELETE)|No|Sí|No|
|[`POST`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST)|No|No|Condicional*|
|[`PATCH`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PATCH)|No|No|Condicional*|
|[`CONNECT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/CONNECT)|No|No|No|

![[Pasted image 20250616152320.png]]