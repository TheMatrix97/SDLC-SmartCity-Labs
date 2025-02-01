# Threat Modeling LAB

## Storyline

Trabajamos para una StartUp que se dedica al despliegue de cargadores para vehículos eléctricos en la ciudad. Para ello, está diseñando una aplicación web que permitirá a los usuarios acceder a un mapa con todos los puntos de recarga y activar el cargador de forma remota bajo demanda. 

A continuación se muestra un esquema del sistema propuesto

![](./figures/base_schema.png)

Nos han pedido hacer un análisis de amenazas `Threat Modeling` utilizando el modelo `STRIDE`. Para acotar el problema, nos centraremos en la interacción del usuario con el servicio web y el almacenamiento en una base de datos SQLServer. Tal y como se muestra a continuación

![](./figures/focus_schema.png)

## Requisitos

Utilizaremos la herramienta [Microsoft Threat Modeling Tool](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool). La podemos descargar en la web oficial en el siguiente enlace: (https://aka.ms/threatmodelingtool).


## Lab

1- Abriremos la herramienta de modelado de Amenazas y crearemos un proyecto nuevo. Utilizaremos la plantilla de modelos `SDL TM Knowledge Base (Core)`

![](./figures/create_project.png)


2- Añadiremos el recurso `Web Application` a nuestro proyecto. Este representará la aplicación web que dará servicio a los usuarios.
![](./figures/web_application.png)

3- A continuación, añadiremos el objeto `Human User`, que representará los clientes que interactuaran con esta aplicación web. Cambiaremos el nombre del objeto por `Clientes` para mayor claridad.

![](./figures/clientes.png)

4- Definiremos los flujos de datos entre los clientes y la aplicación web, estos serán del tipo `HTTP`. Un flujo de `request` de cliente a la aplicación web y el flujo contrario de respuesta, tal y como se muestra a continuación

![](./figures/client_data_flow.png)

5- Si no indicamos lo contrario, la herramienta presupondrá que todos los componentes del modelo residen en el mismo dominio. En este caso concreto, tendremos que indicarle que el tráfico entre el cliente y la aplicación web pasará por internet. Esta característica la definiremos dibujando una línea `internet boundary`, que cruzará el flujo definido anteriormente

![](./figures/internet_boundary.png)

6- En este punto, ya tenemos una primera iteración del modelo. Lo analizaremos para revisar las amenazas que detecta la herramienta, que deberían de ser 13

![](./figures/client_threats.png)

7- Si nos fijamos, hay una amenaza relacionada con la utilización de un canal de datos no seguro entre el cliente y la aplicación Web (`Data Flow Sniffing`). Modificaremos el modelo, substituyendo las conexiones `HTTP` por `HTTPS` y volveremos a analizar el modelo

![](./figures/fix_http.png)

8- El siguiente paso es incluir la conexión con la base de datos SQL. La utilizaremos para almacenar el estado de la aplicación: usuarios, peticiones de carga, registros de cargadores... etc.

Añadiremos los siguientes atributos acorde a nuestro caso de uso:
- Stores Credentials (Yes)
- Stores Log Data (Yes)
- Encrypted (Yes)
- Write Access (Yes)
- Backup (Yes)


![](./figures/add_db.png)

9- Conectaremos la base de datos con la aplicación web, utilizando una conexión `Generic Data Flow`, que representa un canal cifrado y autenticado mediante SSL, en la implementación real se utilizará `TLS`.

Modificaremos los atributos de las conexiones con las características propias de una conexión TLS a una base de datos

- Destination Authenticated (Yes)
- Provides Confidenciality (Yes)
- Provides Integrity (Yes)

![alt text](./figures/connect_db.png)


10- Analiza el modelo y revisa las amenazas que ha detectado la herramienta. ¿Crees que todas son aplicables a nuestro caso de uso?
En nuestro caso, marcaremos la amenaza `Cross Site Scripting` como mitigada, ya que estamos utilizando un framework web que sanitiza los datos antes de imprimirlos

![alt text](./figures/mitigated_threat.png)

11- Genera un report completo del modelo que hemos dibujado y observa el informe generado. ¿Qué información incluye?

![alt text](./figures/full_report_gen.png)

### Opcional

12- Queremos que nuestra aplicación permita a los usuarios hacer login con Google vía OAuth2.0 utilizando el flujo `Authorization Code Grant` y acceder a su información en la nube. Buscar información sobre este flujo e introduce los cambios necesarios en el modelo para incluir esta funcionalidad

<details>
<summary>Hint</summary>

A continuación se muestra una propuesta. Por una parte el cliente deberá acceder al proveedor para autorizar la aplicación, y por otra, la aplicación web tendrá que acceder al servicio para pedir el token y acceder a la información del usuario.

![alt text](./figures/optional_proposal.png)

Revisa la rama `solution` para obtener el fichero .tm7 del modelo propuesto como solución.

</details>