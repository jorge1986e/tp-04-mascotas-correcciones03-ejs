# Trabajo Práctico 04 - Mascotas (EJS y Express)
Alumno: Chocobar Jorge

## Descripción
Este proyecto es una aplicación web desarrollada con **Node.js**, **Express** y **EJS** como motor de plantillas. Permite visualizar un catálogo de mascotas, ver los detalles de cada una mediante su identificador, y registrar nuevas mascotas a través de un formulario con validación básica.



## Instalación
Para instalar las dependencias necesarias del proyecto, ejecuta el siguiente comando en la terminal:
```
npm install
```

## Ejecución
Para iniciar la aplicación, ejecuta:

```
npm start
```

Luego, abre tu navegador y ingresa a: http://localhost:3000

Para verificar la sintaxis antes de entregar:

```
npm run check
```

## Páginas y rutas
La aplicación cuenta con las siguientes rutas configuradas en index.js:
```
- GET /: Página de inicio ("Mascoteros").
- GET /mascotas: Muestra el listado completo de mascotas.
- GET /mascotas/nueva: Muestra el formulario para registrar una nueva mascota.
- GET /mascotas/:id: Muestra la vista detallada de una mascota específica buscada por su ID.
- POST /mascotas: Recibe formulario de registro para agregar una nueva mascota.
- GET /api/mascotas: Devuelve los datos de las mascotas en formato JSON.
```

## Estructura de vistas
Las vistas están en la carpeta views/ y usan el motor EJS junto con express-ejs-layouts.

```
views/
|-- layouts/main.ejs          (layout principal)
|-- partials/encabezado.ejs   (navegación)
|-- partials/pie.ejs          (pie de página)
|-- mascotas/lista.ejs        (listado y estado vacío)
|-- mascotas/detalle.ejs      (detalle de una mascota)
|-- mascotas/nueva.ejs        (formulario)
|-- inicio.ejs                (página inicial)
```
y no-encontrado.ejs (página 404) en la raíz de views/.

## Recursos estáticos
Los archivos públicos se ubican en public/ y se sirven con express.static: /css/estilos.css, /img/mascota.svg y /js/app.js (además del logo y las imágenes de la página inicial y del 404).

## Formulario
La aplicación incluye un formulario de registro /mascotas/nueva con validaciones en el servidor para asegurar que los campos requeridos y la edad sean válidos antes de registrar la mascota.

## Persistencia de los datos
Al iniciar, la aplicación lee datos/mascotas.json y guarda las mascotas en un arreglo en memoria. Las mascotas nuevas se agregan solo a ese arreglo; no se escriben en el archivo JSON. Cuando el servidor se reinicia, la memoria del proceso se libera y se vuelve a leer el JSON original, por eso los registros nuevos desaparecen y la lista vuelve a las mascotas iniciales.

## Preguntas y Respuestas Teóricas:

### 1. Diferencia entre layout, vista y parcial
- Layout: Es la plantilla principal o estructura general que se repite en todas las páginas (por ejemplo, el archivo que contiene la etiqueta <html>, <head>, el encabezado y el pie de página) define el contenedor común.
- Vista: Es el contenido específico de cada página en particular (por ejemplo, el formulario de registro o el detalle de una mascota). Se inyecta dentro del layout principal.
- Partials: Es un fragmento de código HTML reutilizable que se puede incluir dentro de varias vistas o layouts (como un menú de navegación, un encabezado encabezado.ejs o un pie de página pie.ejs).

### 2. Datos enviados a una vista mediante: res.render
res.render es el método de Express que compila una plantilla EJS y la envía como HTML al cliente. Se le pueden pasar datos en forma de objeto como segundo argumento. 
Ejemplo en el código:

```
res.render("inicio", {titulo: "Mascoteros",});
```
Aquí le enviamos la variable titulo a la vista inicio.ejs para que pueda mostrarla dinámicamente usando <%= titulo %>.

### 3. Función de express.static
express.static es un middleware integrado en Express que sirve archivos estáticos (como imágenes, archivos CSS, JavaScript del cliente) desde un directorio específico.
Ejemplo en el código:
```
app.use(express.static(path.join(__dirname, "..", "public")));
```
Esto permite que cualquier archivo guardado en la carpeta public sea accesible directamente desde el navegador por ejemplo, http://localhost:3000/img/mascota.svg

### 4. Función de express.urlencoded
express.urlencoded es un middleware que analiza (parsea) los datos enviados desde un formulario HTML mediante el método POST. 
Ejemplo en el código:
```
app.use(express.urlencoded({ extended: false }));
```
Sin esta función, Express no sabría leer los datos que llegan en el cuerpo de la petición (req.body). Gracias a ella, podemos acceder fácilmente a los campos del formulario como req.body.nombre, req.body.edad, etc.

### 5. Recorrido POST, redirección y GET
Es el ciclo típico al enviar un formulario:
1. Recorrido POST: El usuario completa el formulario y lo envía. Los datos viajan al servidor mediante una ruta configurada con app.post("/mascotas", ...) (en nuestro código, se validan los datos y se agrega la mascota al arreglo).
2. Redirección: Una vez procesado el POST con éxito, en lugar de renderizar una vista directamente, el servidor utiliza res.redirect("/mascotas") para indicarle al navegador que cambie de URL.
3. GET: El navegador hace una nueva petición automática mediante el método GET a la ruta /mascotas, la cual se encarga de renderizar y mostrar el listado actualizado de mascotas.

