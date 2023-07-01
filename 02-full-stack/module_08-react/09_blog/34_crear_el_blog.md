# Crear el Blog

<div id="index"></div>

* [Cambiar el componente funcional a uno basado en clase](#cambiar-el-componente-funcional-a-uno-basado-en-clase)
* [Crear y obtener datos de la API](#crear-y-obtener-datos-de-la-api)
* [Mostrar los datos de los blogs](#mostrar-los-datos-de-los-blogs)

<br/>


[<< EDITAR PORTFOLIO ITEMS](../08_portfolio_manager/33_editar_portfolio_items.md#editar-portfolioitems) | [HOME](../../../README.md#devcamp)


<br/><hr/>
<hr/><br/>


<div align='right'>
    <a href='#index'>Volver arriba</a>
</div>


## Cambiar el componente funcional a uno basado en clase

Al comienzo del proyecto ya fue creado el componente funcional `blog.js`. Lo que haremos a continuación, será convertirlo en un componente basado en clase. Para ello, accederemos a la ruta `src/components/pages/blog.js` y cambiaremos el código por el siguiente:

```js
// blog.js

import React, { Component } from 'react';
// ...

export default class Blog extends Component {
    construsctor() {
        super();
    }

    render() {
        return (
            /* ... */
        );
    }
}
```

<br/>

Mantendremos el código que teníamos dentro de `return()`, pero modificaremos el resto de líneas para conseguir modificar el componente de forma correcta.

Si arrancamos la aplicación, veremos que nada debería haberse visto modificado (*puesto que no hemos añadido ni modificado elementos dentro del componente*).


<br/><hr/>
<hr/><br/>


<div align='right'>
    <a href='#index'>Volver arriba</a>
</div>


## Crear y obtener datos de la API

Para crear datos en la API, accederemos al link de [devcamp.space](https://www.devcamp.space/) y nos loguearemos con nuestro usuario. Una vez dentro, accederemos a la sección de `Portfolio`, después a `Blogs` y crearemos tantas entradas como queramos.

Ahora, añadiremos el código necesario para obtener la información de los blogs que acabamos de crear:

```js
// blog.js

// ...
import axios from 'axios';

export default class Blog extends Component {
    costructor() {
        // ...

        this.state = {
            blogItems: []
        };

        this.getBlogItems = this.getBlogItems.bind(this);
    }

    getBlogItems() {
        axios.get(
            'https://nlarrea.devcamp.space/portfolio/porftolio_blogs',
            { withCredentials: true}
        ).then(response => {
            console.log('response', response);
        }).catch(error => {
            console.log('getBlogItems error', error);
        });
    }

    componentDidMount() {
        this.getBlogItems();
    }

    // ...
}
```

<br/>

Si abrimos la consola del navegador podremos ver cómo hemos realizado la petición a la API correctamente y hemos recibido los datos de los blogs que hemos creado.

Ahora, vamos a modificar el estado de nuestro componente para que guarde los datos que acabamos de recibir:

```js
// blog.js

// ...

export default class Blog extends Component {
    // ...

    getBlogItems() {
        axios(/* ... */)
        .then(response => {
            this.setState({
                blogItems: response.data.portfolio_blogs
            });
        }).catch(error => {
            // ...
        });
    }

    // ...
}
```

<br/>

Para comprobar si se ha modificado o no el estado de la app, podemos hacer uso de la extensión de Chrome `React Developer Tools`. Si abrimos la pestaña de `Components` y buscamos nuestro componente, podremos ver que el estado ha sido modificado correctamente.


<br/><hr/>
<hr/><br/>


## Mostrar los datos de los blogs

Ahora que sabemos que recibimos los datos de los blogs creados, vamos a mostrarlos en la aplicación. En primer lugar, vamos a crear una nueva constante dentro de la función `render()` que contenga una lista de los blogs formateados para mostrar lo que queramos de ellos.

Comenzaremos mostrando el título y comprobando que el código es correcto:

```js
// blog.js

// ...

export default class Blog extends Component {
    // ...

    render() {
        const blogRecords = this.state.blogItems.map(blogItem => {
            return <h1>{blogItem.title}</h1>;
        });

        return (
            <div>
                {blogRecords}
            </div>
        );
    }
}
```

<br/>

Si abrimos la aplicación, veremos que se muestran los títulos de los blogs que hemos creado.

A continuación, dado que se va a alargar bastante el contenido de dicha constante, lo que haremos será reorganizar el código, creando así un nuevo componente llamado `blog-item.js`.

Así es como quedaría el código de `blog.js`:

```js
// blog.js

// ...

import BlogItem from '../blog/blog-item';

export default class Blog extends Component {
    // ...

    render() {
        const blogRecords = this.state.blogItems.map(blogItem => {
            return <BlogItem key={blogItem.id} blogItem={blogItem} />;
        });

        return (
            <div>
                {blogRecords}
            </div>
        );
    }
}
```

<br/>

Y en el nuevo componente, en primer lugar mostraremos únicamente el título y el contenido de cada blog:

```js
// blog-item.js

import React from 'react';

const BlogItem = props => {
    const {
        id,
        title,
        content,
        blog_status,
        featured_image_url
    } = props.blogItem;

    return (
        <div>
            <h1>{title}</h1>

            <div>{content}</div>
        </div>
    )
}
```

<br/>

Accediendo a la app, veremos que debería mostrarse el título y el contenido de cada blog.