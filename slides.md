---
# try also 'default' to start simple
theme: apple-basic
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
layout: intro-image-right
image: './images/qr-slides.png'
---

# ¿Y si Python entendiera español?

Introducción práctica a modificar CPython

<div class="absolute bottom-10">
  <a href="sofietorch.github.io/pycones_slides">sofietorch.github.io/pycones_slides</a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade
layout: intro-image-right
image: './images/qr-slides.png'
---

# El plan de hoy

* Intro a CPython
* Compilar nuestro propio Python
* ¿Cómo funciona la grammar?
* (Pequeña) Intro a C
* Cambiar keywords y funciones a español
* Perderle el miedo a CPython :)

<div class="absolute bottom-10">
  <a href="sofietorch.github.io/pycones_slides">sofietorch.github.io/pycones_slides</a>
</div>

---
transition: fade
layout: intro-image-right
image: './images/qr-slides.png'
---

# Antes de comenzar

* Instalar WSL (si están en Windows)
* Editor de texto listo
* Preparar el repo de CPython (<Link to="9">Ver slide 9</Link>)

<div class="absolute bottom-10">
  <a href="sofietorch.github.io/pycones_slides">sofietorch.github.io/pycones_slides</a>
</div>

---
transition: fade
layout: intro-image-right
image: './images/sofi.jpeg'
---

# Sofi Toro

* Software engineer @Synics
* Former Google Developer Student Club Lead
* Former organizer @GDG Cochabamba, JS Bolivia & Hub Boliviano de IA
* Me gusta cantar, ir de hiking y charlar :)

---
transition: fade
---

# ¿CPython?

<div class="mb-3"><i>¿No era Cython?</i></div>

* Runtime/entorno de ejecución de Python
* La implementación "oficial" de Python
* Cuando escribes `python` en la consola, CPython se ejecuta (si lo instalaste desde _python.org_)

---
transition: fade
---

# `cpython/`

<v-clicks>

|                           |                                           |
| ------------------------- | ----------------------------------------- |
| <kbd>Doc</kbd>            | Fuente de la documentación                |
| <kbd>Grammar</kbd>        | Definición del lenguaje                   |
| <kbd>Lib</kbd>            | Los módulos estándar escritos en Python   |
| <kbd>Modules</kbd>        | Los módulos estándar escritos en C        |
| <kbd>Objects</kbd>        | Object model y tipos core                 |
| <kbd>Parser</kbd>         | El parser de Python                       |
| <kbd>Python</kbd>         | El intérprete de CPython                  |

</v-clicks>

---
transition: fade
---

# ¿Y qué más?

* <span v-mark.highlight.green>Compila</span> de Python a bytecode (archivos `.pyc` ocultos)
* Interpreta y ejecuta el bytecode con la PVM (Python Virtual Machine)
  
>Fun fact: el bytecode se guarda en caché para ser ejecutado, por lo que si corres una app Python dos veces sin cambiar el código, la 2da vez siempre será más rápida.

---
transition: fade
---

# ¿Por qué en C?

<div class="mb-3"><i>(y no en Python)</i></div>

<v-clicks>

* Self-hosted compilers
* Source-to-source compilers
* Alternativas: PyPy, Jython, Cython
* El primer paso para crear un compilador es definir el lenguaje
  
</v-clicks>

---
transition: fade
layout: center
---

# Compilando CPython (3.9)

1. `$ git clone --branch 3.9 https://github.com/python/cpython`
2. `$ cd cpython`
3. `$ ./configure --with-pydebug`
4. `$ make -j2 -s`

<v-click>

🎉
```
$ ./python
Python 3.9.24+ (heads/3.9:9c4638d1b29, Oct 16 2025, 19:25:25) 
[GCC 13.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

```

</v-click>

---
transition: fade
---

# The grammar file

`Grammar/python.gram` Define las reglas para la estructura del lenguaje.

* \* para repetir
* \+ para al menos una repetición
* [] para partes opcionales
* | para alternativas
* () para agrupar

---
transition: fade
layout: intro-image-right
---

# Definiendo un café


* Debe tener una taza
* Debe tener al menos un shot de espresso, pero puede tener varios shots
* Puede tener leche, pero es opcional
* Puede tener agua, pero es opcional
* Si tiene leche, la leche puede ser entera, deslactosada o de soya.

<v-click>

```
coffee: 'cup' ('espresso')+ ['water'] [milk]
milk: 'full-fat' | 'skimmed' | 'soy'
```

</v-click>

<v-click>

<img src="/images/coffe-railroad-diag.png" class="absolute right-10 h-7.5em top-50"/>

</v-click>

---
transition: fade
layout: two-cols
---

# `while`

Expresión + `:` + bloque de código:
```python
while finished == True:
  do_something()
```

<v-click>

Expresión de asignación (`named_expression` en el grammar):
```python
while letters := read(document,  10):
  print(letters)
```

</v-click>

<v-click>

Opcionalmente, puede seguirle `else` con un bloque de código:
```python
while item := next(iterable):
  print(item)
else:
  print("Iterable is empty")
```

</v-click>

::right::

<div class="mt-22"></div>

<v-click>

`while_stmt`:

```
while_stmt[stmt_ty]:
  | 'while' a=named_expression ':' b=block c=[else_bl...
```

<img src="/images/while-railroad-diag.png" class="my-6"/>

</v-click>

<v-click>

<span v-mark.underline.green="4">
<code>try_stmt</code>, <code>if_stmt</code>
</span>

</v-click>

<style>
.two-columns {
  column-gap: 1.7em;
}
</style>


---
transition: fade
layout: center
---

# Regenerando la gramática

`python.gram` > `small_stmt`

````md magic-move {lines: true}
```txt [python.gram] {*|7}
small_stmt[stmt_ty] (memo):
| assignment
| e=star_expressions { _Py_Expr(e, EXTRA) }
| &'return' return_stmt
| &('import' | 'from') import_stmt
| &'raise' raise_stmt
| 'pass' { _Py_Pass(EXTRA) }
| &'del' del_stmt
| &'yield' yield_stmt
| &'assert' assert_stmt
| 'break' { _Py_Break(EXTRA) }
| 'continue' { _Py_Continue(EXTRA) }
| &'global' global_stmt
| &'nonlocal' nonlocal_stmt
```

```txt [python.gram] {7}
small_stmt[stmt_ty] (memo):
| assignment
| e=star_expressions { _Py_Expr(e, EXTRA) }
| &'return' return_stmt
| &('import' | 'from') import_stmt
| &'raise' raise_stmt
| ('pass' | 'pasar') { _Py_Pass(EXTRA) }
| &'del' del_stmt
| &'yield' yield_stmt
| &'assert' assert_stmt
| 'break' { _Py_Break(EXTRA) }
| 'continue' { _Py_Continue(EXTRA) }
| &'global' global_stmt
| &'nonlocal' nonlocal_stmt
```
````

---
transition: fade
layout: center
---

# Regenerando la gramática

<div class="mt-6"></div>

Regenerar el parser

```
$ make regen-pegen
```

Recompilar Python

```
$ ./configure --with-pydebug
$ make -j2 -s
```

---
transition: fade
layout: center
---

# Regenerando la gramática

<div class="mt-6"></div>

Corriendo Python

```bash
$ ./python
>>> def boo():
...    pasar
...
>>> pasar()
>>>
```

---
transition: fade
layout: center
---

# "Referenced" statements

<div class="mt-6"></div>

````md magic-move
```txt [python.gram] {*|4,10}
small_stmt[stmt_ty] (memo):
| assignment
| e=star_expressions { _Py_Expr(e, EXTRA) }
| &'return' return_stmt
| &('import' | 'from') import_stmt

...

return_stmt[stmt_ty]:
| 'return' a=[star_expressions] { _Py_Return(a, EXTRA) }
```

```txt [python.gram] {4,10-11|*}
small_stmt[stmt_ty] (memo):
| assignment
| e=star_expressions { _Py_Expr(e, EXTRA) }
| &('return'|'retornar') return_stmt
| &('import' | 'from') import_stmt

...

return_stmt[stmt_ty]:
| 'return' a=[star_expressions] { _Py_Return(a, EXTRA) }
| 'retornar' a=[star_expressions] { _Py_Return(a, EXTRA) }
```
````

<v-click>
Recompilar (again)
```bash
$ make regen-pegen
$ ./configure --with-pydebug
$ make -j2 -s
```

</v-click>

---
transition: fade
---

# What is Slidev?

Slidev is a slides maker and presenter designed for developers, consist of the following features

- 📝 **Text-based** - focus on the content with Markdown, and then style them later
- 🎨 **Themable** - themes can be shared and re-used as npm packages
- 🧑‍💻 **Developer Friendly** - code highlighting, live coding with autocompletion
- 🤹 **Interactive** - embed Vue components to enhance your expressions
- 🎥 **Recording** - built-in recording and camera view
- 📤 **Portable** - export to PDF, PPTX, PNGs, or even a hostable SPA
- 🛠 **Hackable** - virtually anything that's possible on a webpage is possible in Slidev
<br>
<br>

Read more about [Why Slidev?](https://sli.dev/guide/why)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
transition: slide-up
level: 2
---

# Navigation

Hover on the bottom-left corner to see the navigation's controls panel, [learn more](https://sli.dev/guide/ui#navigation-bar)

## Keyboard Shortcuts

|                                                     |                             |
| --------------------------------------------------- | --------------------------- |
| <kbd>right</kbd> / <kbd>space</kbd>                 | next animation or slide     |
| <kbd>left</kbd>  / <kbd>shift</kbd><kbd>space</kbd> | previous animation or slide |
| <kbd>up</kbd>                                       | previous slide              |
| <kbd>down</kbd>                                     | next slide                  |

<!-- https://sli.dev/guide/animations.html#click-animation -->
<img
  v-click
  class="absolute -bottom-9 -left-7 w-80 opacity-50"
  src="https://sli.dev/assets/arrow-bottom-left.svg"
  alt=""
/>
<p v-after class="absolute bottom-23 left-45 opacity-30 transform -rotate-10">Here!</p>

---
layout: two-cols
layoutClass: gap-16
---

# Table of contents

You can use the `Toc` component to generate a table of contents for your slides:

```html
<Toc minDepth="1" maxDepth="1" />
```

The title will be inferred from your slide content, or you can override it with `title` and `level` in your frontmatter.

::right::

<Toc text-sm minDepth="1" maxDepth="2" />

---
layout: image-right
image: https://cover.sli.dev
---

# Code

Use code snippets and get the highlighting directly, and even types hover!

```ts [filename-example.ts] {all|4|6|6-7|9|all} twoslash
// TwoSlash enables TypeScript hover information
// and errors in markdown code blocks
// More at https://shiki.style/packages/twoslash
import { computed, ref } from 'vue'

const count = ref(0)
const doubled = computed(() => count.value * 2)

doubled.value = 2
```

<arrow v-click="[4, 5]" x1="350" y1="310" x2="195" y2="342" color="#953" width="2" arrowSize="1" />

<!-- This allow you to embed external code blocks -->
<<< @/snippets/external.ts#snippet

<!-- Footer -->

[Learn more](https://sli.dev/features/line-highlighting)

<!-- Inline style -->
<style>
.footnotes-sep {
  @apply mt-5 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---
level: 2
---

# Shiki Magic Move

Powered by [shiki-magic-move](https://shiki-magic-move.netlify.app/), Slidev supports animations across multiple code snippets.

Add multiple code blocks and wrap them with <code>````md magic-move</code> (four backticks) to enable the magic move. For example:

````md magic-move {lines: true}
```ts {*|2|*}
// step 1
const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})
```

```ts {*|1-2|3-4|3-4,8}
// step 2
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
    }
  }
}
```

```ts
// step 3
export default {
  data: () => ({
    author: {
      name: 'John Doe',
      books: [
        'Vue 2 - Advanced Guide',
        'Vue 3 - Basic Guide',
        'Vue 4 - The Mystery'
      ]
    }
  })
}
```

Non-code blocks are ignored.

```vue
<!-- step 4 -->
<script setup>
const author = {
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
}
</script>
```
````

---

# Components

<div grid="~ cols-2 gap-4">
<div>

You can use Vue components directly inside your slides.

We have provided a few built-in components like `<Tweet/>` and `<Youtube/>` that you can use directly. And adding your custom components is also super easy.

```html
<Counter :count="10" />
```

<!-- ./components/Counter.vue -->
<Counter :count="10" m="t-4" />

Check out [the guides](https://sli.dev/builtin/components.html) for more.

</div>
<div>

```html
<Tweet id="1390115482657726468" />
```

<Tweet id="1390115482657726468" scale="0.65" />

</div>
</div>

<!--
Presenter note with **bold**, *italic*, and ~~striked~~ text.

Also, HTML elements are valid:
<div class="flex w-full">
  <span style="flex-grow: 1;">Left content</span>
  <span>Right content</span>
</div>
-->

---
class: px-20
---

# Themes

Slidev comes with powerful theming support. Themes can provide styles, layouts, components, or even configurations for tools. Switching between themes by just **one edit** in your frontmatter:

<div grid="~ cols-2 gap-2" m="t-2">

```yaml
---
theme: default
---
```

```yaml
---
theme: seriph
---
```

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-default/01.png?raw=true" alt="">

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-seriph/01.png?raw=true" alt="">

</div>

Read more about [How to use a theme](https://sli.dev/guide/theme-addon#use-theme) and
check out the [Awesome Themes Gallery](https://sli.dev/resources/theme-gallery).

---

# Clicks Animations

You can add `v-click` to elements to add a click animation.

<div v-click>

This shows up when you click the slide:

```html
<div v-click>This shows up when you click the slide.</div>
```

</div>

<br>

<v-click>

The <span v-mark.red="3"><code>v-mark</code> directive</span>
also allows you to add
<span v-mark.circle.orange="4">inline marks</span>
, powered by [Rough Notation](https://roughnotation.com/):

```html
<span v-mark.underline.orange>inline markers</span>
```

</v-click>

<div mt-20 v-click>

[Learn more](https://sli.dev/guide/animations#click-animation)

</div>

---

# Motions

Motion animations are powered by [@vueuse/motion](https://motion.vueuse.org/), triggered by `v-motion` directive.

```html
<div
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }"
  :click-3="{ x: 80 }"
  :leave="{ x: 1000 }"
>
  Slidev
</div>
```

<div class="w-60 relative">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-square.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-circle.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-triangle.png"
      alt=""
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 30, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn more](https://sli.dev/guide/animations.html#motion)

</div>

---

# LaTeX

LaTeX is supported out-of-box. Powered by [KaTeX](https://katex.org/).

<div h-3 />

Inline $\sqrt{3x-1}+(1+x)^2$

Block
$$ {1|3|all}
\begin{aligned}
\nabla \cdot \vec{E} &= \frac{\rho}{\varepsilon_0} \\
\nabla \cdot \vec{B} &= 0 \\
\nabla \times \vec{E} &= -\frac{\partial\vec{B}}{\partial t} \\
\nabla \times \vec{B} &= \mu_0\vec{J} + \mu_0\varepsilon_0\frac{\partial\vec{E}}{\partial t}
\end{aligned}
$$

[Learn more](https://sli.dev/features/latex)

---
dragPos:
  square: 691,32,167,_,-16
---

# Diagrams

You can create diagrams / graphs from textual descriptions, directly in your Markdown.

<div class="grid grid-cols-4 gap-5 pt-4 -mb-6">

```mermaid {scale: 0.5, alt: 'A simple sequence diagram'}
sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```

```mermaid {theme: 'neutral', scale: 0.8}
graph TD
B[Text] --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```mermaid
mindmap
  root((mindmap))
    Origins
      Long history
      ::icon(fa fa-book)
      Popularisation
        British popular psychology author Tony Buzan
    Research
      On effectiveness<br/>and features
      On Automatic creation
        Uses
            Creative techniques
            Strategic planning
            Argument mapping
    Tools
      Pen and paper
      Mermaid
```

```plantuml {scale: 0.7}
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}

database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}

[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

</div>

Learn more: [Mermaid Diagrams](https://sli.dev/features/mermaid) and [PlantUML Diagrams](https://sli.dev/features/plantuml)

---
foo: bar
dragPos:
  square: 691,32,167,_,-16
---

# Draggable Elements

Double-click on the draggable elements to edit their positions.

<br>

###### Directive Usage

```md
<img v-drag="'square'" src="https://sli.dev/logo.png">
```

<br>

###### Component Usage

```md
<v-drag text-3xl>
  <div class="i-carbon:arrow-up" />
  Use the `v-drag` component to have a draggable container!
</v-drag>
```

<v-drag pos="663,206,261,_,-15">
  <div text-center text-3xl border border-main rounded>
    Double-click me!
  </div>
</v-drag>

<img v-drag="'square'" src="https://sli.dev/logo.png">

###### Draggable Arrow

```md
<v-drag-arrow two-way />
```

<v-drag-arrow pos="67,452,253,46" two-way op70 />

---
src: ./pages/imported-slides.md
hide: false
---

---

# Monaco Editor

Slidev provides built-in Monaco Editor support.

Add `{monaco}` to the code block to turn it into an editor:

```ts {monaco}
import { ref } from 'vue'
import { emptyArray } from './external'

const arr = ref(emptyArray(10))
```

Use `{monaco-run}` to create an editor that can execute the code directly in the slide:

```ts {monaco-run}
import { version } from 'vue'
import { emptyArray, sayHello } from './external'

sayHello()
console.log(`vue ${version}`)
console.log(emptyArray<number>(10).reduce(fib => [...fib, fib.at(-1)! + fib.at(-2)!], [1, 1]))
```

---
layout: center
class: text-center
---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

<PoweredBySlidev mt-10 />
