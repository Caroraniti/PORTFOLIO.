# Observaciones

Caro, felicitaciones por tu trabajo. Tu portfolio se ve muy lindo; me alegra ver que en general seguiste las pautas del diseño solicitado pero a la vez le diste tu toque personal. Me gustan muchisimo las imagenes e iconos elegidos, la eleccion de colores. 

El mayor problema en tu portfolio es que el responsive no funciona, lo que afecta negativamente tu nota a pesar de que se nota que pusiste esfuerzo. Seria una nota muchisimo mas alta si eso estuviera, pero una web sin responsive no está completa: la mayoría de nuestros usuarios van a ver nuestra web desde el celular, por lo que es absolutamente necesario poder ofrecerles la misma experiencia a ellos. Noto que tenés media queries, por lo que no sé si te faltó tiempo o no lo pudiste resolver. Si es el segundo caso, me escribirías? Asi podemos verlo juntas más en detalle.

Te dejo varios comentarios para mencionar cositas puntuales. Como comentamos en clase, este trabajo no se hace para que constates conocimientos, sino para que aprendas: en ese sentido, mi intencion es que estos comentarios te sirvan para aprender, mejorar tu codigo a futuro e ir apreciando mejor qué se espera de nosotras como desarrolladoras front end.

## Estructura correcta de documento HTML

Cumplido en general. 

Algo que me llama la atención es tu `header`, dado que allí tenés una orden que no cumple la estructura solicitada para los fonts de google:

```html
    <link href = "https: //fonts.googleapis.com/css2? family = Open + Sans: ital, wght @ 1300 & display = swap "rel =" stylesheet ">
<!-- compara con: -->
 <link href="https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@1300&display=swap" rel="stylesheet">
 ```

Me da la impresión que el autotraductor del navegador te cambió ese link. De todos modos no usas esta tipografia en tu TP, asi que ni siquiera deberia estar aqui ese link. No debe haber espacios en los links, sino el navegador no puede traerse esa tipografia (si vas a la consola en tu TP, vas a notar que ese link tira error, justamente por los espacios en la URL)


## Respeta la consigna

- El portfolio cuenta con las secciones solicitadas 

Cumplido

- Al clickear en los links de navegación, debe llevar a la sección correspondiente

No cumplido para la barra de navegación superior. Doy por hecho que tenés los conocimientos, ya que en el footer funciona, así que asumo que lo de la barra de navegación superior fue un descuido, pero ojo con eso. Afecta muy negativamente la experiencia en tu página. 

- Al clickear en los links de contacto, debe llevar a la página externa correspondiente 

Cumplido. Un detalle que afecta la experiencia es que el link solo funciona si hago click en el icono, y no dentro de todo el circulo blanco. Un detalle, pero molesto y potencialmente inaccesible (si a nuestro usuario le cuesta manejar el mouse para hacer click en lugares muy pequeños, por ejemplo). 

Esto ocurre porque tu HTML es asi:

```html
<li class="iconos">
    <a href="http://www.twitter.com/Caroraniti">
        <img class="imagen-icono" src="twitter.svg" alt="twitter">
    </a>
</li>
```

La clase "iconos" es la que da el fondo blanco y circular, y espero que todo eso sea un link. Fijate como cambia si hago que la etiqueta `a` rodee al `li`: 

```html
<a href="http://www.twitter.com/Caroraniti">
    <li class="iconos">
        <img class="imagen-icono" src="twitter.svg" alt="twitter">
    </li>
</a>
```

- El portfolio debe tener un diseño responsivo y verse correctamente en distintos dispositivos 

Cumplido a medias. Veo que tus tarjetas por ejemplo se adaptan perfectamente a responsive, pero esto no ocurre para la barra de navegación, la sección de presentación, o el footer. 

Sobre tus media queries, es muy recomendable que las ordenes de mayor a menor. Hoy en tu CSS no parecen seguir un orden especifico. Considerá este ejemplo: en pantallas de escritorio queremos que el `h1` sea rojo, en tablet queremos que se vea verde y en celulares queremos que se vea azul. 

```css
h1 {
    color: red;
}

/* celulares */
@media screen and (max-width: 576px) {
    h1 {
    color: blue;
    }
}

/* tablet */
@media screen and (max-width: 992px)  {
    h1 {
    color: green;
    }
}
```

En escritorio y tablet todo parece verse bien... pero en celulares, seguimos viendo el `h1` de color verde en lugar de verlo azul. Eso ocurre porque en la orden para tablets solo hemos dicho `(max-width: 992px)`, es decir: "siempre que el tamaño de la pantalla sea menor a 992px, veremos ese estilo". Inmediatamente antes de esa orden tenemos una que dice "siempre que el tamaño de la pantalla sea menor a 576px..." pero la orden para 992px está por debajo y, por lo tanto, termina imponiéndose por la cascada de CSS. Eso ocurre en todas tus media queries. Escribime por favor si esto no te queda claro.

Una vez que tenes ordenadas las media queries de mayor a menor, vas a notar que las ordenes que damos en media queries no deberian repetirse: si una orden esta dada para un elemento en una resolucion grande, se cumplirá en las subsiguientes media queries para pantallas mas pequeñas a menos que explicitamente la cambiemos. Por eso no es correcto que repitas los estilos, por ejemplo en caja-contacto:

```css
@media screen and (max-width: 580px){
    .caja-contacto{
        width: 90%;
        display: flex;
        flex-direction: column;
        margin-top: 30px;
        margin-bottom: 30px;
        padding-top: 20px;
        padding-bottom: 20px;
        padding-right: 10px;
        padding-left: 10px;
    }
}

@media screen and (max-width: 400px){
    .caja-contacto{
        width: 90%;
        display: flex;
        flex-direction: column;
        margin-top: 30px;
        margin-bottom: 30px;
        padding-top: 20px;
        padding-bottom: 20px;
        padding-right: 10px;
        padding-left: 10px;
    }
}
```

Indicas que la barra-navegacion debe desaparecer, pero solo das esa orden para pantallas cuyo tamaño máximo es 375px:

```css
@media (max-width:375px){
    .barra-navegacion{
        width: 100%;
        display: none;
    }  
}
```

Eso hace que en **todas** las resoluciones **mayores** a 375px, la barra de navegacion se vea. Vos queres algo diferente: que **no** se vea en las resoluciones **menores** a 1100px por ejemplo. Esta orden deberia estar en otra media query. 

Para resoluciones menores a 900px, tu imagen de portada junto con el texto "rebalsan" la pantalla. Deberias indicarles en una media query que la imagen se ponga por debajo del texto. Por ejemplo:

```css
@media (max-width:900px){
    .portada {
        flex-direction: column;
        align-items: center;
    }  
}
```

Otro problema que vas a tener ahi es que le diste a la imagen un tamaño fijo de 500px, asi que **siempre** va a medir 500px. Si el tamaño del celular del usuario es de 350px, igual la imagen medirá 500px y rebalsará la pantalla. Deberías darle un width de 100% en las resoluciones de celular. 

El footer no tiene media queries, pero te animo a hacerlas y consultarme si algo te traba. 

- El portfolio debe estar deployado y ser accesible desde una URL 

Cumplido

- El repositorio en GitHub debe tener un readme adecuado 

Cumplido. 

### Respeta el diseño dado 

Cumplido a medias. 

A nivel diseño, y sabiendo que siempre la estética de un sitio es una apreciación personal, creo que el mayor problema de tu web es que muchas veces faltan o sobran algunos espacios a diferencia del diseño propuesto. Por darte un ejemplo de lo que quiero señalar, notá cuánto mide de alto tu sección de presentación vs el modelo de Ada. Notá qué grande es el espaciado entre tu titulo "Mis conocimientos" y el subtitulo vs el modelo de Ada.  Este tipo de pautas no son decisión nuestra como desarrolladores: vienen indicadas desde el diseño, y es necesario tratar de que sean lo mas fieles al modelo que podamos.

El otro problema mas grave es la estructura de las tarjetas. Quiza en tu compu las veas bien, pero en la mia por ejemplo para "Mis Conocimientos" yo veo 4 tarjetas arriba y dos abajo, cuando en el modelo son siempre 3 y 3. Comentamos en clase que una buena solucion para esto es agregar un div mas entre .contenedor-apps y las tarjetas, que tenga un max-width de tal forma que siempre se vean 3. Consultame si esto te trae dudas. 

Las tarjetas de "Mis proyectos" se ven siempre tiradas hacia la izquierda en lugar de estar centradas. Lo resolviste bien, el problema es que le diste un width de 1400px a .contenedor-cajas y deberia ser mas pequeño. 1200px estaria mejor. Hace que sea max-width en lugar de width para que funcione mejor en responsive. 

### Buena estructura de proyecto 

Usás muchas imágenes en tu proyecto, y viéndolo a primera vista en github no es tan fácil ver cuáles son los archivos principales. Siempre que uses imágenes locales, como en este caso, agregalas a una carpeta que se llame por ejemplo "imgs", "imagenes" o "assets". De esta manera solo los archivos principales - html, readme y css - son los que están en la carpeta principal. La excepción a esta regla es el favicon -faltante en tu codigo-, que siempre debería ir en la carpeta principal. 

### Código bien indentado 

Cumplido.

### Comentarios que permiten mejorar la legibilidad del código 

Cumplido. 

### Uso correcto de etiquetas semánticas 

Muchos problemas acá. 

Tu `header` termina junto a la `nav`: si bien en algunos diseños identificar el `header` es complejo, creo que en este caso definitivamente debe incluir a la primera sección "portada". Un `header` representa siempre lo que llamamos "contenido introductorio": todo aquello que da la bienvenida a nuestra web, sin dar información más específica (pero invitándonos a verla)

Creo también que las tarjetas de producto deberían ser `article` en lugar de `div`. Esto permite que los usuarios de lectores de pantalla los encuentren fácilmente, y también ayuda al SEO. Todo elemento que sea autocontenido, reusable, repetible y pueda usarse en diferentes contextos, debe ser un article. Sobre ese tema te dejo una lectura: https://www.smashingmagazine.com/2020/01/html5-article-section/

Usas mas de un `h1` en tu web. El h1 es el titulo principal, y debería haber sólo uno por página. El resto deberían estar en orden de jerarquía: h2 para los titulos de secciones, h3 para los títulos dentro de cada subsección, etc. En este caso, "Mis conocimientos", "Mis proyectos" etc deberían ser h2 y los títulos de las cards deberían ser h3. 

En tu footer tenes esta estructura: 

```html 
<ul>
    <a class="link-footer" href="#">HOLA</a>
</ul>

<ul>
    <a class="link-footer" href="#">CONOCIMIENTOS</a>
</ul>

<ul>
    <a class="link-footer" href="#">PROYECTOS</a>
</ul>

<ul>
    <a class="link-footer" href="#">CONTACTO</a>
</ul>
```

Que no es correcta, ya que esto no es 4 listas, sino 1 lista con 4 elementos que deberian ser `li`. Un lector de pantalla va a decirle al usuario que acá hay 4 listas!! 

### Buenos nombres de clases 

No está cumplido. Luego de los problemas con las etiquetas semánticas, es el factor que más atenta contra la calidad de tu código. Es muy dificil entender tu CSS porque no hay manera de saber a que se refiere cada cosa: hay que estar chequeando a cada rato el HTML para poder entenderlo. 

Cuando decimos que un nombre de clase debe ser descriptivo, lo decimos en un sentido funcional: qué rol cumple este elemento en el código. Los colores de los elementos, su forma, su estilo, su posición, todas esas cosas pueden cambiar y efectivamente cambian todo el tiempo en las webs que hacemos. El botón que hoy es un circulo mañana será cuadrado; la sección que estaba primera mañana estará tercera, por lo que no sirve decir "texto2". Esos factores no son buenos descriptores, y no deberían ser parte de nuestros nombres de clases. "texto2" es el texto que acompaña la seccion cita: "texto-seccion-cita" es mas claro. "texto3" es el subtitulo de la seccion Mis Proyectos: "subtitulo-seccion-proyectos" es mejor nombre. 

Otros nombres de clases que usas directamente no tienen sentido. "blanco", "palabras", "descripcion-palabra" son algunos ejemplos. Entiendo que es dificil pensar buenos nombres, y es algo con lo que todos renegamos alguna vez, pero es importante que te vayas acostumbrando a darle más atención a eso. 

Cuando quieras separar palabras, poneles un guion: "contenedorsomeone", "contenedorproyectos", son dificiles de leer. 

En resumen, a la hora de darle un nombre de clase a algo, pensá qué es ese elemento, qué función cumple en tu página. 

### Código CSS bien estructurado 

Cumplido a nivel formal. Noto muchos estilos innecesarios o confusos, que te voy indicando en tu archivo de css. 

### Reutilización de estilos 

No cumplido. La idea de las clases es que justamente agrupen a muchos elementos, no que se apliquen a uno solo. Considerá por ejemplo tus imágenes de Mis Conocimientos:

```css
.html5{
    width: 70px;
    height: 70px;
}

.css {
    width: 70px;
    height: 70px;
}

.sass {
    width: 70px;
    height: 70px;
}

.java {
    width: 70px;
    height: 70px; 
}

.react {
    width: 70px;
    height: 70px;
}

.git {
    width: 70px;
    height: 70px;
}
```

Todas estas imagenes tienen la misma función y el mismo estilo. Para qué darles a todas distintas clases? Podriamos haberles dado a todas la clase "imagen-tarjeta-mis-conocimientos" por ejemplo. 

```css
.imagen-tarjeta-mis-conocimientos {
    width: 70px;
    height: 70px;
}
```

No repetirnos a nosotras mismas es un principio muy importante en programación, porque en la repetición se encuentran buena parte de nuestros errores. Reutilizá tus clases siempre que puedas.

### Cumple con criterios básicos de accesibilidad 

Eliminas el outline en tus botones, y la unica indicacion en el focus es el borde del mismo color del boton (.links:focus), por lo que no se ve el foco cuando esta en ese elemento. Tambien eliminas el outline de tu formulario. No puedo insistir lo suficiente en esto: **nunca** se elimina el outline de los elementos a menos que tengas una alternativa bien testeada que funcione bien en todos. Tu pagina es imposible de recorrer con teclado: la mitad de las veces no hay forma de saber en donde estamos paradas. 

Te faltan muchas etiquetas semanticas como te comenté en esa sección que harían que tu web sea mas amable con el lector de pantalla. 

- Los colores tienen un contraste adecuado 

No cumplido toda vez en que tenes un texto de color blanco y un fondo de color "violet". Siempre chequeá con herramientas del tipo https://webaim.org/resources/contrastchecker/

- Las imágenes tiene el atributo `alt` que corresponde 

Cumplido a nivel formal, pero una persona que necesita de un lector de pantalla va a tener problemas entendiendo muchas de tus imagenes. Para la imagen de presentación decís "imagen1" en el alt. El lector va a leerle al usuario: "Imagen: imagen uno", algo que el usuario no vidente ya sabe: que esta es la primera imagen de tu sitio.

Las imágenes le comunican muchisimas cosas a tus usuarios videntes. No es casualidad que para tu portada hayas elegido una mujer con una computadora: este es tu portfolio despues de todo, y queres transmitir rapidamente "este es el sitio de una mujer programadora". Si hubieras elegido la imagen de un hombre haciendo malabares la primera impresión de tu sitio sería muy distinta. Esa misma información, tenemos la obligación de transmitirsela a quien no puede ver tus imagenes. "Mujer programadora junto a una computadora" es una descripción más clara y mejor. 

El alt es un texto que lee el lector de pantalla, por lo que no debés usar guiones como en "generador-memes". 

Hay ocasiones en las cuales agregar un alt, paradojicamente arruina la experiencia. Considerá tus tarjetas de tecnologias. El lector va a leer:

"Sección: Mis conocimientos. Imagen: html5. Texto html 5. Imagen: css. Texto: css"... y asi. Si la idea es que el usuario no vidente conozca las tecnologías que manejas, esto es repetitivo y molesto. En estos casos podemos agregar "aria-hidden" a la imagen, ya que no suma nada. 

Tus `links` tambien necesitan un aria asi el usuario sabe adonde va a ir. Si falta, el lector de pantalla lee la url, lo cual es muy molesto. Algo asi es mucho mejor: `<a href="https://www.linkedin.com/in/Caroraniti" aria-label="Mi linkedin">`


- Los íconos y elementos que no presentan texto agregan la información correspondiente por otros medios (etiquetas aria, texto oculto) 

No cumplido. 

- Los íconos y elementos que no necesitan ser anunciados por un lector de pantalla tienen la etiqueta aria
correspondiente 

Cumplido. 

- Commits con mensajes adecuados 

No cumplido, pero entiendo que tuviste que cambiar de carpeta. 

- Cuenta con un favicon

No cumplido. 

### Nota: 7