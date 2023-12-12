# React NodeJS Cloud Builds

El siguiente proyecto detalla los pasos para levantar una app con front-end en react y back-end en nodejs.   
Además integra el despliegue automático del cliente y el servidor en Cloud Run a través de Cloud Build. 

![React NodeJS Cloud Builds.png](img/arquitectura.png)

## Creación de usuarios con MERN Full Stack

Este es un proyecto construido utilizando la pila MERN Full Stack, que incluye MongoDB, Express, React y Node.js. 
MongoDB se utiliza como la base de datos y Mongoose se utiliza para interactuar con ella. 
Mongoose proporciona una interfaz simple y fácil de usar para realizar operaciones CRUD en la base de datos.

El proyecto consta de las siguientes vistas:


- **Home**:En la vista de Home, los usuarios tienen la opción de acceder a una de las siguientes funcionalidades:
    - Registro de usuario: Los usuarios pueden acceder a un formulario de registro para crear nuevas cuentas de usuario 
    en la aplicación.
    - Base de Datos: Los usuarios pueden acceder a una funcionalidad que les permite consultar la base de datos y 
    obtener información almacenada en ella.

- **Registro (Register)**: En esta vista, los usuarios pueden acceder a un formulario de registro donde pueden registrar 
nuevos usuarios en la base de datos de MongoDB.

- **Base de Datos (Database)**: Esta vista permite a los usuarios consultar la base de datos y obtener información 
almacenada en ella.

Puedes ver una pequeña descripción de las palabras más relevantes del proyecto. 

- **MongoDB**: Base de datos NoSQL utilizada para almacenar los datos del proyecto.  
- **Express**: Framework de Node.js utilizado para construir la API RESTful del backend.  
- **React**: Biblioteca de JavaScript utilizada para construir el frontend de la aplicación.  
- **Node.js**: Entorno de ejecución de JavaScript utilizado para construir el backend de la aplicación.  


## ¿Cómo está construido?


### Server

El servidor está construido utilizando Express, un framework de Node.js que facilita la creación de API RESTful. 
Se utiliza para manejar las solicitudes de los clientes y realizar operaciones en la base de datos.


## Creación de la base de datos 


- Vamos a la web oficial de MongoDB: [https://www.mongodb.com](https://www.mongodb.com/).
    
- Creamos un nuevo proyecto en MongoDB Atlas. Seguido a esto, creamos una BBDD pinchando en  "Build a Database", guardamos el usuario y contraseña que nos devuelve.
    
- Seleccionamos "Connect" y elegimos "Connect with MongoDB Compass". Nos devolvera una cadena de conexion similar a esta.

`````bash
# mongodb+srv://<username>:<password>@cluster0.2lnyz2p.mongodb.net/
`````
- Modificamos **username** y **password** con el usuario y contraseña que nos devolvio antes al crear la Database y guardamos para luego crear el secreto en GCP.

- Permitimos el acceso a la Database a cualquier ip, para ello modificamos el **Network access** incluyendo 0.0.0.0/0 en **ADD IP ADDRESS**

![acceso_ips.png](img%2Facceso_ips.png)


## Dockerización de la Aplicación 

En este ejemplo hemos dividido la creación de la app en dos docker distintos, por un lado el 
cliente y por otro el servidor. 

Puedes ver los ficheros Dockerfile [Dockefile servidor](/server/Dockerfile),
[Dockefile cliente](/client/Dockerfile)

## Despliegue de la aplicación

Para el despliegue de la aplicación usaremos Cloud Build y unos ficheros de construcción. 
Estos ficheros que están montados para el cliente y servidor, hacen referencia a los Dockerfile, para 
en primer lugar realizar la construcción de los contenedores. 

Los ficheros cloud build puedes verlos en los siguientes ficheros: 

### [cloudbuild-server](cloudbuild_client.yml)

- En este fichero .yml aparte de modificar el nombre de nuestro proyecto para que funcione, tendremos que crear un secreto en Google cloud plataform para que se pueda conectar a la base de datos de mongoDB.


- Nos vamos a Secret Manager, pulsamos en crear secreto y en valor del secreto introducimos la linea de conexion que nos devolvio anteriormente en mongoDB  *mongodb+srv://nuestro_usuario:password@cluster0.2lnyz2p.mongodb.net/*

![secreto.png](img%2Fsecreto.png)

- Por ultmimo, modificamos el campo **versionName:** del .yml introduciento el nombre del recurso de secreto que acabamos de crear.



### [cloudbuild-server](cloudbuild_server.yml)

- En este fichero .yml, aparte de realizar las mismas modificaciones que en fichero anterior, hay que modificar la ulr de los ficheros **Create.jsx** y **UsersDatabase.jsx** con la url que devuelve la CloudRun del server levantado anteriormente.

![img.png](img/modificacion_url_backend.png)

## Pagina web

Con esto, nuestra app estaria funcionando correctamente y cada cambio que se realice en el codigo del repositorio se implementara en nuestra app.

![pagina.png](img%2Fpagina.png)
