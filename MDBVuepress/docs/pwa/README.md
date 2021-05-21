# PWA aplicación progresiva web

Web oficial [MDB Vue](https://mdbootstrap.com/education/vue/getting-started/)

## Tutorial de la aplicación web progresiva Vue

Recientemente, Google, uno de los mayores defensores de PWA, lanzó Chrome 72 para Android, que admite la tan esperada API TWA (Trusted Web Activity). Significa que a partir de ahora, PWA se puede distribuir a través de Google Play Store.

Gracias a la firme defensa de PWA por parte de Google, es seguro decir que PWA es el futuro de las aplicaciones web. En esta serie de tutoriales, explicaremos los conceptos básicos involucrados en la creación de aplicaciones PWA con Vue.

¿Qué estamos construyendo?
En este tutorial, transformaremos nuestra aplicación Agenda creada desde cero en el tutorial básico en una aplicación web progresiva completa. Lección tras lección, vamos a agregar nuevas funcionalidades: el objetivo de mejorar la experiencia del usuario de manera progresiva es, de hecho, la esencia de PWA. Todas las técnicas aprendidas aquí se pueden aplicar con éxito en sus propios proyectos. Tenga en cuenta que cualquier aplicación web o sitio web se puede convertir en una PWA; ¡hacerlo con las aplicaciones Vue existentes es un desafío que vale la pena!

¿Qué vamos a aprender?

- Implementar tu proyecto en Firebase
- Hacer que su aplicación sea instalable
- Agregar soporte de modo fuera de línea a su aplicación
- Trabajar sin conexión
- Disminuir el tiempo de carga
- Agregar soporte para dispositivos iOS
- Auditar el rendimiento de su aplicación y qué es un Faro

## Implementando en Firebase

En este punto, tenemos una aplicación pequeña y atractiva. Pero para ser honesto, no es muy útil. Es solo una interfaz: no podemos almacenar nuevas citas y los nuevos datos no persistirán si actualiza la página. La aplicación también está disponible solo en nuestro entorno local.

Ciertamente, necesitamos alguna solución de backend, pero por otro lado estamos aquí para aprender sobre los conceptos de PWA, no para preocuparnos por servidores, API o almacenes de datos.

En este tutorial, usaremos la plataforma Firebase, una solución proporcionada por Google. Nos permite mantener nuestro código de backend lo más simple posible y centrarnos en lo que realmente importa en la creación de aplicaciones web progresivas.

Para aprovechar los beneficios que brinda Firebase, debemos configurar un nuevo proyecto allí. Para hacer esto, abra la siguiente dirección en su navegador web.

Después de iniciar sesión en el servicio de Firebase, debe crear una nueva aplicación. Para hacer esto, haga clic en el botón "Agregar proyecto" y siga el breve formulario en el que proporcionará el nombre de su proyecto y alguna otra información requerida.

Ahora, para poder comunicarnos con el backend de Firebase, necesitamos **instalar firebase-tools** globalmente en nuestra máquina. Abra la línea de **comando/terminal** y escriba:

```bash
yarn global add firebase-tools
```
Después de eso, inicie sesión en el servicio de Firebase con una cuenta de Google:

```bash
firebase login
```
En caso de que su terminal no reconozca el comando firebase, podría ser necesario mejorar la variable ambiental $PATH con el directorio bin global yarn. En caso de que lo haga, recuerde reiniciar la sesión de terminal.

A continuación, debemos inicializar nuestra aplicación como un proyecto de Firebase. En el directorio raíz del proyecto, ejecute:

```bash
firebase init
```
La consola le hará algunas preguntas; respóndalas de acuerdo con la guía a continuación:

- ¿Qué funciones de Firebase CLI desea configurar para esta carpeta? Por ahora, seleccionemos Hosting. Siempre podemos agregar servicios adicionales más adelante en caso de que los necesitemos.
- Seleccione un proyecto de Firebase predeterminado para este directorio Elija el proyecto existente, el que acaba de crear en la plataforma de Firebase
- ¿Qué desea utilizar como directorio público? - dist - es donde dejará nuestro código de producción (después de la primera compilación)
- ¿Configurar como una aplicación de una sola página (reescribir todas las URL en /index.html)? (y / N) - y

Firebase creará dos archivos: firebase.json, que contiene la configuración de la aplicación, y .firebaserc con los detalles del proyecto de Firebase. Si desea cambiar alguna configuración que definió durante el proceso de inicialización, revísela.

Antes de implementar nuestra aplicación en los servidores de Firebase, prepararemos un pequeño script de nodo que nos ayudará a ahorrarnos algunas pulsaciones de teclas en el futuro. Agregue lo siguiente al archivo package.json:

```html
"scripts": {
"deploy": "yarn build && firebase deploy",
```
A partir de ahora, para implementar una nueva versión de nuestra aplicación, solo necesitamos ejecutar este script. Activa el proceso de compilación, que prepara una versión de producción de nuestra aplicación y luego la envía para que se aloje en Firebase. Ahora, todo lo que nos queda por hacer es publicar nuestra aplicación.

```bash
yarn deploy
```

¡Y eso es! ¡Tu primera aplicación ya está en los servidores de Firebase esperando que la visites en la dirección indicada!

![Componentes](/MDBVue/img/phone.png "Componentes")

## Hacer una aplicación instalable

El manifiesto

Una de las funciones más poderosas que ofrece PWA es la capacidad de agregar nuestra aplicación web a la pantalla de inicio de un dispositivo. Esta característica ayuda a cerrar la brecha entre las aplicaciones web y nativas, lo que permite que estas últimas se vean y se sientan como si fueran "nativas".

Sorprendentemente, no requiere mucho trabajo. Cubriremos todo en las próximas lecciones.

La función se reduce a ofrecer a los usuarios la posibilidad de instalar nuestra aplicación en sus pantallas de inicio. En la práctica, la capacidad toma la forma de un banner de instalación de aplicaciones, un cuadro de diálogo simple del navegador. Según la documentación oficial, solo hay unos pocos requisitos:

- La aplicación web aún no está instalada
- El usuario ha interactuado con el dominio durante al menos 30 segundos
- Codebase incluye un manifiesto de aplicación web
- La aplicación se sirve a través de HTTPS
- Ha registrado un trabajador del servicio con un controlador de eventos de recuperación (actualmente solo es necesario para Chrome en Android)

Por supuesto, los dos primeros están fuera de nuestro control, es el usuario quien decide durante cuánto tiempo se llevan a cabo las interacciones o qué está o no instalado en sus pantallas de inicio. Como desarrolladores, tenemos que ocuparnos de los tres últimos. Comencemos con el manifiesto de la aplicación.

### ¿Qué es un manifiesto de aplicación?

El manifiesto de la aplicación es un archivo JSON simple que describe algunas de las propiedades que la aplicación debe haber definido para utilizar el encanto de PWA. En eso, el archivo se parece a una descripción de App Store de una aplicación nativa. Es posible que ya se haya encontrado con él en algunos directorios de otros proyectos, especialmente los creados con los sistemas estándar vue-cli o create-react-app.

El archivo manifest.json mínimo consta de estos campos:

- name y short_name - cualquiera. Si se proporcionan ambos, short_name se usa en la pantalla de inicio del usuario, el iniciador u otros lugares donde el espacio puede ser limitado. El nombre se utiliza en el indicador de instalación de la aplicación.

- iconos: una serie de objetos que representan imágenes, cada uno con sus campos src, tamaño y tipo. Debe incluir íconos de tamaño 192px y 512px. Chrome escalará automáticamente el icono del dispositivo. Si prefiere escalar sus propios íconos y ajustarlos para la perfección de píxeles, proporcione íconos en incrementos de 48dp

- start_url: le dice al navegador dónde debe comenzar su aplicación cuando se inicia y evita que la aplicación se inicie en cualquier página en la que estuviera el usuario cuando agregó su aplicación a su pantalla de inicio. Agregue una cadena de consulta al final de start_url para realizar un seguimiento de la frecuencia con la que se inicia su aplicación instalada

public/manifest.json

```bash
"start_url": "/?utm_source=homescreen",
```
- monitor debe ser uno de los siguientes: pantalla completa, independiente o interfaz de usuario mínima. Cambia la cantidad de interfaz de usuario del navegador que se muestra al usuario y puede variar desde "navegador" (cuando se muestra la ventana completa del navegador) hasta "pantalla completa" (cuando la aplicación está en pantalla completa).

Aquí encontrará una lista completa de las propiedades admitidas que se pueden utilizar.

Ahora, armados con los antecedentes teóricos necesarios, ¡creemos un archivo manifest.json por nuestra cuenta! Pongámoslo en el directorio / static del proyecto, dando a entender que queremos mantenerlo como está (estático), en lugar de tenerlo procesado de alguna manera, como hacemos con los archivos .vue, por ejemplo. Si lo hace allí, significa que ya no necesitamos el archivo .gitkeep, que solo garantizaba que el sistema de control considerara la carpeta para que podamos deshacernos de ella libremente.

static/manifest.json

```bash
{
"short_name": "Agenda App",
"name": "MDBootstrap Agenda App Demo",
"icons": [
{
"src": "icons/Icon-36.png",
"sizes": "36x36",
"type": "image/png"
},
{
"src": "icons/Icon-48.png",
"sizes": "48x48",
"type": "image/png"
},
{
"src": "icons/Icon-72.png",
"sizes": "72x72",
"type": "image/png"
},
{
"src": "icons/Icon-96.png",
"sizes": "96x96",
"type": "image/png"
},
{
"src": "icons/Icon-120.png",
"sizes": "120x120",
"type": "image/png"
},
{
"src": "icons/Icon-144.png",
"sizes": "144x144",
"type": "image/png"
},
{
"src": "icons/Icon-152.png",
"sizes": "152x152",
"type": "image/png"
},
{
"src": "icons/Icon-167.png",
"sizes": "167x167",
"type": "image/png"
},
{
"src": "icons/Icon-180.png",
"sizes": "180x180",
"type": "image/png"
},
{
"src": "icons/Icon-192.png",
"sizes": "192x192",
"type": "image/png"
},
{
"src": "icons/Icon-512.png",
"sizes": "512x512",
"type": "image/png"
},
{
"src": "icons/Icon-1024.png",
"sizes": "1024x1024",
"type": "image/png"
}
],
"start_url": "./index.html",
"display": "standalone",
"background_color": "#000000",
"theme_color": "#4DBA87"
}
```
### Iconos

Como puede ver, el archivo JSON hace referencia a una serie de imágenes de diferentes tamaños que aún no tenemos. ¿Por qué los incluimos a todos? Hay un mundo de pantallas esperando a que se instale nuestra PWA, la mayoría de ellas tienen su propio tamaño y densidad de píxeles. Poder cubrir eso significa preparar estas imágenes por adelantado e incluirlas en nuestro manifiesto web. Deje que la aplicación haga el resto con respecto a elegir una imagen adecuada para su visualización.

¡Nota!

Todos los diferentes tamaños se mencionan para ilustrar los posibles propósitos de cobertura de píxeles perfectos, que superan más allá de un ejemplo mínimo manifiesto. Para satisfacer la auditoría de Lighthouse PWA, que es un punto de referencia importante 192x192 to 512x512px sized icons.

Analicemos la lista. Los iconos de 36x36, 48x48, 72x72, 96x96, 144x144 y 192x192 píxeles están optimizados para su uso en la plataforma Android; La gente de Google se aseguró de que estos tamaños funcionen bien sin que se vean pixelados o borrosos. Para cubrir todas las necesidades de Andoroid, lo que también necesitamos es un 512x512, que se usa para las pantallas de bienvenida.

Los tamaños de los iconos para la plataforma iOS son: 120x120, 152x152, 167x167 y 180x180 píxeles. Es el último que cubre las pautas de iOS más actuales; los demás se incluyen por razones de compatibilidad con versiones anteriores. El conjunto de dimensiones finales que puede necesitar, 1024x1024, se utiliza en la App Store de Apple. Aunque no muchas PWA terminan allí, un icono cuadrado grande puede resultar útil, ya que la plataforma siempre puede reducirlo.

¿Le gustaría tener su propio icono, pero no sabe por dónde empezar? Cuando se trata del diseño mismo, se deben seguir las mismas mejores prácticas para cualquier ícono de Android. En caso de que ya tenga un ícono para una aplicación, pero cree que crear todos estos archivos diferentes manualmente es un arrastre, ¡tiene toda la razón! Hay muchas herramientas en línea para crear los conjuntos de iconos necesarios para PWA; es posible que desee investigar su favorito o dejar que le proporcionemos un código postal para que no tenga que hacerlo. Agregue la carpeta (descomprimida) / icons en el directorio static /.

### Vinculando el manifiesto

Ya casi terminamos aquí; lo único que queda es dejar que el navegador sepa que el manifiesto está a bordo incluyéndolo en la etiqueta **head** de nuestro archivo ./index.html en el directorio raíz del proyecto.

./index.html

```html
<head>
  <meta charset="utf-8">
  ...
  <link rel="manifest" href="/static/manifest.json" />
</head>
```
Después de ejecutar yarn build o yarn deploy, debería ver que la copia de la carpeta/static se inculca dentro del directorio/dist. Tener la estructura del archivo copiada significa que la ruta relativa, incrustada explícitamente en nuestro HTML, permanece igual, mientras que el resto del proyecto se prepara para una implementación.

Para ver si todo funciona, simplemente escribe dev el proyecto y consulta la consola de devtools (F12 o Ctrl + Shift + J según el navegador utilizado). Todos los posibles problemas con la disponibilidad de los íconos, causados ​​por un nombre incorrecto de archivo o un formato JSON incorrecto, deben enumerarse allí. Para un enfoque más holístico para probar la preparación para PWA de nuestra aplicación, quédese con nosotros para una próxima lección sobre la auditoría.
sobre eso en la próxima lección) es posible que desee comenzar poco a poco. En ese caso, sugiero comenzar con iconos de 192x192px y 512x512px.

Analicemos la lista. Los iconos de 36x36, 48x48, 72x72, 96x96, 144x144 y 192x192 píxeles están optimizados para su uso en la plataforma Android; La gente de Google se aseguró de que estos tamaños funcionen bien sin que se vean pixelados o borrosos. Para cubrir todas las necesidades de Andoroid, lo que también necesitamos es un 512x512, que se usa para las pantallas de bienvenida.

Los tamaños de los iconos para la plataforma iOS son: 120x120, 152x152, 167x167 y 180x180 píxeles. Es el último que cubre las pautas de iOS más actuales; los demás se incluyen por razones de compatibilidad con versiones anteriores. El conjunto de dimensiones finales que puede necesitar, 1024x1024, se utiliza en la App Store de Apple. Aunque no muchas PWA terminan allí, un icono cuadrado grande puede resultar útil, ya que la plataforma siempre puede reducirlo.

¿Le gustaría tener su propio icono, pero no sabe por dónde empezar? Cuando se trata del diseño mismo, se deben seguir las mismas mejores prácticas para cualquier ícono de Android. En caso de que ya tenga un ícono para una aplicación, pero cree que crear todos estos archivos diferentes manualmente es un arrastre, ¡tiene toda la razón! Hay muchas herramientas en línea para crear los conjuntos de iconos necesarios para PWA; es posible que desee investigar su favorito o dejar que le proporcionemos un código postal para que no tenga que hacerlo. Agregue la carpeta (descomprimida) / icons en el directorio static /.

### HTTPS y trabajadores de servicios

Hablemos de los dos últimos requisitos que debemos cumplir antes de que nuestra aplicación se vuelva instalable: tenerla servida a través de https y hacerlo acompañado por un trabajador del servicio.

### ¿Qué es un trabajador de servicios?

Un trabajador de servicios es un tipo de trabajador web, un concepto interesante que ha abierto a los navegadores modernos al mundo de las experiencias fuera de línea. ¿Cómo? En resumen, es un script que se ejecuta en un hilo separado, independientemente del sitio web.

### ¿Por qué cambia tanto las reglas del juego?

Durante los primeros pasos en el mundo de JavaScript, probablemente vio cómo los bucles infinitos en el código afectaban a la interfaz de usuario, es decir, las funcionalidades de ejecución de JS se volvieron irreflexivas. Bloquearon "el hilo principal" y luego la interfaz se congeló.

Todo se debe a que JavaScript es un lenguaje de un solo hilo. Significa que tiene una pila de llamadas y un montón de memoria, ejecutando el código en orden. Cuando las cosas van hacia el sur (como bucles que se repiten sin cesar o solicitudes sincrónicas que tardan demasiado), todo se derrumba, lo que hace que la interfaz de usuario no responda.

La idea de un trabajador web es emocionante, porque tiene su propio hilo y, por lo tanto, puede realizar tareas pesadas, que de otro modo reducirían el rendimiento de su sitio web, en segundo plano. Además, de acuerdo con la especificación, los trabajadores pueden estar trabajando durante mucho tiempo después de que se haya cerrado la ventana responsable de su desove (aunque no puede tomar más tareas nuevas).

"De acuerdo", alguien podría decir, "entonces un trabajador de servicios es como una potencia de cálculo adicional, pero la obtenemos de todos modos, a medida que avanza la tecnología de hardware". Los trabajadores serían un impulso considerable sin una dirección clara, si no fuera por una implicación arquitectónica: se sientan entre el navegador y la red y actúan como servidores proxy. Significa que no solo obtienen su propio hilo para el cálculo, sino que también lo hacen mientras se encuentran entre el cliente y el servidor. Pueden interceptar las solicitudes realizadas por el sitio web y redirigirlas a la caché. Imagínese, hay una solicitud saliente de constent y toda la respuesta se guarda en la memoria del navegador a lo largo del camino, eliminando la necesidad de solicitarla nuevamente. La próxima vez que intentemos acceder a algunos de ellos, no se necesita conexión a Internet.

Aquí es donde brilla el trabajador del servicio, como vehículo para el almacenamiento en caché y la gestión de datos. Puede proporcionarnos acceso al contenido y funciones como notificaciones automáticas, incluso en caso de que se pierda la conexión a Internet.

Sin embargo, este tipo de poder tiene un precio. Esta es una nueva tecnología, por lo que solo es compatible con los navegadores modernos. Además, como los trabajadores del servicio pueden ser fácilmente utilizados de forma incorrecta, su uso está restringido a ejecutar HTTPS por razones de seguridad. Aunque durante el desarrollo podremos utilizar un service worker a través de localhost, para implementarlo en un sitio, será necesaria una configuración HTTPS en su servidor.

Los trabajadores web son un tema enorme en sí mismos y aquí abordaré sus elementos esenciales básicos basados ​​en código en el contexto de PWA, es decir, cómo registrar uno, en qué consiste generalmente y algunos consejos sobre el desarrollo. Hay algunos materiales extensos sobre el tema preparados por los líderes de la industria; asegúrese de consultarlos para una inmersión profunda.

Service Worker es un código JavaScript que vive en un hilo diferente al principal del navegador, donde reside la interfaz de usuario y el resto del JS. De esa manera, es muy especial: para que los navegadores puedan utilizarlo adecuadamente, debe estar registrado correctamente (más sobre eso a continuación) y definido en un archivo separado. Desde allí, no tiene acceso directo a las API web de reglar, como el objeto global de ventana.

Lo que hace el trabajador, en la práctica, es reaccionar a ciertos eventos; es por eso que el archivo en sí consiste principalmente en devoluciones de llamada. Para referirse a sí mismo desde el trabajador para agregar detectores de eventos, la API expone una variable propia, lo que la hace un poco más confiable para los principiantes que la normal, vinculada dinámicamente.

./service-worker.js

```html
const cacheVersion = 'v1';

self.addEventListener('install', function (event) {
  self.skipWaiting(); // We do it for our worker not to wait until the previous one expires

  event.waitUntil(
    caches.open(cacheVersion).then(function (cache) {
      return cache.addAll([
        './index.html'
      ]);
    })
  );
});

self.addEventListener('fetch', function (event) {
  event.respondWith(caches.match(event.request).then(function (response) {
    // caches.match() always resolves
    // but in case of success response will have value
    if (response !== undefined) {
      return response;
    } else {
      return fetch(event.request).then(function (response) {
        // response may be used only once
        // we need to save clone to put one copy in cache
        // and serve second one
        let responseClone = response.clone();

        caches.open(cacheVersion).then(function (cache) {
          cache.put(event.request, responseClone);
        });
        return response;
      }).catch(function () {
        return console.log('no cache response!');
      });
    }
  }));
});
```
Como puede ver, el trabajador de arriba escucha dos eventos.

El evento de instalación brinda la oportunidad de definir la política de almacenamiento en caché. Primero, llamamos self.skipWaiting (); para que nuestro Trabajador se ponga manos a la obra al instante. Luego, abre una caché particular (llamada v1) y usa el método cache.addAll () para agregar una lista de archivos específicos allí. A partir de ahora, solo almacenamos en caché ./index.html.

Lo que hemos almacenado, en última instancia, queremos recuperarlo. Para definir la forma en que sucede, se utilizan la devolución de llamada del evento de recuperación y el método .respondWith () de la respuesta. En caso de que ya exista un recurso almacenado para una solicitud específica (caches.match (event.request)) y no esté indefinido, lo recuperamos y lo pasamos como el valor de retorno de la recuperación. En caso de que de hecho esté indefinido (por lo tanto, no almacenado), hacemos una solicitud a la web, pero inmediatamente vuelve, lo clonamos, para que podamos almacenarlo en la caché y servirlo como respuesta.

Con este script de trabajo almacenamos en caché lo que necesitamos, y una vez que el cliente se comunica con nosotros en una visita posterior, los recursos que solicitó en el pasado van directamente del caché, y en caso de que nos pidan algo que no tenemos. almacenado todavía, lo buscamos para servirlo y almacenarlo para más tarde.

El registro

Para que el archivo anterior se considere un trabajador de servicio y no un archivo JavaScript normal del "mismo hilo", debe registrarse como tal. Podríamos hacerlo en cualquier lugar (incluida una etiqueta de script grosera dentro de HTML), pero lo haremos en el punto de entrada (webpack y JavaScripts), es decir, el archivo src / main.js, escondido debajo de la raíz Vue insance.

./src/main.js

```html
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('https://mdbcdn.b-cdn.net/service-worker.js').then(() => {
    console.log('Service Worker registered!');
});
} else {
  console.log('Service Worker not supported :(');
}
```

En la práctica de webdev, valdría la pena asegurarse de que lo que hacemos arriba, que todavía tiene lugar en el hilo principal, no nos impida representar lo que queremos mostrar. Podríamos lograrlo simplemente envolviendo el registro en la devolución de llamada del evento de carga de la ventana. Afortunadamente, sin embargo, el código transpilado del paquete web se está inyectando al final de la etiqueta **body**, lo que es suficiente como garantía en nuestro caso de uso.

### Flujo de trabajo twaeks

Con los pasos del ciclo de vida de registro e instalación contabilizados, tenemos todos los ingredientes principales para una implementación exitosa del trabajador de servicio. Podríamos encontrar un problema en la consola con respecto al tipo MIME del archivo; aparentemente, service-worker.js no es JS. Además, al hacer clic en el enlace al archivo en la descripción del error, se abrirá una nueva pestaña con nuestro proyecto. ¿Lo que está sucediendo?

La razón de este comportamiento extraño radica en la configuración de nuestro paquete web. Comencemos con el hecho de que webpack-dev-server, una parte crucial de nuestro entorno de desarrollo, no está informado de que hay un recurso JavaScript que debería estar disponible desde nuestro servidor web en ejecución y, lo que es aún más sorprendente, no debe ser procesado. por el empaquetador. Esta indisponibilidad da como resultado el desconcertante efecto secundario: cuando intentamos abrir el archivo, nos redirigen a nuestro ./index.html, por lo que una nueva pestaña localhost dentro del navegador. No es de extrañar que los tipos MIME no cuadren: en esta confusión, terminamos intentando registrar un trabajador de servicio mientras proporcionamos un archivo HTML.

### ¿Como arreglarlo?

Conocemos el directorio / static desde donde están disponibles los recursos para nuestro entorno alojado localmente. Mover ./service-worker.js allí, donde ya reside nuestro manifest.json, parece una buena forma de introducirlo a escondidas. Desafortunadamente, hacer que funcione no será tan sencillo.

Teniendo en cuenta lo poderosa que es una característica del trabajador del servicio, se toman medidas de seguridad contra el uso indebido. Uno de ellos es el alcance del trabajador del servicio, su propiedad que define el alcance exacto dentro de un proyecto. A menudo se asume, con un valor predeterminado de **alcance: '/'**, pero se puede personalizar usando el objeto de opciones pasado como segundo argumento en la función navigator.serviceWorker.register (). ¡Significa que puede tener trabajadores personalizados para diferentes lugares de su proyecto web!

Sin embargo, sea cual sea el alcance asumido o declarado, hay un problema: el trabajador del servicio nunca puede alcanzar "arriba", por lo tanto, fuera del directorio donde se encuentra el archivo. Entonces, ponerlo en la carpeta / static significaría que solo puede almacenar en caché los recursos allí, y nunca, digamos, nuestro ./index.html esperando afuera. En su lugar, tenemos que hacer que la configuración del paquete web reconozca el archivo.

Para hacerlo, necesitamos alterar ligeramente nuestros archivos de configuración de paquete web dev y prod en el directorio / build. En ambos estamos utilizando CopyWebpackPlugin para mover nuestros archivos. Mencionemos a nuestro trabajador de servicio allí; de esa manera, estará disponible tanto desde nuestro servidor de desarrollo como desde el directorio / dist una vez que compilemos.

./build/webpack.dev.config.js
./build/webpack.prod.config.js

```html
new CopyWebpackPlugin([
  {
    from: path.resolve(__dirname, '../static'),
    to: config.dev.assetsSubDirectory,
    ignore: ['.*']
  },
  {
    from: path.resolve(__dirname, '../service-worker.js'),
    to: './service-worker.js',
  }
])
```
¡Y voilá! Ahora estamos dando acceso declarativamente al archivo al servidor de desarrollo del paquete web, mientras que también tenemos el script de trabajo copiado en / dist cuando compilamos. Puede verificar el trabajador que acabamos de registrar en la pestaña Aplicación en Chromium DevTools o en about: debugging # / runtime / this-firefox en Firefox (simplemente copie y pegue la cadena en la barra de direcciones).

### Andamio PWA predeterminado con Vue-CLI

Nuestro propósito aquí era hacer todo por nosotros mismos con fines de aprendizaje, pero en el trabajo diario de los desarrolladores podíamos hacer uso de algunas herramientas ya existentes para el trabajo. Para nuevos proyectos, podríamos preferir intercambiar el control detallado provisto con una configuración de paquete web personalizado, por una solución de andamio lista para usar de Vue-CLI. La solución de línea de comandos nos proporciona menos opciones de configuración "al metal", ya que la aspereza del paquete está oculta debajo de la abstracción de los scripts vue-cli-service, al tiempo que agrega algunos extras útiles: una estructura de proyecto lista para usar y algunas características de desarrollo excelentes. , incluido un excelente complemento de soporte para PWA.

Un proyecto creado de esta manera tendría un script src / registerServiceWorker.js incluido de fábrica. Bajo el capó, utilizaría register-service-worker, un módulo creado por Evan You, autor de Vue, que simplifica el registro de trabajadores y proporciona enlaces para personalizar la reacción de los trabajadores a algunos de los eventos más comunes, como el almacenamiento en caché, encontrar una actualización o desconectarse. . Realmente hace la vida más fácil cuando lo que buscamos es la creación rápida de prototipos.

### Sirviendo a través de HTTPS

Otro requisito para representar el banner de instalación de la aplicación por parte del navegador es tener el proyecto implementado en un dominio certificado por HTTPS, que tenemos gracias a Firebase Hosting. Entonces, actualice nuestra aplicación:

```bash
yarn deploy
```
Ahora, probémoslo. Si abre su sitio web, después de unos segundos debería ver un banner de instalación. Instala la aplicación.

## Trabajando sin conexión

Ahora, vamos a agregar un indicador para informar a nuestros usuarios que se ha perdido la conexión a Internet. Sin embargo, en primer lugar, tengamos una descripción general de lo que se necesita para que funcione el componente. Comencemos con la lógica subyacente.

¿Cómo sabe una aplicación que el flujo digital que la trajo al navegador ya no existe? Con JavaScript es bastante simple. La interfaz del navegador representa el estado y la identidad del agente de usuario. Permite que los scripts lo consulten y se registren para realizar algunas actividades. Para preguntarle si estamos usando Internet para conectarnos a la página actual, intente:

consola

```bash
navigator.onLine
```
Lo anterior devuelve una respuesta booleana, que debería ser falsa para las cosas servidas desde nuestro propio servidor web (localhost). Sin embargo, la solución no es tan útil; no queremos monitorear el estado en cada intervalo dado; de hecho, no nos importa hasta el momento en que cambia. Y cuando finalmente lo haga, queremos reaccionar y ver la notificación. Sí, fue un juego de palabras. Ja ja. ¿Entonces cómo hacemos eso?

Cuando el valor de navigator.onLine cambia, la interfaz de Windows activa un evento, ya sea en línea o fuera de línea. ¡Eso significa que podemos escucharlo!

mi-nuevo-mixtape

```html
window.addEventListener('online', (event) => {
  console.log("You are now connected to the network.");
});
```
Nota

Tenga en cuenta que este evento es intrínsecamente poco confiable. Nuff dijo que una computadora puede estar conectada a una red y no tener acceso a Internet.

¡Es hora de ponerse a trabajar! Comenzaremos preparando nuestro archivo App.vue con una lógica útil. Comencemos agregando detectores de eventos a la interfaz de Windows en el enlace del ciclo de vida creado por la aplicación, para que puedan comenzar a escuchar los eventos en línea / fuera de línea. Lo segundo son las devoluciones de llamada: queremos que nos ayuden a monitorear los cambios en la conexión. Para eso, necesitamos una propiedad de objeto de datos legítima en nuestro componente de aplicación. Lo llamé estado, pero obviamente puedes llamarlo como quieras, siempre que mantengas la coherencia en todo el código.

App.vue

```html
// very bottom of the data object
    status: ''
  };
},
created() {
  window.addEventListener('online', () => {
    this.status = 'online';
  });

  window.addEventListener('offline', () => {
    this.status = 'offline';
  });
},
// component's methods...
```

¡Nuestra aplicación ya puede saber cuándo se desconecta! Puede verificarlo usted mismo primero desconectándose (la pestaña Red de devTools) y luego verificando cómo cambian los datos internos (a través de console.logging la propiedad this.status en la devolución de llamada o observando el cambio de datos real en la extensión Vue panel devTools).

Eso es increíble, pero si bien la aplicación puede decir que algo cambió, nuestros usuarios no pueden. Necesitamos consumir los datos para obtener sus poderes; para pasar el conocimiento del estado al árbol DOM, donde con suerte tendremos un componente para mostrarlo. En realidad, ¿por qué no crear uno ahora?

Vayamos al directorio src / components y creemos un archivo Notificaion.vue (de nuevo, puedes llamarlo Whatever.vue, solo mantenlo). Hay varias razones por las que querríamos tenerlo en un archivo separado: encapsulación, haciendo uso de los elegantes componentes de archivo único .vue, mantener ordenado App.vue son algunas de las más importantes. Puede diseñar esta notificación como desee, es la lógica subyacente lo que es importante. En nuestro ejemplo, usaremos un componente MDBootstrap Alert para las bonitas apariencias y animaciones.

Notification.vue - script tag

```html
<script>
import { mdbAlert } from 'mdbvue';

export default {
  name: "Notification",
  components: {
    mdbAlert
  },
  props: {
    status: String
  },
  data() {
    return {
      availableStatuses: {
        offline: {
          color: 'danger',
          emoji: '☢️',
          text: 'Offline mode engaged.'
        },
        online: {
          color: 'success',
          emoji: '👌',
          text: 'Back online!'
        }
      },
      isShown: false,
    }
  },
  methods: {
    showAlert() {
      this.isShown = true;
      const timeout = setTimeout(this.hideAlert, 3000);
    },
    hideAlert() {
      this.isShown = false;
    }
  },
  watch: {
    status() {
      this.showAlert();
    }
  }
}
</script>;
```

Repasemos el componente juntos. Importamos mdbAlert de la biblioteca mdbvue y lo declaramos como un componente utilizado. A continuación, declaramos la propiedad anticipada, que es la cadena de estado. El objeto de datos tiene dos propiedades: objeto availableStatuses y booleano isShown. El primero se usa para almacenar posibles estados; por ahora, solo está en línea y fuera de línea, pero a medida que su aplicación crece, es posible que sea necesario mejorar el objeto, tal vez incluso importarlo desde un archivo externo.

El otro, isShown, se utilizará para alternar la visibilidad de mdbAlert. No queremos que nuestras notificaciones se muestren todo el tiempo, ¿verdad? Para alternar el booleano usaremos los siguientes métodos showAlert y hideAlert. Para nuestro componente, mostrar significa cambiar el valor de isShown a verdadero y establecer un tiempo de espera de 3000 milisegundos después del cual volverá a ser falso.

Finalmente, el observador. Lo necesitamos para estar atento al cambio de utilería. "¡Pero Vue es inteligente, puede detectar cambios de utilería y actuar sin pedirlo explícitamente!", Escuché una voz proveniente de la segunda fila. Es cierto, Vue puede ver cómo cambian los accesorios para nuestra conveniencia, pero no lo hace (¡y no debería hacerlo!) Para cada parte móvil. Lo hace por sí mismo solo cuando el accesorio parece estar directamente involucrado en el proceso de representación a través de directivas o cuando el cambio puede afectar potencialmente la estructura de datos en el componente, como es el caso de las propiedades calculadas.

La identificación del caso de la notificación es diferente: somos nosotros quienes mandamos. El componente consume los accesorios de estado de forma indirecta, ya que la representación se controla a través de un mecanismo interno setTimout. Vue necesita que se le diga explícitamente que esté atento a los cambios de prop, y una vez que tengan lugar, queremos responder llamando al método this.showAlert (). Eso es todo para nuestra etiqueta de script, ¡la parte más difícil está detrás de nosotros!

Notification.vue - etiqueta de plantilla

```html
<template>
<mdb-alert v-if="isShown" :color="availableStatuses[status].color" enterAnimation="bounceIn"
  leaveAnimation="slideOutRight">
  <span role="img" aria-label="emoji">
    {{availableStatuses[status].emoji}}
  </span>
  {{availableStatuses[status].text}}
</mdb-alert>
</template>
```
Ahora, a partir de la etiqueta tamplete, puede parecer complejo, pero en realidad es bastante sencillo. Como mencionamos, estamos usando mdbAlert como base. Lo más importante a tener en cuenta aquí es la directiva v-if, que nos permite renderizar condicionalmente el componente subyacente dependiendo de si el booleano isShown se evalúa como verdadero en cualquier mensaje dado. En segundo lugar, el atributo de límite de color. mdbAlert normalmente toma un accesorio de color para personalizar su, bueno, color (vea los colores disponibles aquí). Los dos puntos al principio hacen que el prop "enlace", lo que significa que en lugar de pasar un nombre de un color real, el valor del atributo se evaluará como una expresión de JavaScript, de una manera potencialmente dinámica. No sabemos de qué color debería ser mdbAlert, por lo que estamos usando un nombre de propiedad calculado (los corchetes) para acceder al objeto de datos availableStatuses y elegir un color dependiendo del estado en cuestión.

EnterAnimation y leaveAnimation son accesorios que se utilizan para personalizar las animaciones a medida que el componente mdbAlert va y viene. Usé bounceIn y slideOutRight porque creo que se ven geniales, pero siéntase libre de elegir los suyos de la página MDBVue Animations.

Por último, estamos usando el mismo truco de nombre de propiedad calculado para completar los emoji y el texto de las notificaciones.

Notification.vue - etiqueta de estilo

```html
<style scoped>
.alert {
  position: absolute;
  z-index: 2;
  right: 0;

}
.alert span {
  margin-right: 10px;
}
</style>
```

Finalmente, algo de estilo, y más notablemente, la posición: regla absoluta de CSS. Ahora nuestro componente aparecerá en la esquina superior derecha de la pantalla sin distraer el flujo natural de elementos posicionados relativamente. ¡El componente está listo!

App.vue

```html
<template>
<mdb-container>
<Notification :status="status"/>

<script>
import Notification from '@/components/Notification';
...
components: {
  Notification,
  ...
}
```
Solo queda una cosa por hacer: incluir a nuestro bebé en el componente principal App.vue. Vamos a abrir el archivo, importar Notificación desde '@ / components / Notification'; y registrarlo incluyéndolo en el campo de componentes. Agreguémoslo a la etiqueta **template** de App.vue como un elemento secundario directo de mdbContainer. Asegúrese de que nuestra propiedad clave, status, se le pase de forma limitada (dinámica) precediendo el nombre de la propiedad con dos puntos.

Abra su aplicación e intente encender y apagar su conexión a Internet. Debería ver algo similar a esto:

Modo sin conexión Modo en línea
¡Voilà! Implementemos con yarn deploy.

En caso de que las cosas fueran un poco al sur, a continuación se muestra el código completo para App.vue y componens / Notification.vue:

src/App.vue

src/componentsNotification.vue

```html
<template>
  <mdb-container>
    <Notification :status="status"/>
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
      <mdb-modal-footer class="justify-content-center">
        <mdb-btn color="info" @click.native="addEvent">Add</mdb-btn>
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
  mdbModalFooter,
  mdbInput,
  mdbTextarea
} from "mdbvue";
import Event from "@/components/Event";
import Notification from '@/components/Notification';
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
    mdbInput,
    mdbTextarea,
    Event,
    Notification
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
          description: "Project evalutation"
        }
      ],
      modal: false,
      newValues: [],
      status: ''
    };
  },
  created() {
    window.addEventListener('online', () => {
      this.status = 'online';
    });

    window.addEventListener('offline', () => {
      this.status = 'offline';
    });
  },
  methods: {
    handleDelete(eventIndex) {
      this.events.splice(eventIndex, 1);
    },
    handleInput(val, type) {
      this.newValues[type] = val;
    },
    addEvent() {
      this.events.push({
        time: this.newValues["time"],
        title: this.newValues["title"],
        location: this.newValues["location"],
        description: this.newValues["description"]
      });
    }
  }
};
</script>
```
## El modelo de shell de la aplicación

Nuestra aplicación se ve muy bien, pero siempre hay margen de mejora. Si queremos competir con las aplicaciones nativas, tenemos que trabajar en el tiempo de carga de nuestra aplicación.

Si prueba su sitio web en una conexión rápida a Internet, es posible que no haya notado la pantalla en blanco, pero está ahí, más aún cuando un usuario con una señal pésima descarga nuestro archivo JavaScript monolítico.

### Configuración de estrangulamiento

Puede emular conexiones de red lentas desde el menú Network Throttling en las herramientas de desarrollo del navegador Chrome o Firefox.
Es debido a cómo funcionan las aplicaciones modernas de una sola página: en la primera carga, servimos el archivo index.html, que por ahora es solo una página en blanco con div # app. A continuación, se descarga el paquete JS y luego tiene que iniciar nuestra aplicación Vue. Este enfoque nos permite hacer que nuestro sitio web sea dinámico, inmersivo e interactivo, pero la desventaja es que aumenta significativamente el tiempo de carga.

Como desarrolladores, es nuestro deber considerar a todos los usuarios, que pueden vivir en áreas donde la velocidad de la red no es tan impresionante. Queremos asegurarnos de que todos los usuarios tengan la mejor experiencia posible.

En la lección de hoy, quiero presentarles el concepto de App Shell.

### El Shell de la aplicación

La arquitectura Application Shell es un concepto que separa el contenido de su aplicación de la interfaz de usuario compartida en diferentes vistas. En otras palabras, el shell de la aplicación es una parte de la aplicación que no cambia con frecuencia. Queremos el mínimo de HTML y CSS para mostrar algo significativo al usuario lo antes posible. La primera carga debe ser extremadamente rápida y luego cobrar de inmediato. El paquete JS siempre es demasiado grande, reduzcamoslo.

1. Eliminemos nuestra importación de hojas de estilo en el archivo src / main.js y reemplácelas con etiquetas de enlace en la etiqueta principal de la carpeta raíz index.html. Lo estamos haciendo para que los recursos críticos se puedan recuperar rápidamente, a lo largo del marcado inicial y sin esperar nuestro JS de gran peso.

./index.html

```html
<!-- Bootstrap core CSS -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.2.1/css/bootstrap.min.css"
  rel="stylesheet">
<!-- Material Design Bootstrap -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/mdbootstrap/4.7.1/css/mdb.min.css" rel="stylesheet">
```
¡Nota!

Dado que estaremos enlazando a las hojas de estilo directamente en nuestro index.html, no es necesario mencionar el CSS de bootstrap por segunda vez, como una dependencia en nuestro package.json. Siéntase libre de eliminarlo desde allí. Recuerda tejer el proyecto si lo haces.

2. Con las herramientas de desarrollo del navegador, copie el código HTML de nuestra aplicación. Péguelo dentro de div # app en index.html del directorio raíz.

El marcado copiado de esa manera tendrá atributos con nombre hash específicos de Vue que comienzan con data-v- {hash}. Su propósito tiene que hacer el funcionamiento interno del marco: se utilizan para identificar sin problemas los elementos DOM. No tiene que preocuparse de que estén allí, siéntase libre de conservarlos o eliminarlos. La forma más sencilla es encontrar todos los lugares en los que aparece un atributo en particular mediante una herramienta de búsqueda o, en las versiones más recientes de VSCode, seleccionando uno y manteniendo presionada Ctrl + D para seleccionar los demás, luego Supr / Retroceso. Para omitir este trabajo manual, eche un vistazo a continuación, donde se presenta un marcado que ya se ha arreglado.

¿Ve el div sin clase y sin id debajo del encabezado Today:? Estos son nuestros eventos: en vivo, dinámicos, obtenidos de un servidor, caché o almacenamiento local. En este momento estamos trabajando en el HTML estático, uno que se mostrará antes de que suceda cualquier otra cosa; no tiene idea de los eventos que mostrar, ya que aún no ha tenido la oportunidad de preguntarle al backend sobre ellos. Es por eso que, en lugar de fingir que sabemos cosas que realmente no sabemos, demos a nuestros usuarios una indicación del progreso durante la carga inicial de la aplicación. Elimine el marcado de estos molestos eventos en sus etiquetas sin clase, sin identificación y pegue el código para un cargador Bootstrap en su lugar.

markup

```html
<div class="d-flex justify-content-center">
  <div class="spinner-border" style="width: 3rem; height: 3rem;" role="status">
    <span class="sr-only">Loading...</span>
  </div>
</div>
```
Mucho mejor. Ahora actualice la página; por un breve momento, debería ver nuestra aplicación con un cargador. Cuando el JavaScript termine de descargarse y el proceso de inicialización se active, Vue generará contenido para reemplazar lo que esté sucediendo en el elemento div # de la aplicación.

Bien, ahora un código ./index.html completo debería verse así:

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <title>mdbvue</title>
  <script src="https://maps.googleapis.com/maps/api/js" async></script>
  <link rel="manifest" href="/static/manifest.json" />
  <!-- Bootstrap core CSS -->
  <link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.2.1/css/bootstrap.min.css"
    rel="stylesheet">
  <!-- Material Design Bootstrap -->
  <link href="https://cdnjs.cloudflare.com/ajax/libs/mdbootstrap/4.7.1/css/mdb.min.css" rel="stylesheet">
</head>

<body>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-9">
          <h2 class="text-uppercase my-3">Today:</h2>
          <div class="d-flex justify-content-center">
            <div class="spinner-border" style="width: 3rem; height: 3rem;" role="status">
              <span class="sr-only">Loading...</span>
            </div>
          </div>
        </div>
        <div class="col-3">
          <h3 class="text-uppercase my-3">Schedule</h3>
          <h6 class="my-3">
            It's going to be busy that today. You have <b>4 events</b> today.
          </h6>
          <h1 class="my-3">
            <div class="row">
              <div class="text-center col-3"><i class="far fa-sun"></i></div>
              <div class="col-9">Sunny</div>
            </div>
            <div class="row">
              <div class="text-center col-3"><i class="fas fa-thermometer-three-quarters"></i></div>
              <div class="col-9">23°C</div>
            </div>
          </h1>
          <p>
            Don't forget your sunglasses. Today will dry and sunny, becoming warm in the afternoon with
            temperatures of between 20 and 25 degrees.
          </p>
        </div>
      </div>
    </div>
  </div>
  <!-- built files will be auto injected -->
</body>

</html>
```
¡Y así es como les hacemos saber a nuestros usuarios que están sucediendo cosas en segundo plano! Implemente esta nueva y atractiva característica mediante la implementación de hilo.

## Soporte para dispositivos iOS

El concepto de PWA en su forma actual fue propagado por Google, y actualmente sigue siendo su mayor partidario, proporcionando orientación en la creación de especificaciones y promoción general. Sin embargo, vale la pena señalar que la tecnología original de tener accesos directos a aplicaciones web fue presentada por primera vez por el mismísimo Steve Jobs en 2007 (y en ese momento se llamaba WebClip). No ganó mucha tracción en ese momento y la tecnología quedó prácticamente abandonada durante más de 10 años. Apple volvió a jugar silenciosamente en marzo de 2018 con iOS 11.3, agregando soporte para los conceptos básicos de PWA: Web Manifest y Service Worker. ¡Ahora puede aprovechar el almacenamiento en caché con los trabajadores del servicio y hacer que su PWA funcione sin una conexión a Internet en dispositivos iOS también! Sin embargo, no todo son rosas.

Cuando se trata de Worker, solo puede conservar los datos de la aplicación y los archivos de caché, pero no las tareas en segundo plano. También hay un límite de almacenamiento de 50 MB y "pocas semanas". Los íconos de Manifest no funcionan a la perfección (o no funcionan en absoluto) y no hay soporte para pantallas de inicio; solo obtendrá una pantalla en blanco cuando la aplicación se esté cargando. La aplicación se vuelve a cargar cada vez que se recupera del segundo plano, y no hay soporte para notificaciones automáticas y muchas otras funcionalidades que son esenciales para una aplicación móvil; no hay soporte para dispositivos Bluetooth Low Enegy, WebShare, Reconocimiento de voz y, más notablemente, el Banner de aplicación web. Pero lo lograremos, juntos, y trataremos de hacer lo que podamos para brindar la experiencia de PWA. Lo lograremos agregando etiquetas específicas de iOS en nuestra plantilla.

La falta de soporte para algunas de las funcionalidades del manifiesto significa que necesitamos emularlas de alguna manera, por lo general, agregando etiquetas especiales a nuestro ./index.html.

### Icono de acceso directo

En primer lugar, al igual que con las plataformas Windows o Android, hay un conjunto de iconos de referencia para cubrir iOS. Estos son todos cuadrados, con dimensiones de 120x120, 152x152, 167x167 y 180x180 píxeles. La mejor opción es preparar su conjunto de íconos para diferentes plataformas y diferentes tamaños todos juntos, y los generadores en línea generalmente lo cubren en eso. Además, al instalar su aplicación, iOS no usa los íconos del archivo de manifiesto, pero podemos solucionarlo agregando etiquetas especiales:

./index.html

```html
<link rel="apple-touch-icon" href="static/icons/Icon-120.png">
<link rel="apple-touch-icon" sizes="152x152" href="static/icons/Icon-152.png">
<link rel="apple-touch-icon" sizes="167x167" href="static/icons/Icon-167.png">
<link rel="apple-touch-icon" sizes="180x180" href="static/icons/Icon-180.png">
```

### Agregar un título de icono de lanzamiento

Para definir el título de una aplicación, similar al nombre y las propiedades de nombre corto definidas en el archivo manifest.json, agreguemos un elemento apple-mobile-web-app-title:

public / index.html

```html
<meta name="apple-mobile-web-app-title" content="Agenda App">
```
Ahora, cuando el usuario haga clic en el botón "agregar a la pantalla de inicio", verá una ventana emergente como se muestra a continuación:

### Modo autónomo

Para imitar el aspecto independiente de la aplicación nativa, tenemos que decirle al navegador que queremos lanzar nuestro proyecto como una aplicación web. También queremos ocultar una barra de estado, por defecto negra, que es visible en la parte superior de la pantalla. Podemos lograr esto agregando estas dos líneas:

public / index.html

```html
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```
### Pantalla de bienvenida

Lo último de lo que tenemos que ocuparnos es una pantalla de inicio (en este momento, iOS no genera pantallas de presentación desde el manifest.json), ya que funciona en Android. En su lugar, tenemos que proporcionar una imagen para la pantalla de cada dispositivo Apple; Además, también necesitamos incorporar puntos de interrupción de CSS y el atributo de orientación en nuestro html, no solo para cada dispositivo, sino tanto para su orientación horizontal como vertical. Uf, eso es alrededor de 20 etiquetas dependiendo del campo de cobertura del dispositivo que esté planeando. Aconsejo utilizar una herramienta en línea para la creación de imágenes de bienvenida, ya que puede ser muy tedioso manualmente. ¡Aquí tienes estos íconos aquí! Desempaquete en el directorio / static.

Al final, nuestro archivo 

./index.html se verá así:

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <meta name="apple-mobile-web-app-title" content="Agenda App">
  <title>mdbvue</title>
  <script src="https://maps.googleapis.com/maps/api/js" async></script>
  <link rel="manifest" href="/static/manifest.json" />
  <!-- Bootstrap core CSS -->
  <link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.2.1/css/bootstrap.min.css"
    rel="stylesheet">
  <!-- Material Design Bootstrap -->
  <link href="https://cdnjs.cloudflare.com/ajax/libs/mdbootstrap/4.7.1/css/mdb.min.css" rel="stylesheet">
  <link rel="apple-touch-icon" href="static/icons/120.png">
  <link rel="apple-touch-icon" sizes="152x152" href="static/icons/Icon-152.png">
  <link rel="apple-touch-icon" sizes="167x167" href="static/icons/Icon-167.png">
  <link rel="apple-touch-icon" sizes="180x180" href="static/icons/Icon-180.png">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

  <!-- iPhone X / XS -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)"
    href="/static/splashscreens/Splash-2436x1125.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"
    href="/static/splashscreens/Splash-1125x2436.png" />

  <!-- iPhone XR -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)"
    href="/static/splashscreens/Splash-1792x828.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"
    href="/static/splashscreens/Splash-828x1792.png" />

  <!-- iPhone 6s / 7 / 8 -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 375px) and (device-height: 667px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)"
    href="/static/splashscreens/Splash-1334x750.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 375px) and (device-height: 667px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"
    href="/static/splashscreens/Splash-750x1334.png" />

  <!-- iPhone XS Max -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"
    href="/static/splashscreens/Splash-1242x2688.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)"
    href="/static/splashscreens/Splash-2688x1242.png" />

  <!-- iPhone 6s Plus / 7 Plus / 8 Plus -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3) and (orientation: landscape)"
    href="/static/splashscreens/Splash-2208x1242.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"
    href="/static/splashscreens/Splash-1242x2208.png" />

  <!-- 12.9" iPad Pro -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 1024px) and (device-height: 1366px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"
    href="/static/splashscreens/Splash-2048x2732.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 1024px) and (device-height: 1366px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)"
    href="/static/splashscreens/Splash-2732x2048.png" />

  <!-- 10.5" iPad Pro -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 834px) and (device-height: 1112px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)"
    href="/static/splashscreens/Splash-2224x1668.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 834px) and (device-height: 1112px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"
    href="/static/splashscreens/Splash-1668x2224.png" />

  <!-- 9.7" iPad / 7.9" iPad mini 4 -->
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 768px) and (device-height: 1024px) and (-webkit-device-pixel-ratio: 2) and (orientation: landscape)"
    href="/static/splashscreens/Splash-2048x1536.png" />
  <link rel="apple-touch-startup-image"
    media="screen and (device-width: 768px) and (device-height: 1024px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"
    href="/static/splashscreens/Splash-1536x2048.png" />
</head>

<body>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-9">
          <h2 class="text-uppercase my-3">Today:</h2>
          <div class="d-flex justify-content-center">
            <div class="spinner-border" style="width: 3rem; height: 3rem;" role="status">
              <span class="sr-only">Loading...</span>
            </div>
          </div>
        </div>
        <div class="col-3">
          <h3 class="text-uppercase my-3">Schedule</h3>
          <h6 class="my-3">
            It's going to be busy that today. You have
            <b> events</b> today.
          </h6>
          <h1 class="my-3">
            <div class="row">
              <div class="text-center col-3" i class="far fa-sun" i></div>
              <div class="col-9"> Sunny</div>
            </div>
            <div class="row">
              <div class="text-center col-3" i class="fas fa-thermometer-three-quarters" i></div>
              <div class="col-9"> 23°C</div>
            </div>
          </h1>
          <p>
            Don't forget your sunglasses. Today will dry and sunny, becoming
            warm in the afternoon with temperatures of between 20 and 25
            degrees.
          </p>
        </div>
      </div>
    </div>
  </div>
  <!-- built files will be auto injected -->
</body>

</html>
```
## Auditoría de PWA
¿Cómo podemos saber si nuestra aplicación funciona como una aplicación web progresiva? ¿Hay algo que podamos mejorar? ¿O nuestra aplicación ya es lo suficientemente buena?

En el artículo de hoy, aprenderemos sobre una herramienta de auditoría que nos ayuda a determinar si nuestra aplicación califica como PWA.

### Faro

Lighthouse es una herramienta de código abierto introducida por Google. Al principio, se creó para medir el rendimiento de PWA, pero ahora su funcionalidad cubre mucho más. Verifica la calidad de su aplicación en cinco categorías:

- Actuación
- Aplicación web progresiva
- Accesibilidad
- Mejores prácticas
- SEO

En el informe, obtendrá una puntuación entre 0 y 100 puntos, donde 100 es la más alta. Su puntaje puede variar entre las pruebas; puede suceder debido a condiciones cambiantes de la red, diferentes procesos que se ejecutan en segundo plano en su computadora, etc. Vale la pena realizar algunas pruebas y obtener el promedio de los resultados.

Además, el informe le brinda un conjunto de recomendaciones que, cuando se implementan, mejoran el rendimiento de su sitio web y la visibilidad para los motores de búsqueda. Siempre es una buena idea comparar su sitio con el conjunto de criterios de Google; las sugerencias ayudan a uno a convertirse en un mejor desarrollador.

Lighthouse está disponible como una extensión de Chrome para Chrome 52 y versiones posteriores, y desde hace poco, ¡también Firefox!

### ¿Cómo ejecutar la auditoría?

Para ejecutar la auditoría, abra su aplicación en un navegador (Chromium/Chrome/Firefox). Las herramientas de auditoría se pueden encontrar en la pestaña Auditorías.

Auditoría de faro
Abra la pestaña, ajuste la configuración a sus necesidades o ejecute de forma predeterminada. Después de un breve momento, verá el informe.

Informe faro
Como puede ver, ¡hay margen de mejora! Al hacer clic en las categorías o al desplazarse hacia abajo, podemos examinar los resultados de la prueba en detalle.

informe PWA detallado

Hay al menos algunas cosas que podríamos hacer para que nuestra aplicación sea más progresiva. Empecemos desde abajo.

El contenido no tiene el tamaño correcto para la ventana gráfica

El problema es que en algún momento, mientras Lighthouse ha estado verificando cómo se comporta nuestra aplicación en diferentes anchos de ventana gráfica, las propiedades innerWidth y outerWidth de la interfaz de la ventana han sido diferentes. Para garantizar la mejor experiencia de usuario móvil, la prueba verifica de manera efectiva si el contenido no se desborda horizontalmente, lo que lleva a un tiempo de desplazamiento inesperado. El número de píxeles en la pista indica el ancho que aparece el problema; al reducir la ventana del navegador, puede simularlo para ver el problema de cerca.

El culpable está ahí: cuando el espacio horizontal se vuelve escaso, las dos columnas principales de nuestra aplicación permanecen una al lado de la otra, lo que hace que el texto salga por el borde derecho. ¿Cómo resolverlo? Bueno, por un lado, podríamos agregar una etiqueta **style** con ámbito a nuestro archivo ./src/App.vue e incluir allí un CSS overflow-x: hidden; regla dirigida al .container principal.

La solución nos permitiría pasar la prueba ocultando los efectos del problema, pero seamos sinceros, no nos permitiría recuperar lo que estamos perdiendo actualmente, es decir, el control sobre el flujo del contenido en las ventanas de visualización estrechas. Somos buenos desarrolladores con una intención genuina, así que utilicemos la personalización de MDB en su lugar. Después de todo, ¿por qué ocultar el contenido que se filtra, si pudiéramos mostrarlo maravillosamente usando la biblioteca?

Tengamos el diseño como está, pero con un pequeño cambio en los detalles: en caso de que no haya espacio para tener las columnas principales una al lado de la otra, que el contenido fluya hacia abajo. Lo lograremos usando el sistema de cuadrícula de MDBVue a través de accesorios **mdb-col**.

Tal como está ahora, estamos describiendo los anchos de las columnas como 9 y 3 para todos los tamaños de pantalla. Sin embargo, como sabemos por los documentos, podemos sobrescribirlos usando accesorios dedicados para tamaños dados y más.

Idealmente, nos gustaría mantener la proporción de las columnas actuales para todos los tamaños de pantalla más anchos que sm (todos), donde las columnas deberían ir una debajo de la otra, ocupando todo el espacio horizontal disponible, que, en el caso de nuestra cuadrícula, significa completo 12.

Para lograrlo, identifique las dos columnas raíz en la aplicación. Debemos jugar con sus accesorios: asignar el ancho máximo para las pantallas predeterminadas (las más pequeñas) y sobrescribirlo con las proporciones actuales para todos los anchos de la ventana gráfica más allá de los de los teléfonos inteligentes usando el accesorio sm.

./src/App.vue

```html
<template>
  <mdb-container>
    <Notification :status="status" />
    <mdb-row>
      <mdb-col col="12" sm="9">
        <!-- first column's contents... -->
      </mdb-col>
      <mdb-col col="12" sm="3">
        <!-- second column's contents... -->
      </mdb-col>
    </mdb-row>
  </mdb-container>
</template>
```
Pruébelo ahora: el contenido debería fluir hacia abajo en el dispositivo móvil, mientras que la prueba de ancho de Lighthouse PWA también debería pasar.

No establece un color de tema para la barra de direcciones
La auditoría Lighthouse considera que la falta de la metaetiqueta del color del tema es un fracaso. ¿Pero no lo declaramos al final de nuestro manifest.json (aunque escrito con un guión bajo, theme_color)?

Resulta que siempre debemos especificar el color usando la etiqueta. Los navegadores reconocen el manifiesto, pero solo una vez que el usuario ha agregado la página a su pantalla de inicio. De lo contrario, la etiqueta sobrescribe el valor del manifiesto, lo que permite un poco más de personalización (para tener un color de tema diferente para diferentes subpáginas). ¡Entonces agreguemos la etiqueta!

./index.html

```html
<!--...somewhere in the head tag... -->
<meta name="theme-color" content="#4DBA87">
```
¡Sí, es todo lo que hay que hacer!

No redirige el tráfico HTTP a HTTPS
Queremos que la comunicación sea segura (r) con https, y la queremos sin excepciones. En la práctica, significa configurar el servidor para redirigir todo el tráfico http a un lugar seguro. Para nuestro entorno de desarrollo, podríamos usar webpack-dev-middleware para agregar un poco de middleware que escuche las solicitudes que comiencen con http o con el indicador request.secure configurado en falso y luego redirigido al mismo lugar, solo a través de https.

El esfuerzo garantizaría la aprobación de la prueba, pero perderíamos el panorama general. La prueba trata sobre la forma en que nuestro servidor está manejando las solicitudes inseguras de nuestros usuarios, y si modificamos nuestro entorno de desarrollo para redireccionamientos particulares no cambiaría nada para ellos. Si realmente queremos garantizar la seguridad y privacidad de nuestra audiencia a través de https, es el servidor de producción del que debemos preocuparnos. En general, las formas exactas de cumplir con el requisito dependen de la arquitectura y configuración del servidor.

Estamos de suerte: muchas plataformas para alojar nuestro proyecto, como Firebase o GitHub Pages, son seguras de forma predeterminada, sus servidores redirigen el tráfico de http a https de forma inmediata. Implemente el proyecto, diríjase a la URL proporcionada y vuelva a ejecutar la auditoría para ver a qué me refiero.

despliegue de hilo

Si está mirando la auditoría de compilación de producción, es posible que reconozca que las redirecciones pasan de prueba, y muchas otras seguidas. ¡Nuestros resultados mejoraron en todos los ámbitos!

Informe de PWA sobre Firebase
La carga de la página no es lo suficientemente rápida en las redes móviles
Este es otro problema que se resolvió al construir nuestro proyecto. ¿Por qué? Entendemos por edificación realizar optimizaciones que no son posibles ni deseadas para el desarrollo. Nuestra configuración de producción de paquetes web uglifica el código (lo que significa que lo minimiza y lo oscurece), lo elimina de los comentarios, los espacios en blanco (y las comillas para los atributos HTML), divide el JavaScript en fragmentos y comprime los medios; asegúrese de revisar ./build ¡Archivo /webpack.prod.conf.js para obtener más detalles!

Todo eso se suma al rendimiento mucho mejor de la aplicación, reforzado con Firebase CDN extra rápido. Con arduo trabajo y algo de ayuda en infraestructura, ¡hicimos nuestra aplicación completamente progresiva!

Sin embargo, la cuestión del rendimiento nunca debe tomarse a la ligera. Siempre vale la pena explorar si podríamos hacer que nuestra aplicación entregue contenido aún más rápido; después de todo, constituiría la experiencia que haría que la gente regresara. Mejore la política de almacenamiento en caché para incluir los recursos necesarios y experimente con la precarga y el diferimiento de scripts utilizando el atributo defer para acelerar los tiempos de carga.

Como hemos aprendido en el artículo, Lighthouse Audit es una poderosa herramienta de evaluación comparativa destinada a proporcionar guildelines para desarrolladores web móviles. Ciertamente, ayuda a que nuestras aplicaciones sean correctas en términos de soporte y rendimiento de PWA, pero no pretenda satisfacer solo la prueba en sí; a menudo, es el contexto más amplio de la prueba lo que vale la pena considerar. Con todo, considerar estas pautas es una excelente manera de aprender las mejores prácticas.