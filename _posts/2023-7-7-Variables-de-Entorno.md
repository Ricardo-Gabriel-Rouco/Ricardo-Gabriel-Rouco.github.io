En este post vamos a ver las variables de entorno empezando por lo más simple de todo:

### ¿Que son las variables de entorno?

Definamos primeramente lo que es un entorno. En todo proyecto que iniciamos tanto de front como de back, dependemos siempre de diversos factores que van más allá del proyecto en si, por ejemplo, del sistema operativo (S.O), de la base de datos (BD). Incluso de claves que deben ser privadas y no deben aparecer en nuestro código, imagínense que en nuestro repositorio de github dejemos escrito en el código fuente la contraseña de acceso de un mail empresarial, o una clave privada para acceder a una api. El caos generado por eso podría ser épico.
Ahora bien a todo esto se lo denomina *"entorno"* y tenemos varios tipos del mismo los cuales se pueden resumir en tres grandes tipos:

- Entorno de desarrollo: este es donde nosotros desarrollamos la aplicación (si, muy original el nombre). En donde nosotros vamos codeando y agregando funcionalidades y refactorizando e incluso en es la etapa donde más café tomamos

- Entorno de testing: siguiendo la misma logica, es el entorno donde literalmente se testea la aplicación. Si uno cumple el rol de tester, en este entorno es donde ustedes deberían hacer todas las pruebas necesarias para que todo funcione correctamente

- Entorno de producción: a qué nos referimos con 'entorno de produccion'? Simple, cuando ya pasamos las etapas de desarrollo y testeo, llega el momento de hacer el deploy, de exponerlo al mundo para que pueda ser usado por un usuario (valga la redundancia). Y este es el entorno al que se lo llama como producción

Ahora, después de esta explicación te estará preguntando: ¿Para que necesito saber esto?. Bien, la respuesta es simple, cada entorno puede tener diferencias, podrian verlo de la siguiente manera: ustedes (la aplicacion) van a una entrevista de trabajo(el entorno) y su ropa (las variables de entorno) serian una camisa, un pantalon o ropa mas bien formal, en cambio si estan en sus casas programando quizas usen algo como un pijama o ropa vieja. Las variables de entorno van cambiando pero en cada entorno el proyecto sigue funcionando sin grandes cambios en su codigo fuente.
Los entornos de desarrollo, testing y producción pueden tener diversos requisitos, como por ejemplo, en desarrollo usas un BD Sqlite, en testing usas una BD MySQL server y en producción se termina usando una BD postgreSQL (lo se, parece absurdo e incoherente apriori pero puede llegar a ser un caso real), entonces sería muy engorroso cambiar el código de acuerdo a cada entorno. En este punto ya es donde necesitamos si o si crear esas variables.

### Mini tutorial ultra rápido y veloz de cómo usarlas

A fines prácticos les dejare un pequeño proyecto en Github en el que solo van a tener un entorno, va a ser una API muy simple y sencilla pa toda la familia. La misma va a tener como dependencias mínimas:

- Sequelize
- Express

Y va a tener la siguiente estructura:

```
\---api
    |   index.js
    |   package.json
    |
    \---src
        |
        +---controllers
        +---handlers
        +---middlewares
        +---models
        |       Products.js
        |
        +---routes
        |       index.js
        |
        ---utils
        | app.js
        | db.js        
| .gitignore
| README.md
```


Cómo primera instancia tenemos que instalar la librería `dotenv` de la siguiente manera dependiendo de su gestor de paquetes:

<u>NPM</u>

`npm install dotenv`

<u>PNPM</u>

`pnpm install dotenv`

<u>YARN</u>

`yarn add dotenv`

**Recuerden ejecutar estos comandos en la terminal siempre ubicados en la carpeta donde tienen el package.json**



Está librería lo que les va a permitir es poder leer el archivo con las variables de entorno. Pero...¿ Dónde está ese archivo?. Bueno, nunca lo creamos pero vamos a ello. En la misma carpeta donde tienen creado el proyecto es decir donde tiene el archivo *package json* que sería nuestra carpeta raíz es ahí donde tienen que crear un archivo llamado con el nombre `.env`, si, así tal cual. ¿Suena raro no? Pero es así como funciona.

Ahora revisando el contenido de los archivos de nuestro proyecto van a ver una carpeta llamada *src* donde hay varios archivos necesarios para que el servidor funcione pero el archivo en el que vamos a trabajar hoy en dia que es el archivo `index.js`



Al cual al abrirlo veremos en el cual nos concentraremos en las siguientes lineas:

```js
server.listen(5000, () => {
  console.log('listening at 5000');
});
```

Como veran en estas lineas, al principio todo parece estar correcto pero si queremos aplicar las variables de entorno debemos dirigirnos a nuestro archivo `.env` y crear la siguiente linea:

```
PORT=5000
```

Dentro de nuestro archivo `index.js` hacemos asi:

1. Importamos la confiuracion de la libreria `dotenv` al inicio del mismo
   
   ```js
   require('dotenv').config()
   ```

2. Desestructuramos la variable que recien creamos
   
   ```javascript
   const { PORT } = process.env
   ```

3. y reemplazamos el numero de puerto por nuestra variable de entorno
   
   **Antes**
   
   ```javascript
   server.listen(5000, () => {
     console.log('listening at 5000');
   });
   ```

    **Despues**

```javascript
server.listen(PORT, () => {
  console.log(`listening at ${PORT}`);
});
```

Ahora al iniciar el servidor con `npm start` les mostrara

![Esta es una imagen de ejemplo](/images/Terminal.png)

