# Agenda app

Web oficial [MDB Vue](https://mdbootstrap.com/education/vue/getting-started/)

## Presentación de la aplicación

Ahora que conocemos lo básico de cómo funciona Vue, ¡construyamos una aplicación real! Primero mostraré el resultado final y luego lo analizaré rápidamente. Después mostraré cómo crear esta aplicación desde cero.

### Ejecute la aplicación

Dentro de los archivos del tutorial encontrará un código de aplicación final, usémoslo para ejecutar nuestra aplicación:

- Abrir una **terminal*
- Navegue a la carpeta agenda-app **/final/**
- Instale las dependencias usando **npm install**
- Ejecute la aplicación usando **npm start**
- Vea los resultados en un **navegador**:

Vista previa de la aplicación

Nuestra aplicación se está ejecutando.

### Pruebe la aplicación

La aplicación que estamos a punto de construir es una aplicación de agenda diaria. Nos permite mostrar una lista de eventos planificados para hoy, incluida la hora, el título, la ubicación y la descripción. Podemos agregar nuevos eventos así como eliminar los existentes. La aplicación también muestra un pronóstico del tiempo.

Ahora tómate un tiempo para jugar con la aplicación. Tenga en cuenta que cada vez que agrega/elimina eventos, el contador de la columna de la derecha está cambiando.

### Adaptable (responsiva)

Nuestra aplicación también se adapta al dispositivo. Simplemente cambie el tamaño del navegador para ver cómo se muestra en diferentes tamaños de pantalla:

Ahora que sabe lo que va a construir, ¡no perdamos más tiempo y comencemos a construir nuestra aplicación!

## Descarge los archivos del proyecto

Una vez que tengamos nuestro entorno listo (es decir, npm instalado). Descarguemos los archivos de trabajo. Podemos hacerlo de dos formas:

**Opción A** Repositorio GIT

Para descargar archivos de trabajo, abra una consola y escriba:
```bash
git clone https://github.com/mdbootstrap/Vue-Tutorial-Agenda-App
```

**Opción B** Archivo ZIP

Si por alguna razón no desea usar git directamente, simplemente puede descargar este archivo zip y extraerlo.

Vue Agenda App [github](https://github.com/mdbootstrap/Vue-Tutorial-Agenda-App.git)

### Abra el proyecto (VSCode)

Abra un proyecto en su editor favorito. Utilizo y recomiendo Visual Studio Code de Microsoft.
Si usa VSCode, le sugiero encarecidamente que instale la extensión Vetur, que agregará funciones útiles a VSCode, como un resaltador de sintaxis y el reconocimiento de archivos ".vue".

### Inicie el proyecto con VSCode

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



## Inicie el proyecto

Comenzaremos a construir nuestra aplicación configurando la estructura de **dos columnas** de nuestra aplicación. Como puede ver en la aplicación de demostración, nuestra aplicación consta de dos columnas: la primera, a la izquierda y más ancha, que enumera todos los eventos diarios; y el de la derecha, que muestra el contador de eventos y el pronóstico del tiempo.

Como también notó, nuestra aplicación será adaptable. Esto es gracias a Bootstrap 4. En esta lección, aprenderemos cómo usar una cuadrícula de Bootstrap en Vue.

### Abra el proyecto

Para comenzar a construir nuestro proyecto, abramos una plantilla que he preparado para ti en los archivos de Tutorial que descargaste antes.

Abra el proyecto **agenda-app/start-here** en el editor de código, reemplace el contenido del archivo **App.vue** con el siguiente código:

```Vue
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="6">Left column</mdb-col>
      <mdb-col col="6">Right column</mdb-col>
    </mdb-row>
  </mdb-container>
</template>

<script>
import { mdbContainer, mdbRow, mdbCol } from "mdbvue";
export default {
  name: "App",
  components: {
    mdbContainer,
    mdbRow,
    mdbCol
  }
};
</script>

<style>
div[class^="col"],
div[class*=" col"] {
  border: 1px dotted black;
}
</style>
```
Lo que hicimos:
- Se agregó un contenedor, con una fila y dos columnas dentro dentro de la plantilla.
- Importó el componente de contenedor, fila y columna y los registró dentro del script
- Se agregó un estilo para que nuestros bordes de cuadrícula sean visibles dentro del estilo.

![Componentes](/MDBVue/img/appResponsive.gif "Componentes")

## El concepto de cuadrícula

Actualmente, nuestra aplicación tiene dos columnas de igual tamaño que se colocan una al lado de la otra en una pantalla grande. Cuando cambiemos el tamaño de la ventana de nuestro navegador, se anidarán uno debajo del otro.

¿Qué es la cuadrícula de Bootstrap?

![Componentes](/MDBVue/img/cuadricula.png "Componentes")

En pocas palabras, es un mecanismo que nos permite crear un sitio web adaptable. La cuadrícula Bootstrap nos permite definir filas, y cada fila consta de hasta 12 columnas.

![Componentes](/MDBVue/img/cuadricula2.png "Componentes")

Puede usar una columna que tendrá 12 bloques de largo o usar varios bloques más pequeños. La única regla es que no puede exceder más de 12 bloques seguidos. Los bloques no tienen que tener el mismo tamaño y  puede omitir algunas de las columnas, así como alinearlas horizontalmente.

![Componentes](/MDBVue/img/cuadricula3.png "Componentes")

### Puntos críticos de adaptabilidad

Bootstrap está desarrollado para priorizar el móvil: utiliza un puñado de consultas de medios para crear puntos de interrupción sensibles para nuestros diseños. Estos puntos de interrupción se basan principalmente en los anchos mínimos de la ventana y le permiten escalar los elementos a medida que cambia la ventana.

Esta es una de las características más importantes, en otras palabras: puede definir un tamaño de columna diferente para diferentes tamaños de pantalla. Bootstrap enumera cinco tamaños de pantalla diferentes, extra pequeño (predeterminado), pequeño (sm), mediano (md), grande (lg) y extra grande (xl). Cada tamaño tiene un punto de ruptura definido de acuerdo con la siguiente tabla:

![Componentes](/MDBVue/img/puntosRotura.png "Componentes")

Tabla de puntos de interrupción de respuesta de Bootstrap

Llegando a la conclusión, esta herramienta súper útil nos permite crear contenido que se ajustará automáticamente al tamaño de la pantalla. Probemos el siguiente código:
```vue
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
      <mdb-col xl="1" lg="2" md="4" sm="6" size="12">xl=1 lg=2 md=4 sm=6 xs=12</mdb-col>
    </mdb-row>
  </mdb-container>
</template>
```
Ejecute la aplicación y cambie el tamaño del navegador:
Espero que ahora pueda comprender lo fácil que es construir componentes adaptables usando Bootstrap. Volvamos a nuestra aplicación. Queremos crear un diseño de dos columnas que sea visible en el escritorio (extra grande y grande) así como en tabletas en aspecto horizontal (mediano). Para un tamaño de pantalla más pequeño, queremos que nuestras columnas se aniden una debajo de la otra.

### Cuadrícula receptiva de nuestra aplicación Calendario

Reemplace la plantilla de nuestro archivo App.vue con el siguiente código:
```html
<mdb-container>
  <mdb-row>
    <mdb-col col="9">Left column</mdb-col>
    <mdb-col col="3">Right column</mdb-col>
  </mdb-row>
</mdb-container>
```
Verifique el resultado en el navegador
Nuestra aplicación se está ejecutando.

## Componente eventos

Nuestra aplicación constará de dos componentes. El principal, el componente de la aplicación, que será responsable del diseño, y el componente de evento, un subcomponente que representará una única entidad de evento.

- Cree una nueva carpeta de **componentes** en **src/**
- Cree un nuevo archivo **Event.vue**
- **Pegue el código** de evento inicial
```Vue
<template>
  <div>
    <h3>9:00 - Title</h3>
  </div>
</template>

<script>
export default {
  name: "Event"
};
</script>

<style scoped>
</style>
```

La mayoría de la gente simplemente llamará a Event un componente. Esto está perfectamente bien, sin embargo, hay una cosa importante que quiero que recuerde si no tuvo ninguna experiencia con la Programación Orientada a Objetos en el pasado.

Nuestra clase es solo una definición del componente. Describe cómo se verá y se comportará nuestro componente, pero aún no crea una instancia del componente.

### Importar y renderizar el componente de evento

Abra el archivo **App.vue**
Importar evento:
```vue
<script>
import { mdbContainer, mdbRow, mdbCol } from "mdbvue";
import Event from "@/components/Event";
export default {
  name: "App",
  components: {
    mdbContainer,
    mdbRow,
    mdbCol,
    Event
  }
};
</script>
```
Renderizar el componente Evento en la parte de la plantilla
```vue
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <Event/>
      </mdb-col>
      <mdb-col col="3">Right column</mdb-col>
    </mdb-row>
  </mdb-container>
</template>
```

Como mencionamos antes, dado que nuestro componente Event es una definición, podemos renderizar múltiples instancias del mismo componente.

Agregue la segunda instancia del evento en la columna de la izquierda
Vista previa de la aplicación

![Componentes](/MDBVue/img/vista.png "Componentes")

Eliminar el texto de la columna de la derecha
```vue
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <Event/>
        <Event/>
      </mdb-col>
      <mdb-col col="3"></mdb-col>
    </mdb-row>
  </mdb-container>
</template>
```

Ahora, cuando sepamos cómo crear una instancia de nuestro componente Event, aprendamos cómo hacerlo más dinámico y pasarle algunos datos.

## Pasando datos

Anteriormente, aprendimos cómo crear el componente Evento. El problema es que este componente no será útil a menos que pueda pasarle datos, como la hora, el título, la ubicación y la descripción del evento específico que queremos mostrar. Ahí es donde los accesorios entran en escena.

### Añadiendo datos
Los **props** son atributos personalizados que podemos registrar en un componente. Cuando pasamos un valor a un atributo **prop**, se convierte en una **propiedad** de la instancia de componente en particular. Definamos algunos **props**:

Agregue tiempo, título, ubicación y descripción a la parte **script** de un evento:
```vue
export default {
  name: "Event",
    props: {
    time: {
      type: String
    },
    title: {
      type: String
    },
    location: {
      type: String
    },
    description: {
      type: String
    }
  },
};
```
Ahora, cuando nuestros **props** estén definidos, podemos actualizar nuestra plantilla. Para mostrar los accesorios usaremos la sintaxis de **llaves dobles** {{}}
Actualizar componente evento parte **template**:
```vue
<template>
  <div>
    <h3>{{time}} - {{title}}</h3>
    <p>{{location}}</p>
    <p>{{description}}</p>
  </div>
</template>
```
Lo último que tenemos que hacer es pasar un valor al componente de la aplicación.
Actualice el **template** del archivo **App.vue**:
```vue
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <Event time="10:00" title="Breakfast with Simon" location="Lounge Caffe" description="Discuss Q3 targets"/>
        <Event time="10:30" title="Daily Standup Meeting (recurring)" location="Warsaw Spire Office"/>
      </mdb-col>
      <mdb-col col="3"></mdb-col>
    </mdb-row>
  </mdb-container>
</template>
```
Ahora podemos generar dinámicamente instancias únicas de estos eventos.
![Componentes](/MDBVue/img/vista2.png "Componentes")

## Lista de eventos

Dado que el componente Evento recibe información como la fecha o el título del componente Aplicación (padre), ahora es el componente Aplicación el que será responsable de almacenar y administrar listas de eventos. 
Agreguemos una **lista de eventos** en la parte del **script** App.vue dentro de la **función data()**:
```vue
<script>
importar {mdbContainer, mdbRow, mdbCol} desde "mdbvue";
importar Evento de "@ / components / Event";
exportar predeterminado {
  nombre: "Aplicación",
  componentes: {
    mdbContainer,
    mdbRow,
    mdbCol,
    Evento
  },
    datos() {
    regreso {
      eventos: [
        {
          hora: "10:00",
          título: "Desayuno con Simon",
          ubicación: "Lounge Caffe",
          descripción: "Discutir los objetivos del tercer trimestre"
        },
        {
          hora: "10:30",
          title: "Reunión diaria de pie (recurrente)",
          ubicación: "Oficina de Varsovia Spire"
        },
        {
          hora: "11:00",
          title: "Llamada con RRHH"
        },
        {
          hora: "12:00",
          título: "Almuerzo con Timmoty",
          ubicación: "Cantina",
          descripción: "Evaluación del proyecto"
        }
      ]
    };
  },
};
</script>
```
Ahora, cuando tengamos nuestros datos iniciales, aprendamos a usar **for loop** para generar dinámicamente todos los eventos de la matriz **events[]**.

### v-for

2.1 Bucle dinámico

Reemplacemos nuestro código de evento estático:
```html
<Event time="10:00" title="Breakfast with Simon" location="Lounge Caffe" description="Discuss Q3 targets"/>
<Event time="10:30" title="Daily Standup Meeting (recurring)" location="Warsaw Spire Office"/>
```
con una llamada dinámica:
```html 
<Event
  v-for="event in events"
  :time="event.time"
  :title="event.title"
  :location="event.location"
  :description="event.description"
/>
```

Que funciona a las mil maravillas:
![Componentes](/MDBVue/img/vista3.png "Componentes")

La v-for es bastante autodescriptiva. Recorre cada elemento dentro de la matriz de eventos[] y crea una instancia de cada componente de evento.

Para acceder a los atributos de un evento en particular, estamos definiendo su alias en la declaración v-for:

v-for = "evento en eventos"

Ahora podemos acceder a él, es decir, a su título llamando event.title.

El alias para un solo elemento puede tener cualquier nombre, podríamos usar una versión más corta y llamarlo e:
```html
<Event
 v-for="e in events"
 :time="e.time"
 :title="e.title"
 :location="e.location"
 :description="e.description"
       />
```
Sin embargo, es una buena práctica utilizar un nombre descriptivo para facilitar la comprensión del código. Como también notó dentro del ciclo v-for, estamos mapeando los atributos de nuestros elementos, es decir, event.time (lado derecho) a la propiedad Event time (lado izquierdo):

(Propiedad de instancia de componente de evento): time = "event.time" (elemento de matriz de eventos [])

### Clave única

Todo funciona a las mil maravillas hasta ahora. Al menos en un navegador. Sin embargo, si miramos en nuestra consola notaremos la siguiente Advertencia:
![Componentes](/MDBVue/img/warning.png "Componentes")

Esta advertencia nos dice que cada elemento debe contener una clave única. Esto ayuda a Vue a acceder a cada elemento. Veamos rápidamente por qué esto es importante.

Con la ayuda de una pequeña extensión de Chrome, Vue.js devtools, podemos verificar el DOM virtual de nuestra aplicación. Asi es como luce ahora:
![Componentes](/MDBVue/img/devtools.png "Componentes")

Como puede ver, todas las instancias de componentes de eventos tienen exactamente el mismo aspecto. Si queremos actualizar uno en concreto, es difícil acceder a uno en particular. Podemos arreglar esto rápidamente. Actualicemos nuestro ciclo:
```html
<Event
  v-for="(event, index) in events"
  :index="index"
  :time="event.time"
  :title="event.title"
  :location="event.location"
  :description="event.description"
  :key="index"
/>
```
Y revise nuestro código nuevamente usando la misma extensión:

![Componentes](/MDBVue/img/devtools2.png "Componentes")

Como puede ver, cada evento tiene ahora una clave única (el primer elemento tiene por defecto clave = 0, por lo tanto, no se muestra).

Repasemos rápidamente el nuevo código v-for. Agregamos el segundo parámetro (índice) parámetro a nuestro ciclo:

```html
v-for = "(evento, índice) en eventos"
```

Este es un índice incrementado automáticamente, lo que significa que aumenta el valor con cada iteración. Lo hemos vinculado a la propiedad clave que ayudará a Vue a acceder rápidamente a un elemento específico sin necesidad de recorrer todos los elementos dentro de la matriz.

También agregamos un índice de propiedad adicional: y lo asignamos al mismo valor. Esto nos ayudará dentro de la aplicación, a acceder a un Evento específico desde el código fuente.

Dado que hemos agregado el mapeo a una nueva propiedad, el último paso es extender una lista de propiedades en el archivo Event.vue. Agreguemos un nuevo accesorio a la lista de accesorios:
```html
props: {
  index: {
    type: Number
  },
  time: {
    type: String
  },
  title: {
    type: String
  },
  location: {
    type: String
  },
  description: {
    type: String
  }
},
```
Como sabemos cómo renderizar eventos dinámicamente a partir de una matriz, démosle forma a nuestra aplicación.

## Estilo para el componente evento

Vamos a importar insignias e íconos de la biblioteca mdbvue en **Event.vue**
```html
<script>
import { mdbBadge, mdbIcon } from "mdbvue";

export default {
  name: "Event",
  components: {
    mdbBadge,
    mdbIcon
  },
  props: {
    index: {
      type: Number
    },
    time: {
      type: String
    },
    title: {
      type: String
    },
    location: {
      type: String
    },
    description: {
      type: String
    }
  },
  methods: {
    onDelete() {
      this.$emit('delete', this.index);
    }
  }
};
</script>
```

Los usaremos dentro de nuestra plantilla.
Actualice la plantilla con el siguiente código HTML:
```html
<template>
  <div>
    <div class="media mt-1">
      <h3 class="h3-responsive font-weight-bold mr-3 pt-0">{{time}}</h3>
      <div class="media-body mb-3 mb-lg-3">
        <mdb-badge tag="a" color="danger-color" class="ml-2 float-right">-</mdb-badge>
        <h6 v-if="title" class="mt-0 font-weight-bold">{{title}}</h6>
        <hr class="hr-bold my-2">

        <p class="font-smaller mb-0">
          <mdb-icon icon="location-arrow"/> {{location}}
        </p>
      </div>
    </div>
    <p class="p-2 mb-4 blue-grey lighten-5">{{description}}</p>
  </div>
</template>
```
Ahora nuestra aplicación comienza a verse mejor.
![Componentes](/MDBVue/img/vista4.png "Componentes")

Sin embargo, probablemente haya notado que algunos de los eventos no tienen una ubicación ni una descripción, pero aún mostramos un marcador de posición para ellos. Esto no se ve bien, por lo que para solucionarlo usaremos otra directiva de Vue: v-if

### v-if

Una de las razones por las que Vue ha crecido tan rápido recientemente es el hecho de que viene con una variedad de directivas predefinidas como v-for que ya conoce. En esta lección, aprenderemos a usar las directivas v-if, v-else y v-else-if.

Echemos un vistazo a nuestro párrafo de descripción:
```html
<p class="p-2 mb-4 blue-grey lighten-5">{{description}}</p>
```
Podemos definir fácilmente si queremos renderizar un párrafo completo usando una condición if simple:

```html
<p v-if="true">I am rendered!</p>
<p v-if="false">I am not :(</p>
```
Y a continuación se muestra un ejemplo de nuestro párrafo de descripción.

![v-if](/MDBVue/img/vif.gif "v-if")

Por supuesto, no queremos establecerlo globalmente, en su lugar queremos mostrar el párrafo dependiendo de si existe un prop de descripción. Para hacer eso, simplemente podemos proporcionarlo como un argumento para la función v-if.
```html
<p v-if="description" class="p-2 mb-4 blue-grey lighten-5">{{description}}</p>
```
Y usa el mismo mecanismo para la ubicación

```html
<p v-if="location" class="font-smaller mb-0">
  <mdb-icon icon="location-arrow"/> {{location}}
</p>
```

Lo que resuelve nuestro problema:
![Componentes](/MDBVue/img/vista5.png "Componentes")

Nota:
mientras usamos la directiva v-if, no usamos la sintaxis de llaves dobles, sino que proporcionamos directamente el nombre de la variable entre comillas dobles ("").

### v-else

A veces, en lugar de ocultar elementos vacíos, es posible que desee mostrar algún contenido alternativo (es decir, si no hay una imagen en la publicación de su blog, es posible que desee mostrar una predeterminada).

En Vue, puede hacer esto fácilmente usando la directiva v-else:
```html
<template>
  <div>
    <p v-if="false">When v-if is true I am visible</p>
    <p v-else>But when it's false, it's my airtime!</p>
  </div>
</template>
```

![Componentes](/MDBVue/img/velse.gif "Componentes")

### v-else-if

También puede terminar en una situación en la que un "si" no es suficiente. Es decir. Imagine que tiene una lista de autores y desea representar un avatar diferente para cada autor, pero aún así, si el autor es anónimo, desea mostrar un avatar predeterminado.

Nuevamente en Vue, puede lograrlo fácilmente usando la directiva v-else-if:
```html
<template>
  <div>
    <div v-if="true">A</div>
    <div v-else-if="false">B</div>
    <div v-else-if="false">C</div>
    <div v-else>Not A/B/C</div>
  </div>
</template>
```

![Componentes](/MDBVue/img/velse3.gif "Componentes")

## Pasar funciones entre componentes

Como sabemos cómo renderizar eventos dinámicamente a partir de una matriz, démosle forma a nuestra aplicación. Actualmente, nuestro componente Evento muestra un icono que supuestamente elimina el elemento de la lista. Como ya sabe, la lista de eventos se encuentra en el componente principal: Aplicación. Por lo tanto, no es posible que el componente Event se elimine a sí mismo. Para eliminar un evento de una lista, tenemos que pasar una llamada del componente secundario (Evento) a su componente principal (Aplicación). En esta lección, aprenderemos cómo pasar llamadas a funciones entre componentes.

Antes de comenzar con eso, primero trabajemos en la plantilla de la aplicación:

### Style

Ya no necesitamos estilos, así que eliminémoslos de los archivos App.vue y Event.vue.

### Template

Actualicemos la plantilla de nuestra aplicación:
```html
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <h2 class="text-uppercase my-3">Today:</h2>
        <Event
          v-for="(event, index) in events"
          :index="index"
          :time="event.time"
          :title="event.title"
          :location="event.location"
          :description="event.description"
          :key="index"
        />
        <mdb-row>
          <mdb-col xl="3" md="6" class="mx-auto text-center">
            <mdb-btn color="info">Add Event</mdb-btn>
          </mdb-col>
        </mdb-row>
      </mdb-col>
      <mdb-col col="3">
        <h3 class="text-uppercase my-3">Schedule</h3>
        <h6 class="my-3">
          It's going to be busy that today. You have
          <b>{{events.length}} events</b> today.
        </h6>
        <h1 class="my-3">
          <mdb-row>
            <mdb-col col="3" class="text-center">
              <mdb-icon far icon="sun"/>
            </mdb-col>
            <mdb-col col="9">Sunny</mdb-col>
          </mdb-row>
          <mdb-row>
            <mdb-col col="3" class="text-center">
              <mdb-icon icon="thermometer-three-quarters"/>
            </mdb-col>
            <mdb-col col="9">23°C</mdb-col>
          </mdb-row>
        </h1>
        <p>
          Don't forget your sunglasses. Today will dry and sunny, becoming
          warm in the afternoon with temperatures of between 20 and 25
          degrees.
        </p>
      </mdb-col>
    </mdb-row>
  </mdb-container>
</template>
```

### Script

Finalmente, importemos algunos componentes faltantes como **mdbButton** y **mdbIcon** de la biblioteca:

```html
import { mdbContainer, mdbRow, mdbCol, mdbIcon, mdbBtn, } from "mdbvue";
[...]
  components: {
    mdbContainer,
    mdbRow,
    mdbCol,
    Event,
    mdbIcon,
    mdbBtn,
  },
```
![Componentes](/MDBVue/img/vista6.png "Componentes")

En la columna de la derecha, agregamos un contador de eventos que nos dice cuántos eventos están programados para hoy. Para obtener este número, simplemente contamos elementos dentro de la matriz usando la función **events.length**.

### Función Eliminar evento

Ahora podemos crear una función que elimine un evento en particular de una matriz. Es una función muy simple que acepta el id del evento que queremos eliminar y lo elimina de la matriz[] usando la función splice():

```html
handleDelete(eventIndex) {
  this.events.splice(eventIndex, 1);
}
```

En Vue definimos nuevas funciones dentro de los **métodos: { }** dentro de la **función export { }**.

```html
export default {
  name: "App",
  components: {
	[...]
  },
  data() {
	[...]
  },
  methods: {
    handleDelete(eventIndex) {
      this.events.splice(eventIndex, 1);
    }
  }
};
```
Ahora, cuando nuestra función esté lista, podemos emitir un evento desde el componente Event.

### Emisión de eventos

Definamos un controlador de eventos para nuestro componente de eventos:
```html
methods: {
  onDelete() {
    this.$emit('delete', this.index);
  }
}
```

Esta función emitirá un evento con dos parámetros:

- nombre del evento - en nuestro caso eliminar
- payload - en nuestro caso será el id del evento correspondiente

Ahora tenemos que activarlo cada vez que los usuarios hagan clic en el icono de eliminar. Por lo tanto, actualice nuestra etiqueta existente: **mdb-badge**.

```html
<mdb-badge tag="a" color="danger-color" class="ml-2 float-right">-</mdb-badge>
```
con una llamada de función @click.native.

```html
<mdb-badge @click.native="onDelete" tag="a" color="danger-color" class="ml-2 float-right">-</mdb-badge>
```
Ahora, cada vez que un usuario haga clic en el icono de eliminación, junto al título del evento, emitirá un nuevo evento de eliminación con la identificación del evento en el que se hizo clic.

### Captura y manejo de eventos

Lo último que tenemos que hacer es capturar el evento emitido en App.ve. Para hacer eso, actualice nuestra invocación de eventos dentro de la plantilla de la aplicación:

```html
<Event
  v-for="(event, index) in events"
  :index="index"
  :time="event.time"
  :title="event.title"
  :location="event.location"
  :description="event.description"
  :key="index"
  @delete="handleDelete"
/>
```

Como notó, hemos agregado un identificador @delete y lo hemos vinculado a la función handleDelete que se creó en el paso 2.

![Componentes](/MDBVue/img/vista7.gif "Componentes")

Ahora, siempre que hagamos clic en un icono de eliminación:

- El usuario hace clic en el icono de eliminar
- @click.native llama a la función onDelete ()
- onDelete () emite un evento
- El componente de la aplicación lo detecta y activa

![Componentes](/MDBVue/img/vista8.jpg "Componentes")

Finalmente, Vue se da cuenta automáticamente de que el modelo de datos ha cambiado y vuelve a mostrar la lista de eventos.

## Modales

En esta lección, aprenderemos cómo usar modales en nuestros proyectos.

Nota:
Los modales son pequeñas ventanas emergentes que son realmente útiles cuando desea mostrar al usuario contenido adicional, configuración o solicitud de consentimiento.

Para usar modales, siga los pasos a continuación.

### Importe los elementos modales en App.vue
```html
import {
  ...
  mdbModal,
  mdbModalHeader,
  mdbModalTitle,
  mdbModalBody,
  mdbModalFooter
} from "mdbvue";

[...]
export default {
  name: "App",
	components: {
	...
	mdbModal,
	mdbModalHeader,
	mdbModalTitle,
	mdbModalBody,
	mdbModalFooter,
	...
	},
  ```
Agregue la nueva variable **modal: false** dentro de la función **data()**
```html
data() {
    return {
      events: [
...
      ],
      modal: false,
    };
  },
```
Agregue el cuerpo modal a la plantilla antes de **mdb-container**
```html
<mdb-modal v-if="modal" @close="modal = false">
  <mdb-modal-header>
    <mdb-modal-title tag="h4" class="w-100 text-center font-weight-bold">Add new event</mdb-modal-title>
  </mdb-modal-header>
  <mdb-modal-body>Modal body</mdb-modal-body>
  <mdb-modal-footer class="justify-content-center">
    <mdb-btn color="info">Add</mdb-btn>
  </mdb-modal-footer>
</mdb-modal>
```
Actualice el botón Agregar evento para mostrar modal al hacer clic (actualice el valor modal a verdadero)
```html
<mdb-btn color="info" @click.native="modal = true">Add Event</mdb-btn>
```
El contenido final del archivo será:
```html
<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="9">
        <h2 class="text-uppercase my-3">Today:</h2>
        <Event
          v-for="(event, index) in events"
          :index="index"
          :time="event.time"
          :title="event.title"
          :location="event.location"
          :description="event.description"
          :key="index"
          @delete="handleDelete"
        />
        <mdb-row>
          <mdb-col xl="3" md="6" class="mx-auto text-center">
            <mdb-btn color="info" @click.native="modal = true">Add Event</mdb-btn>
          </mdb-col>
        </mdb-row>
      </mdb-col>
      <mdb-col col="3">
        <h3 class="text-uppercase my-3">Schedule</h3>
        <h6 class="my-3">
          It's going to be busy that today. You have
          <b>{{events.length}} events</b> today.
        </h6>
        <h1 class="my-3">
          <mdb-row>
            <mdb-col col="3" class="text-center">
              <mdb-icon far icon="sun"/>
            </mdb-col>
            <mdb-col col="9">Sunny</mdb-col>
          </mdb-row>
          <mdb-row>
            <mdb-col col="3" class="text-center">
              <mdb-icon icon="thermometer-three-quarters"/>
            </mdb-col>
            <mdb-col col="9">23°C</mdb-col>
          </mdb-row>
        </h1>
        <p>
          Don't forget your sunglasses. Today will dry and sunny, becoming
          warm in the afternoon with temperatures of between 20 and 25
          degrees.
        </p>
      </mdb-col>
    </mdb-row>

    <mdb-modal v-if="modal" @close="modal = false">
      <mdb-modal-header>
        <mdb-modal-title tag="h4" class="w-100 text-center font-weight-bold">Add new event</mdb-modal-title>
      </mdb-modal-header>
      <mdb-modal-body>Modal body</mdb-modal-body>
      <mdb-modal-footer class="justify-content-center">
        <mdb-btn color="info">Add</mdb-btn>
      </mdb-modal-footer>
    </mdb-modal>
  </mdb-container>
</template>

<script>
import {
  mdbContainer,
  mdbRow,
  mdbCol,
  mdbIcon,
  mdbBtn,
  mdbModal,
  mdbModalHeader,
  mdbModalTitle,
  mdbModalBody,
  mdbModalFooter
} from "mdbvue";
import Event from "@/components/Event";
export default {
  name: "App",
  components: {
    mdbContainer,
    mdbRow,
    mdbCol,
    mdbIcon,
    mdbBtn,
    mdbModal,
    mdbModalHeader,
    mdbModalTitle,
    mdbModalBody,
    mdbModalFooter,
    Event
  },
  data() {
    return {
      events: [
        {
          time: "10:00",
          title: "Breakfast with Simon",
          location: "Lounge Caffe",
          description: "Discuss Q3 targets"
        },
        {
          time: "10:30",
          title: "Daily Standup Meeting (recurring)",
          location: "Warsaw Spire Office"
        },
        {
          time: "11:00",
          title: "Call with HRs"
        },
        {
          time: "12:00",
          title: "Lunch with Timmoty",
          location: "Canteen",
          description: "Project evalutation "
        }
      ],
      modal: false
    };
  },
  methods: {
    handleDelete(eventIndex) {
      this.events.splice(eventIndex, 1);
    }
  }
};
</script>

<style>
</style>
```
Guarde el archivo y ejecute la aplicación:

![Componentes](/MDBVue/img/vista9.gif "Componentes")

Como puede ver, podemos cerrar ese modal haciendo clic en el signo X en la esquina superior derecha o simplemente haciendo clic en cualquier lugar fuera del modal.

Nota:
Los modales son excelentes herramientas y se pueden personalizar de varias maneras. Lo más probable es que los utilice para obtener el consentimiento del usuario para el uso de cookies o para aceptar una política de privacidad en un sitio web, mostrar un formulario de registro/inicio de sesión o algunos detalles adicionales como un mapa y/o formulario de contacto.

Puede encontrar docenas de plantillas modales predefinidas.

En esta lección, aprenderemos cómo manejar las entradas. Las entradas son componentes muy utilizados en el desarrollo web. Los usamos en todas partes, comenzando con el formulario de inicio de sesión, los formularios de suscripción al boletín, un nuevo editor de publicaciones hasta los paneles de configuración, etc.

Simplemente siga los pasos a continuación.

### Importar mdbInput y mdbTextarea a App.vue
```html
import {
  ...
  mdbInput,
  mdbTextarea
} from "mdbvue";

[...]
export default {
  name: "App",
	  components: {
	    ...
	    mdbInput,
	    mdbTextarea,
	    ...
	  },
```
### Añadir un nuevo array newValues[] a data()
```html
data() {
  return {
    events: [
		...
    ],
    modal: false,
    newValues: []
  };
},
```
### Reemplace el cuerpo modal con el siguiente código
```html
<mdb-modal-body>
  <form class="mx-3 grey-text">
    <mdb-input
      name="time"
      label="Time"
      icon="clock"
      placeholder="12:30"
      type="text"
      @input="handleInput($event, 'time')"
    />
    <mdb-input
      name="title"
      label="Title"
      icon="edit"
      placeholder="Briefing"
      type="text"
      @input="handleInput($event, 'title')"
    />
    <mdb-input
      name="location"
      label="Location (optional)"
      icon="map"
      type="text"
      @input="handleInput($event, 'location')"
    />
    <mdb-textarea
      name="description"
      label="Description (optional)"
      icon="sticky-note"
      @input="handleInput($event, 'description')"
    />
  </form>
</mdb-modal-body>
```
Agregar la función handleInput
```html
handleInput(val, type) {
  this.newValues[type] = val;
  console.log(this.newValues);
},
```
Veamos cómo funcionan las funciones handleInput(). Imprimamos para consolar nuestro objeto de estado cada vez que se llame a esta función. Para hacer esto, agregamos console.log (this.newValues); al cuerpo de la función:
```html
handleInput(val, type) {
  this.newValues[type] = val;
  console.log(this.newValues);
},
```
![Componentes](/MDBVue/img/vista10.gif "Componentes")

Cada vez que escribimos algo dentro de la entrada, su valor se agrega a la variable newValues ​​para que podamos usarlo cuando se envía nuestro formulario.

Ya que tenemos todas nuestras entradas manejadas, eliminemos console.log () y pasemos a los pasos finales: enviar el formulario y agregar el nuevo elemento a la lista.

### Agregue la función addEvent()
```html
addEvent() {
  this.events.push({
    time: this.newValues["time"],
    title: this.newValues["title"],
    location: this.newValues["location"],
    description: this.newValues["description"]
  });
}
```
### Agregue @ click.native = "addEvent"

Al botón Agregar en el pie de página modal:
```html
<mdb-btn color="info" @click.native="addEvent">Add</mdb-btn>
```
Ahora, cada vez que hagamos clic en el botón Agregar, activaremos la función addEvent() que agregará un nuevo elemento a la matriz de eventos usando push().

Nota:
No tenemos que agregar una identificación al nuevo evento nosotros mismos. Nuestro bucle v-for lo hará por nosotros automáticamente.

![Componentes](/MDBVue/img/vista11.gif "Componentes")

Resumen
Nuestra aplicación está lista, sin embargo, todavía hay algunas cosas más que debemos hacer:
- cambie el tipo de cualquier propiedad del evento (es decir, cambie la hora a JS Date en lugar de cadena)
- llamar a una API externa para obtener una previsión meteorológica real
- agregue validación a nuestro formulario (asegúrese de que el usuario complete todos los datos obligatorios)
- manejar duplicados