# MDB Vue

Web oficial [MDB Vue](https://mdbootstrap.com/education/vue/getting-started/)

## Inicie el proyecto

### 1º   Descarge los archivos del proyecto
Una vez que tengamos nuestro entorno listo (es decir, npm instalado). Descarguemos los archivos de trabajo. Podemos hacerlo de dos formas:

**Opción A** Repositorio GIT

Para descargar archivos de trabajo, abra una consola y escriba:
```bash
git clone https://github.com/mdbootstrap/Vue-Tutorial-Agenda-App
```

**Opción B** Archivo ZIP

Si por alguna razón no desea usar git directamente, simplemente puede descargar este archivo zip y extraerlo.

Vue Agenda App [github](https://github.com/mdbootstrap/Vue-Tutorial-Agenda-App.git)

### 2º  Abra el proyecto (VSCode)

Abra un proyecto en su editor favorito. Utilizo y recomiendo Visual Studio Code de Microsoft.
Si usa VSCode, le sugiero encarecidamente que instale la extensión Vetur, que agregará funciones útiles a VSCode, como un resaltador de sintaxis y el reconocimiento de archivos ".vue".

### 3º   Inicie el proyecto con VSCode

Arrastre la carpeta "start-here" a VSCode y en el menú seleccione:
Terminal > New Terminal.

**Primero** descarge el **package manager** con:
```
npm install
```
**Segundo** inicie el proyecto en el navegador:
```
npm start
```
**Tercero** abra el puerto http://localhost:8080



## ¡HOLA MUNDO!
Abra el archivo "src/App.vue".
Escribe: 
```vue
<p> Hola Mundo Vue </p> 
```
entre 
```html
<template> </template>
```
Verifique el resultado en el navegador
Nuestra aplicación se está ejecutando.

### Estructura del proyecto

![Estructura](/img/estructuraProyecto.jpg "Estructura")

1. El directorio **node_modules** es donde se almacenan todas las bibliotecas que necesitamos para construir Vue lo generamos al ejecutar el comando **"npm install"**.
2. En el directorio **src**, se coloca el código fuente de su aplicación.
3 En el directorio **assets**, guardamos cualquier archivo estático, como imágenes o iconos.
4. El archivo **App.vue** es el componente principal en el que se incluyen (anindan) todos los demás componentes.
5. El archivo **main.js** es lo que renderiza nuestro componente App.vue (y todo lo que está anidado en él) y lo monta el DOM o HTML.
6. **index.html** es el archivo principal de nuestra aplicación.

![index.html](/img/index.png "index.html")

Si echamos un vistazo dentro de nuestro archivo **"index.html"**, podemos ver que hay un **"div"** con la identificación de **"app"**, lo que significa que aquí es donde se montará nuestra aplicación.
       
### Componente de archivo único

Echemos un vistazo al archivo **App.vue** llamado componente de archivo único.
Como puede ver, el archivo consta de tres partes:

1. Templates (html)
2. Scripts (código)
3. Styles (css)


Como puede deducir fácilmente, **template** es responsable de una **vista** (html) de nuestro componente, el **script** contiene la **configuración** del componente y las **funciones** requeridas, mientras que los **styles** contienen los estilos **CSS** que se aplican a nuestro componente.


Primero, reemplace el contenido del archivo App.vue con el siguiente contenido
```vue
<template>
  <div class="app">
    <h1>Este es tu primer componente en Vue</h1>
    <h3>{{ mensaje }}</h3>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    mensajee: "El poder de MDBVue!"
  })
};
</script>

<style>
.app {
  margin: 0 auto;
  font-family: sans-serif;
  background-color: #ccf7e2;
  padding: 10px;
  border-radius: 5px;
  max-width: 500px;
}
</style>
```

## TEMPLATE

### Elemento principal

Esta parte del componente es responsable de representar su **vista**. Como puede notar, usamos **HTML** en el interior. Un dato importante para recordar es que el contenido interno debe contener solo un elemento principal.

### Interpolación de llaves dobles

![Llaves dobles](/img/llavesDobles.png "Llaves dobles")

El otro elemento interesante y posiblemente nuevo para usted es el título. Esta es la sintaxis denominada **"llaves dobles"**. Esta interpolación de texto es la forma más básica de vinculación de datos. Como puede ver, la variable se reemplaza dinámicamente por la **cadena de texto** asignada a la **variable** dentro de la función del script.

### Usar expresiones de JavaScript

Hasta ahora, solo hemos vinculado claves de propiedad simples en nuestras plantillas. Pero Vue.js en realidad admite todo el poder de las expresiones de JavaScript dentro de todos los enlaces de datos. Por ejemplo, el siguiente código:
```html
///////// PARTE TEMPLATE
<ul>
  <li>{{ number + 1 }}</li>
  <li>Value of ok var is: + {{ ok ? 'true' : 'false' }}</li>
  <li>{{ message.split('').reverse().join('') }}</li>
</ul>


///////// PARTE SCRIPT

data: () => ({
  message: "Powered by MDB!",
  number: 5,
  ok: true
})
```

## SCRIPT

Esta parte del código es importante para cada componente por varios motivos:

1. **Exportar nuestro componente** (y define su nombre) para que podamos usarlo en otros componentes.
2. Tenemos que **importar** y **enumerar** los componentes externos antes de poder usarlos dentro de nuestro componente
3. Definimos los datos (atributos) (variables) sus valores
4. Definimos las **funciones de JavaScript** utilizadas dentro de un componente.

Estudiaremos el primer y segundo punto en detalle en la próxima lección donde aprenderemos cómo exportar e importar componentes dentro de Vue. Por ahora, echemos un vistazo más de cerca a los puntos tres y cuatro.

### Atributos

Como ya viste en una lección anterior, podemos definir **variables** dentro de la **función** para poder usarlas dentro de nuestra parte de **template**:
Podemos usarlo como parte de nuestra lógica, por ejemplo:
```html
<span v-if = "visible"> Ahora me ves </span>

///////// PARTE SCRIPT

datos: () => ({
  mensaje: "¡Desarrollado por MDB!",
  visible: true
})
```
O como entrada de datos (que podemos modificar directamente mediante funciones):

```html
<ol>
  <li v-for = "todo in todos"> {{todo.text}} </li>
</ol>

///////// PARTE SCRIPT

data: () => ({
  todo: [
    {texto: "Aprender JavaScript"},
    {texto: "Aprender Vue"},
    {texto: "Crea algo increíble"}
  ]
})
```
### Funciones

Podemos hacer bastantes **JavaScript** dentro de las secciones de los **template** utilizando la interpolación de **"llaves dobles"**. Aunque las expresiones dentro de la plantilla son muy convenientes, diseñadas para operaciones simples. Poner demasiada lógica en sus templates es difícil de mantener. Por ejemplo:
```html
<li> {{message.split (''). reverse (). join ('')}} </li>
```
El template ya no es simple y declarativo. Tienes que mirarlo por un segundo antes de darte cuenta de que muestra el reverso. El problema se agrava cuando desea incluir el reverso en su plantilla más de una vez.
Es por eso que para cualquier lógica compleja, debe usar funciones y devolver el resultado final.
```html
///////// PARTE TEMPLATE
<p>Original message: "{{ message }}"</p>
<p>Computed reversed message: "{{ reversedMessage }}"</p>

///////// PARTE SCRIPT

export default {
  name: "App",
  data: () => ({
    message: "Powered by MDB!",
  }),
  computed: {
    // a computed getter
    reversedMessage: function() {
      // `this` points to the vm instance
      return this.message
        .split("")
        .reverse()
        .join("");
    }
  }
};
```
Como puede ver, podemos usar **"llaves de doble"** no solo para devolver el valor de una variable, sino también para **llamar a funciones** definidas dentro de la parte del script.

## STYLES

Por último, pero no menos importante, está nuestra parte de **CSS**. Esta sección es bastante autodescriptiva. Está destinado a almacenar todos los estilos utilizados por el componente. Lo importante es que los estilos pueden tener un alcance.

Esto significa que si usa un estilo como el siguiente:
```html
<style>
div [clase * = "col"] {
   borde: 1 px punteado en negro;
}
</style>
```
Este **estilo** solo se aplicará a cualquier **div** dentro de **todo el sitio web**. Será global.

### Scope

Si desea limitar su **alcance** a **divs** solo dentro de nuestro componente, puede agregar la opción:
```html
<style scope>
...
</style>
```
## Componentes

Entre otras ventajas, Vue ganó un impulso increíble durante el último año gracias a sus componentes reactivos y componibles. Los **componentes** son **instancias** de Vue **reutilizables** con un nombre. Podemos usar estos componente como un elemento personalizado dentro del componente principal.

Es común que una aplicación se organice en un árbol de componentes anidados:

![Componentes](/img/components.png "Componentes")

### Crear y exportar un componente

1. Crea una nueva carpeta llamada **Components** dentro de la carpeta **src**

![Componentes](/img/carpetaComponents.gif "Componentes")

2. Crea un archivo llamado **HolaMundo.vue** dentro de la carpeta **components**

![HolaMundo](/img/holamundo.gif "HolaMundo")

3. Copia el siguiente código y pégalo dentro de **HolaMundo.vue**

```vue
<template>
  <div class="hello">
    <h3>I am Hello World Component</h3>
  </div>
</template>

<script>
export default {
  name: "HelloWorld"
};
</script>

<style scoped>
h3 {
  font-weight: normal;
  padding-top: 20px;
  padding-bottom: 30px;
}
</style>
```
Como puede ver en la parte del **script** exportamos el componente.
```vue
<script>
export default {
  name: "HelloWorld"
};
</script>
```
Aquí es donde podemos definir el nombre de nuestro componente. Ahora podemos usarlo dentro de nuestro componente **App.vue**.

### Importando componentes

Abra y remplace el contenido de **App.vue** con el siguente código:

```vue
<template>
  <div class="app">
    <h2>I am App (main component)</h2>
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
.app {
  margin: 0 auto;
  font-family: sans-serif;
  background-color: #ccf7e2;
  padding: 10px;
  border-radius: 5px;
  max-width: 500px;
}
</style>
```
Para usar el componente **HolaMundo** primero tenemos que **importarlo** y **definirlo** dentro de la parte del **script**:

1. Importando:
```vue
<script>
import HelloWorld from "@/components/HelloWorld";

[...]
</script>
```
2. Agregando a la lista de componentes:

```vue
export default {
  name: "App",
  components: {
    HelloWorld
  }
};
```
3. Ahora podemos usar nuestro nuevo componente en **template** usando la etiqueta:
```html
<HelloWorld />
```