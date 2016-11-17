# TP 4: DSLs
## Generador de CSS

Desarrollar un DSL interno en Ruby que permita generar CSS, agregando algunos features de lenguaje que las hojas de estilo no poseen.

Toda la sintaxis planteada en los ejemplos es válida y realizable, pero si en algún punto se les vuelve muy complicada o no se les ocurre cómo lograrla, pueden proponer una sintaxis alternativa *siempre y cuando documenten y justifiquen su decisión*.

### Parte 1: clases, IDs y propiedades básicas de CSS

Queremos poder escribir en nuestro DSL:

```
body {
    background {
       color: rgb(255,0,255),
       width: 80.px,
       height: 80.px
    }
}

.claseSuelta {
    fontSize: 16.px
}

a.someClass :hover {
    color: blue
}

```

Y que esto nos genere:

```
body {
    background-color: #FF00FF;
    background-size: 80px 80px;
}

.claseSuelta {
    fontSize : 16px;
}

a.someClass :hover {
    color: blue;
}

```

### Parte 2: Variables

Queremos poder utilizar variables para tamaños y colores. Esto nos permitirá, entre otras cosas, crear temas configurando colores principales y derivar los colores de cada clase según cambios de tono.
Por ejemplo:

```
let fondo = rgb(255,0,255)

div.someDivClass {
    background {
       color: fondo
    }
}

footer {
    background {
       color: fondo
    }
}
```

Debería generar algo como:

```
div.someDivClass {
    background-color: #FF00FF;
}

footer {
    background-color: #FF00FF;
}

```

### Parte 3: Mixins

Queremos evitar repetir muchas veces las propiedades en diferentes clases de CSS. Para eso vamos a implementar mixins, de forma que se pueda definir un conjunto de propiedades tal que sea posible incluirlas en diferentes sitios:


```
mixin :noDecoration {
    display: block,
    textDecoration: none;
}

a {
    with: noDecoration
}

li.algo {
    with: noDecoration
}
```

Generando:


```
a {
    display: block;
    text-decoration: none;
}

li.algo {
    display: block;
    text-decoration: none;
}

```


### Bonus: dimensione y colores


Las variables pueden ser de tipo dimensión (píxeles) o color, y deben poder operarse aritméticamente, por ejemplo:

```
let deltaTono = rgb(40,40,40)

.claseClara {
    background {
        color: rgb(120,50,70) + deltaTono
    }
}

.claseOscura {
    background {
        color: rgb(120,50,70) - deltaTono
    }
}
```

Debe poder generar:

```
.claseClara {
    background-color: rgb(160,90,110);
}

.claseOscura {
    background-color: rgb(80,10,30);
}
```

### Bonus: parametrizar mixins

Dado un mixin:

```
mixin :espacios, :alto, :ancho {
    padding-top: alto
    padding-right: ancho
    padding-bottom: alto
    padding-left: ancho
}

div.conEspacios {
    with: espacios(20.px, 40.px)
}

```

Que genere:


```
div.conEspacios {
    padding-top: 20px;
    padding-right: 40px;
    padding-bottom: 20px;
    padding-left: 40px;
}

```
