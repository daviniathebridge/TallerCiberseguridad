![logotipo de The Bridge](https://user-images.githubusercontent.com/27650532/77754601-e8365180-702b-11ea-8bed-5bc14a43f869.png  "logotipo de The Bridge")


# [Bootcamp Web Developer Full Stack](https://www.thebridge.tech/bootcamps/bootcamp-fullstack-developer/)

### HTML, CSS,  JS, ES6, Node.js, Frontend, Backend, Express, React, MERN, testing, DevOps

# Cambio de contraseña con JWT - Taller Coke

[REPOSITORIO](https://github.com/jscbridge/jwt)

## Código

### Raíz

- server.js

```js
const express = require("express");
const router = require("./routes/routes");
const app = express();

// Recoge la conexion de la bbdd
require("./dataBase/mongo");

app.use(express.json());
// Declarar el motor de plantilla
app.set("view engine", "ejs");
// RUTAS ESTÁTICAS ejs y sus js respectivos
app.use(express.static("./views")); 
app.use('/scripts', express.static('views/scripts'))

app.use("/", router);
const port = 5000;
app.listen(port, () => console.log(`Server ON: ${port}`));
```

- package.json: 

```js
{
  "dependencies": {
    "ejs": "^3.1.8",
    "express": "^4.18.2",
    "jsonwebtoken": "^8.5.1",
    "mongoose": "^6.7.2",
    "nodemon": "^2.0.20"
  },
  "scripts": {
    "start": "nodemon server.js"
  },
  "name": "jwtsept",
  "version": "1.0.0",
  "main": "server.js",
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}
```

###  Vistas

- Directorios: 

1. css
- login.css: 

```css
    body {
    background-color: rgb(48, 45, 97);
}
#form-email{
    display: flex;
    justify-content: center;
}
#container {
    width: 20%;
    background-color: rgb(111, 107, 175);
    padding: 10px;
    border-radius: 10px;
}
h4 {
    color: white;
    text-align: center;
}
#cont-body {
    display       : flex;
    flex-direction: column;
}
p,a{
    margin-top: 5%;
    text-align: center;
    color:rgb(233, 173, 173)
}
a{
text-decoration: none;
}
```

- confirmar.css: 
```css
body {
    background-color: rgb(48, 45, 97);
}
#form-password{
    display: flex;
    justify-content: center;
}
#container {
    width: 20%;
    background-color: rgb(111, 107, 175);
    padding: 10px;
    border-radius: 10px;
}
h4 {
    color: white;
    text-align: center;
}
#cont-body {
    display       : flex;
    flex-direction: column;
}
p,a{
    margin-top: 5%;
    text-align: center;
    color:rgb(233, 173, 173)
}
a{
text-decoration: none;
}
```

2. scripts

- login.js: 

```js
let btn = document.getElementById("btn");
btn.addEventListener("click", () => {
  let email = document.getElementById("email").value;
  
  let info = {
    method: "POST",
    body: JSON.stringify({ email }),
    mode: "cors",
    headers: {
      "Access-Control-Allow-Origin": "*",
      "Content-type": "application/json",
    },
  };

  fetch("/getUser", info)
    .then((res) => res.json())
    .then((token) => {
      if (!token) {
        document.getElementsByTagName("p")[0].innerText =
          "El email no existe en la BD";
      } else {
        document.getElementsByTagName("p")[0].innerText = "";
        document.getElementsByTagName("a")[0].style.display = "block";
        document.getElementsByTagName(
          "a"
        )[0].href = `http://localhost:5000/paginapassword/${token}`;
      }
    });
});

```

- paginaPass.js: 

```js
let btn = document.getElementById("btn2");
btn.addEventListener("click", () => {
  let newPass = document.getElementById("pass1").value;
  let repeatPass = document.getElementById("pass2").value;
  var ok = newPass == repeatPass;

  // [window.location.pathname] -> Recoge la url de la pg
  let token = window.location.pathname.split("/")[2];

  if (ok) {
    let password = newPass;
    
    let info = {
      method: "POST",
      body: JSON.stringify({ password, token }),
      mode: "cors",
      headers: {
        "Access-Control-Allow-Origin": "*",
        "Content-type": "application/json",
      },
    };

    fetch("/verificar", info)
      .then((res) => res.json())
      .then((data) => {
        if (data) {
          document.getElementsByTagName("p")[0].innerText =
            "Éxito! - Se ha actualizado su nuevo Password - ";
        } else {
          document.getElementsByTagName("p")[0].innerText = "Token incorrecto o caducado";
        }
      });
  } else {
    document.getElementsByTagName("p")[0].innerText =
      "No son iguales o está vacio el campo";
  }
});

```
3. views

- login.ejs: 

```HTML
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Cambio de Password</title>
    <link rel="stylesheet" href="../css/login.css" />
  </head>
  <body>
    <div id="form-email">
      <div id="container">
        <h4>RECUPERACIÓN CONTRASEÑA</h4>
        <div id="cont-body">
          <input type="email" name="email" id="email" placeholder="Email" />
          <input type="button" value="Cambio" id="btn" />
        </div>
        <!-- Simula en link enviado al email -->
        <a id="link-email" href="" style="display: none">Link confirmación (email)</a>
        <p></p>
      </div>
    </div>
    <br />
  </body>
  <script src="http://localhost:5000/scripts/login.js"></script>
</html>
```

- paginaPass.ejs: 

```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="../css/confirmar.css" />
    <title>Confirmar</title>
  </head>
  <body>
    <div id="form-password">
      <div id="container">
        <h4>CAMBIO DE CONTRASEÑA</h4>
        <div id="cont-body">
          <input type="password" name="pass1" id="pass1" placeholder="Nuevo Password"/>
          <input type="password" name="pass2" id="pass2" placeholder="Repetir Password"/>
          <input type="button" value="Confirmar" id="btn2" />
          <p></p>
        </div>
      </div>
    </div>
      <script src="http://localhost:5000/scripts/paginaPass.js" defer></script>
  </body>
</html>
```

### Routes

- routes.js: 

```js
const router = require("express").Router();
const user = require("../controllers/users.controllers");

// Localhost:5000 -> [user.start]: carga view login.ejs.
router.get("/", user.start);
// Busca por correo en bbdd y realiza el jwt.
router.post("/getUser", user.getUser);
// Carga paginaPassword junto con el token en su url.
router.get("/paginapassword/:infoJwt", user.paginaPassword);
// Verifica el token y actualiza.
router.post("/verificar", user.verificar);

module.exports = router;

```

### Models 

- user.model.js: 

```js
//!  Creación del modelo (ej: usuario)
const mongoose = require("mongoose");


//! PLANTILLA - MODELO

const usuarioConstructorSchema = {
  email: String,
  password: String,
};

//! ESTRUCTURA EL MODELO PARA QUE LO ENTIENDA MONGODB
const usuarioSchema = mongoose.Schema(usuarioConstructorSchema, {
  versionKey: false,
});

//! Constante con el modelo con el nombre de "usuarios"
const Users = mongoose.model("users", usuarioSchema);

module.exports = Users;
```

### Database 

- mongo.js: 

```js
const mongoose = require("mongoose");
const url = "mongodb://localhost:27017/jwt";

mongoose.connect(url, function (err) {
  if (err) throw err;
  console.log("BBDD ON");
});
```

### Controllers

- user.controllers.js: 

```js
const userModel = require("../models/user.model");
const jwt = require("jsonwebtoken");

const User = {
  // Carga view LOGIN al arrancar.
  start: (req, res) => {
    res.render("../views/views/login.ejs");
  },
  // Carga view PAGNAPASS al dar -link-.
  paginaPassword: (req, res) => {
    res.render("../views/views/paginaPass.ejs");
  },
  // VERIFICA el TOKEN y recogemos el email. [true] -> UPDATE.  [false] -> Incorrecto
  verificar: async (req, res) => {
    let { token, password } = req.body;
    try {
      // Verifica el token donde está el email del usuario
      let jwtVerify = jwt.verify(token, "Ll140ll140");
      let email = jwtVerify.email;
      await userModel.findOneAndUpdate({ email }, { password });
      res.json(true);
    } catch (error) {
      res.json(false);
    }
  },
  // BUSCA el user: [true] - > CREA un token(correo) y lo devuelve. [false] -> No existe usuario
  getUser: async (req, res) => {
    const { email } = req.body;
    const infoUser = await userModel.findOne({ email });
    if (infoUser) {
      const infoJwt = jwt.sign({ email }, "Ll140ll140", {
        expiresIn: "1000s",
      });
      res.json(infoJwt);
    } else {
      res.json(false);
    }
  },
};
module.exports = User;

```
