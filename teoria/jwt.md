![logotipo de The Bridge](https://user-images.githubusercontent.com/27650532/77754601-e8365180-702b-11ea-8bed-5bc14a43f869.png  "logotipo de The Bridge")


# [Bootcamp Web Developer Full Stack](https://www.thebridge.tech/bootcamps/bootcamp-fullstack-developer/)

### HTML, CSS,  JS, ES6, Node.js, Frontend, Backend, Express, React, MERN, testing, DevOps

# JWT

![img](../assets/jwt.png)

JSON Web Token es un estándar abierto basado en JSON para la creación de tokens de acceso que permiten la propagación de identidad y permisos. 

Por ejemplo, un servidor podría generar un token indicando que el usuario tiene permisos de administrador y proporcionarlo a un cliente. El cliente entonces podría utilizar el token para probar que está actuando como un administrador en el cliente o en otro sistema.  El token está firmado por la clave del servidor, así que el cliente y el servidor son ambos capaz de verificar que el token es legítimo. Los JSON Web Tokens están diseñados para ser compactos y poder ser enviados en las URLs.

## Estructura
Los JSON Web Tokens generalmente están formados por tres partes: un encabezado o header, un contenido o payload, y una firma o signature

### Encabezado
Identifica qué algoritmo fue usado para generar la firma y es algo como esto: 

```

    header = '{"alg":"HS256","typ":"JWT"}'

```
- alg: algoritmo
- typ: tipo de token

### Contenido
El contenido lleva la información de los permisos del token:

```

    payload = '{"loggedInAs":"admin","iat":1422779638}'

```
- loggedInAs: permisos
- iat: marca temporal que indica cuándo se creó el token (el estándar lo recomienda)

### Firma 
La firma está calculada codificando el encabezamiento y el contenido en base64url,  concatenándose ambas partes con un punto como separador:

```
    key           = 'secretkey'
    unsignedToken = encodeBase64Url(header) + '.' + encodeBase64Url(payload)
    signature     = HMAC-SHA256(key, unsignedToken) 

```

El token final se concatena usando puntos, de la siguiente forma: 

```

    token = encodeBase64Url(header) + '.' + encodeBase64Url(payload) + '.' + encodeBase64Url(signature) # token es: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI 

```

Ahora vamos a implementar un ejemplo usando JWT: 

### Ejemplo: 

Los pasos a seguir son los siguientes: 

1. Crea un nuevo proyecto y genera tu package.json con npm init -y

2. Instala en tu proyecto express, jsonwebtoken y nodemon

3. Añade una opción de start en los scripts de package.json con el valor nodemon index.js (nodemon nos permite actualizar el código del servidor sin parar y arrancar).

4. Crea el siguiente fichero: 

**index.js:**

```javascript

    const express = require("express");
    const jwt = require("jsonwebtoken");

    const app = express();

    app.get("/api", (req , res) => {
        res.json({
            mensaje: "Nodejs and JWT"
        });
    });

    app.post("/api/login", (req , res) => {
        const user = {
            id: 1,
            nombre : "Davinia",
            email: "daviniadelarosa@gmail.com"
        }
        jwt.sign({user}, 'secretkey', {expiresIn: '1000s'}, (err, token) => {
            res.json({
                token
            });
        });

    });

    app.post("/api/posts", verifyToken, (req , res) => {

        jwt.verify(req.token, 'secretkey', (error, authData) => {
            if(error){
                res.sendStatus(403);
                console.log(error);
            }else{
                res.json({
                        mensaje: "Acceso correcto",
                        authData
                    });
            }
        });
    });

    // Authorization: Bearer <token>
    function verifyToken(req, res, next){
        const bearerHeader =  req.headers['authorization'];

        if(typeof bearerHeader !== 'undefined'){
            const bearerToken = bearerHeader.split(" ")[1];
            req.token  = bearerToken;
            next();
        }else{
            res.sendStatus(403);
        }
    }

    app.listen(3000, () => {
        console.log("nodejs app running...");
    });

```

5. Al no disponer de Front en esta aplicación debes realizar tus pruebas con Postman

![img](../assets/config_postman.png)

[DOCUMENTACION_JWT](https://jwt.io/)
