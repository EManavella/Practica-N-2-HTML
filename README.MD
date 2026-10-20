# Práctica N°2 - CSS: Ejercicios 1 y 5

## Ejercicio 1: Responder

### 1. ¿Qué es CSS y para qué se usa?

**CSS** (*Cascading Style Sheets*, hojas de estilo en cascada) es un lenguaje que describe la presentación de los documentos escritos en HTML o XML: colores, tipografías, tamaños, márgenes, bordes, fondos y posición de los elementos.

Se usa para separar el contenido (HTML) de la presentación (CSS). Esto permite cambiar el aspecto de todo un sitio editando un solo archivo, mantener un diseño uniforme, reducir el código repetido y adaptar la misma página a distintos dispositivos. Su estándar lo mantiene el W3C.

### 2. CSS utiliza reglas para las declaraciones de estilo, ¿cómo funcionan?

Una regla tiene dos partes: un selector, que indica a qué elementos se aplica, y un bloque de declaraciones entre llaves. Cada declaración es un par `propiedad: valor;`.

```css
selector {
    propiedad: valor;
    propiedad: valor;
}

p {
    color: red;
    font-size: 14px;
}
```

En el ejemplo, la regla se aplica a todos los `<p>`: el navegador busca los elementos que coinciden con el selector y les aplica cada declaración. Cuando varias reglas afectan a un mismo elemento, se resuelve por la cascada (punto 7).

### 3. ¿Cuáles son las tres formas de dar estilo a un documento?

1. **En línea:** con el atributo `style` en la etiqueta. Afecta solo a ese elemento y tiene la mayor prioridad.
   ```html
   <p style="color: red;">Texto</p>
   ```
2. **Interna :** con un elemento `<style>` dentro del `<head>`. Afecta a todo el documento.
   ```html
   <style>
       p { color: red; }
   </style>
   ```
3. **Externa:** en un archivo `.css` aparte, vinculado con `<link>` en el `<head>` (o con `@import` dentro de un `<style>`). Se reutiliza en varias páginas y es la forma recomendada.
   ```html
   <link rel="stylesheet" type="text/css" href="estilos.css">
   ```

### 4. ¿Cuáles son los distintos tipos de selectores más utilizados? Ejemplifique cada uno.

| Tipo | Sintaxis | Qué selecciona | Ejemplo |
|---|---|---|---|
| Universal | `*` | Todos los elementos | `* { margin: 0; }` |
| De elemento (tipo) | `p` | Todos los elementos de ese tipo | `p { color: black; }` |
| De clase | `.nombre` | Elementos con `class="nombre"` | `.destacado { font-weight: bold; }` |
| De id | `#nombre` | El elemento con `id="nombre"` | `#menu { background: #ccc; }` |
| Compuesto | `p.nombre`, `p#nombre` | Elemento y clase/id a la vez | `p.aviso { color: red; }` |
| De agrupación | `A, B` | Los elementos de A y de B | `h1, h2, h3 { color: navy; }` |
| Descendiente | `A B` | B dentro de A, a cualquier nivel | `ul a { color: blue; }` |
| Hijo directo | `A > B` | B que es hijo directo de A | `ul > li { margin: 0; }` |
| Hermano adyacente | `A + B` | B que va justo después de A | `h1 + p { font-size: 120%; }` |
| Hermano general | `A ~ B` | B que sigue a A, mismo nivel | `h1 ~ p { color: gray; }` |
| De atributo | `[a]`, `[a=v]`, `[a^=v]`, `[a$=v]`, `[a*=v]` | Elementos según su atributo | `a[target="_blank"] { color: red; }` |
| Pseudo-clase | `:estado` | Elementos en un estado o posición | `li:first-child { color: red; }` |
| Pseudo-elemento | `::parte` | Una parte de un elemento | `p::first-line { font-weight: bold; }` |

### 5. ¿Qué es una pseudo-clase? ¿Cuáles son las más utilizadas aplicadas a vínculos?

Una pseudo-clase es una palabra clave que se agrega a un selector, precedida por `:`, para indicar un Estado o condición especial del elemento que no se puede expresar con su nombre, clase, id o atributos (por ejemplo, que el mouse esté encima o que sea el primer hijo).

Las más usadas con vínculos son:

| Pseudo-clase | Cuándo se aplica |
|---|---|
| `a:link` | Enlace que todavía no fue visitado |
| `a:visited` | Enlace ya visitado |
| `a:hover` | Mientras el mouse está encima |
| `a:active` | Mientras se hace clic |
| `a:focus` | Cuando el enlace tiene el foco (por ejemplo, con el teclado) |

El orden recomendado es **link, visited, hover, active** (regla LVHA), porque todas tienen la misma especificidad y la última declarada pisa a las anteriores.

```css
a:link    { color: blue; }
a:visited { color: purple; }
a:hover   { color: fuchsia; }
a:active  { color: red; }
```

### 6. ¿Qué es la herencia?

La herencia es el mecanismo por el cual un elemento toma el valor de algunas propiedades de su elemento padre cuando no tiene un valor propio asignado.

- **Se heredan** (en general, las de texto): `color`, `font-family`, `font-size`, `font-weight`, `line-height`, `text-align`, `list-style`, etc.
- **No se heredan** (en general, las de caja): `margin`, `padding`, `border`, `background`, `width`, `height`, etc.

Con el valor `inherit` se puede forzar la herencia de una propiedad.

```css
body { color: navy; font-family: Arial; }
```

Todos los elementos dentro del `body` (párrafos, listas, etc.) se ven en azul marino y con Arial sin declarar nada. Un valor heredado es el de menor prioridad: cualquier regla aplicada directamente al elemento lo reemplaza.

### 7. ¿En qué consiste el proceso denominado cascada?

La cascada es el proceso con el que el navegador decide qué valor usar cuando más de una regla afecta a la misma propiedad del mismo elemento. Resuelve el conflicto con estos criterios, en orden:

1. Origen e importancia: los estilos del autor ganan a los del navegador, y una declaración con `!important` gana a una normal.
2. especificidad: gana el selector más específico. De mayor a menor: estilo en línea, id (`#`), clases / atributos / pseudo-clases (`.`, `[ ]`, `:`) y, por último, elementos / pseudo-elementos. El `*` no suma.
3. Orden de aparición: si la especificidad es igual, gana la regla que aparece última.

Si ninguna regla se aplica directamente, se usa el valor heredado y, si no hay, el valor por defecto del navegador.

```css
p      { color: black; }   /* especificidad (0,0,1) */
.aviso { color: red; }     /* especificidad (0,1,0) */
```

En un `<p class="aviso">` el texto sale rojo, porque la clase es más específica que el elemento.

---

## Ejercicio 5: reglas CSS

### 1. Los textos enfatizados dentro de cualquier título deben ser rojos

```css
h1 em, h2 em, h3 em, h4 em, h5 em, h6 em {
    color: red;
}
```
Selector descendiente: un `<em>` dentro de cualquier título (`h1` a `h6`). Si también se quisiera incluir a `<strong>`, se agrega a la lista.

### 2. Cualquier elemento con el atributo `href`, dentro de un párrafo que está dentro de un bloque, debe ser negro

```css
div p [href] {
    color: black;
}
```
`[href]` selecciona cualquier elemento con ese atributo, `p` es el párrafo que lo contiene y `div` es el bloque. Hay un espacio antes de `[href]` para que se aplique a un descendiente del párrafo y no al propio párrafo.

### 3. Texto de las listas no ordenadas dentro del bloque "ultimo": amarillo; si es un enlace a otra página, azul

```css
#ultimo ul {
    color: yellow;
}
#ultimo ul a {
    color: blue;
}
```
`#ultimo` es el bloque identificado con `id="ultimo"`. El color amarillo se hereda al texto de los `<li>`. La segunda regla es más específica y pisa el color de los enlaces (`<a>`).

### 4. Los elementos "importante" dentro de cualquier bloque deben ser verdes, pero rojos si están dentro de un título

```css
div .importante {
    color: green;
}
h1 .importante, h2 .importante, h3 .importante,
h4 .importante, h5 .importante, h6 .importante {
    color: red;
}
```
Las dos reglas tienen la misma especificidad `(0,1,1)`, por eso la de los títulos va después: si un elemento cumple las dos condiciones, gana la última. Se asumió que "importante" es una clase; si fuera un id, se reemplaza `.importante` por `#importante`.

### 5. Todos los `h1` que especifiquen el atributo `title`, con cualquier valor, deben ser azules

```css
h1[title] {
    color: blue;
}
```
El selector de atributo `[title]` sin valor selecciona a los elementos que tengan el atributo, sea cual sea su valor.

### 6. Enlaces en listas ordenadas: azules si no fueron visitados, violetas si ya se visitaron, y sin subrayado

```css
ol a {
    text-decoration: none;
}
ol a:link {
    color: blue;
}
ol a:visited {
    color: violet;
}
```
`ol a` alcanza a los enlaces dentro de listas ordenadas. `:link` es el enlace no visitado y `:visited` el visitado. El `text-decoration: none` se declara una sola vez y vale para ambos estados.