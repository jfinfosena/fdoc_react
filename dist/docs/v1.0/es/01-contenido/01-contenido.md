---
title: "Introducción a React y Configuración del Entorno"
position: 1
date: 2025-10-13
---

+++hero-section
---
title: "¡Bienvenido al mundo de React!"
subtitle: "En esta primera clase, sentaremos las bases para tu viaje con React. Entenderemos qué es esta poderosa biblioteca, por qué es tan popular, y daremos los primeros pasos prácticos configurando nuestro entorno de desarrollo."
backgroundImage: "https://images.unsplash.com/photo-1633356122544-f134324a6cee?q=80&w=2070"
overlayOpacity: 0.7
buttons:
  - text: "Ver Código Ejemplo"
    url: "#ejemplo-practico"
    variant: "primary"
    icon: "CodeBracketIcon"
  - text: "Siguiente Lección"
    url: "/v1.0/es/01-contenido/02-contenido"
    variant: "secondary"
    icon: "ArrowRightIcon"
---
+++

##  Fundamentos Teóricos y Contexto

### ¿Qué es React?

*   React es una biblioteca (library) de JavaScript de código abierto, creada y mantenida por Meta (Facebook) y una gran comunidad.
*   Su objetivo principal es ayudarte a construir interfaces de usuario (UIs) de forma eficiente, especialmente para aplicaciones web complejas e interactivas.
*   La idea central de React es dividir la interfaz en pequeñas piezas reutilizables llamadas componentes. Piensa en ellos como bloques de LEGO: cada uno tiene su propia lógica y apariencia, y los combinas para crear interfaces completas.

+++admonition
---
type: "info"
title: "¿Por qué React?"
---
React es la biblioteca más popular para construir interfaces de usuario, con más de 40% de desarrolladores frontend usándola según las encuestas de Stack Overflow. Su enfoque en componentes reutilizables y el Virtual DOM la hacen ideal para aplicaciones modernas.
+++

### ¿Por Qué Usar React? Principales Beneficios

*   Declarativo: En lugar de decirle al navegador `cómo` actualizar la pantalla paso a paso (imperativo), le dices a React `qué` quieres mostrar basado en los datos actuales (el estado). React se encarga de hacer los cambios necesarios de manera eficiente. Esto hace el código más predecible y fácil de depurar.
*   Basado en Componentes: Facilita la reutilización de código (escribes un componente una vez y lo usas muchas veces), mejora la organización del proyecto y simplifica las pruebas.
*   Rendimiento (Gracias al Virtual DOM):
    *   React usa una copia virtual de la estructura de tu UI en memoria (el "Virtual DOM").
    *   Cuando los datos cambian, React compara la nueva versión del Virtual DOM con la anterior.
    *   Luego, calcula la forma más eficiente de actualizar el DOM `real` del navegador, minimizando operaciones costosas y haciendo que tu aplicación se sienta más rápida. No necesitas interactuar directamente con el Virtual DOM, ¡React lo maneja por ti!
*   Gran Ecosistema y Comunidad: Hay muchísimas herramientas, librerías adicionales, tutoriales y una comunidad activa dispuesta a ayudar. Esto también significa que hay muchas oportunidades laborales para desarrolladores React.

### Conceptos Clave Iniciales

*   SPA (Single Page Application): Aplicaciones que cargan una sola página HTML y luego actualizan el contenido dinámicamente usando JavaScript, sin necesidad de recargar toda la página al navegar. React es excelente para construir SPAs, lo que resulta en una experiencia de usuario más fluida. Contrasta con las MPAs (Multi-Page Applications) tradicionales, donde cada cambio de página implica una recarga completa desde el servidor.
*   Virtual DOM: (Repaso) Es la representación interna que React usa para optimizar las actualizaciones del DOM real del navegador.

### Alternativas a React

Existen otros frameworks y bibliotecas populares para construir UIs:

*   Angular: Un framework completo de Google, usa TypeScript y tiene una estructura más definida ("opinionado").
*   Vue.js: Un framework progresivo, conocido por su facilidad de aprendizaje y excelente documentación.
*   Svelte: Un enfoque diferente; es un compilador que escribe JavaScript eficiente y desaparece en tiempo de ejecución.

No hay una opción "mejor" universalmente; la elección depende del proyecto. React destaca por su flexibilidad, enfoque en componentes y gran ecosistema.

### Prerrequisitos

Para seguir este curso, necesitas conocimientos sólidos de:

*   HTML y CSS: Fundamentales para estructurar y estilizar páginas web.
*   JavaScript (Moderno - ES6+): ¡Esencial! Necesitas entender:
    *   Variables: `let`, `const`
    *   Funciones Flecha: `=>`
    *   Métodos de Array: `.map()`, `.filter()` (¡`map` es crucial!)
    *   Módulos: `import`/`export`
    *   (Idealmente) Promesas, `async/await` (los usaremos más adelante).
*   Node.js y npm:
    *   Node.js: Entorno para ejecutar JavaScript fuera del navegador. Lo necesitamos para las herramientas de desarrollo de React.
    *   npm (Node Package Manager): Gestor de paquetes que viene con Node.js. Lo usaremos para instalar React y otras librerías (dependencias).

*   Un Editor de Código: VS Code es muy recomendado por sus extensiones útiles para React/JavaScript, pero puedes usar Sublime Text, Atom, WebStorm, etc.

### Instalación de Node.js y npm

+++steps
### Paso 1: Descargar Node.js
Ve a [https://nodejs.org/](https://nodejs.org/) y descarga la versión LTS (Long Term Support) – es la más estable y recomendada.

### Paso 2: Instalar Node.js
Ejecuta el instalador descargado y sigue los pasos del asistente. Node.js incluye npm automáticamente.

### Paso 3: Verificar la instalación
Abre tu terminal (Terminal en Mac/Linux, CMD/PowerShell/Git Bash en Windows) y ejecuta estos comandos:

node -v
npm -v

Si ves las versiones, ¡estás listo! Si tienes problemas, pide ayuda.
+++

---

##  Creando Proyecto React con Vite

### Explicación paso a paso

+++steps
### Paso 1: Verificar requisitos previos
Asegúrate de tener Node.js instalado (versión 14.18+ o superior). Puedes verificarlo con:

  
`node --version`

### Paso 2: Crear proyecto con Vite
Vite ofrece plantillas preconfiguradas para React. Usa el siguiente comando:

`npm create vite@latest`

Esto inicia un asistente interactivo donde seleccionarás:
- **Nombre del proyecto**: `mi-proyecto-react`
- **Framework**: `React`
- **Variante**: `JavaScript` (para este ejemplo)

### Paso 3: Comando alternativo
Para evitar el asistente, puedes usar un solo comando:

`npm create vite@latest mi-proyecto-react -- --template react`

### Paso 4: Navegar al proyecto

`cd mi-proyecto-react`

### Paso 5: Instalar dependencias

`npm install`

### Paso 6: Iniciar servidor de desarrollo

`npm run dev`


Esto inicia un servidor local (generalmente en `http://localhost:5173`). Abre la URL en tu navegador para ver la aplicación React predeterminada.
+++

### Estructura del proyecto
Una vez creado, el proyecto tendrá una estructura similar a esta:

+++file-tree
mi-proyecto-react/
├── node_modules/
├── public/
│   └── vite.svg
├── src/
│   ├── assets/
│   ├── App.jsx
│   ├── main.jsx
│   ├── App.css
│   └── index.css
├── .gitignore
├── index.html
├── package.json
└── vite.config.js
+++

**Archivos principales:**
- `src/main.jsx`: Punto de entrada que renderiza la aplicación
- `src/App.jsx`: Componente principal de React
- `vite.config.js`: Configuración de Vite

### Personalizar el proyecto
Edita `src/App.jsx` para agregar tu propio código. Por ejemplo, reemplaza su contenido con:

```jsx
import { useState } from 'react';
import './App.css';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <h1>Contador con React y Vite</h1>
      <p>Has hecho clic {count} veces</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </div>
  );
}

export default App;
```

**Explicación:**
- Este código crea un contador simple que incrementa un valor al hacer clic en un botón
- El servidor de Vite recargará automáticamente los cambios en el navegador

### Comandos de producción
Cuando estés listo para desplegar, usa estos comandos:

```bash
# Construir para producción
npm run build

# Previsualizar la build
npm run preview
```

- Los archivos optimizados estarán en la carpeta `dist/`
- `npm run preview` te permite verificar la versión de producción localmente

## Ejemplo completo
Supongamos que quieres crear un proyecto llamado `contador-app`. Ejecuta estos comandos:

```bash
npm create vite@latest contador-app -- --template react
cd contador-app
npm install
npm run dev
```

Luego, edita `src/App.jsx` con el código del contador mostrado arriba. Abre `http://localhost:5173` en tu navegador y verás un contador funcional.

### URL oficial de Vite
La documentación y recursos oficiales están disponibles en:
- [https://vitejs.dev/](https://vitejs.dev/)

+++admonition
---
type: "success"
title: "Ventajas de Vite con React"
---
- **Rápido**: Vite usa ES Modules y un servidor de desarrollo ultrarrápido
- **Configuración mínima**: Plantillas listas para usar
- **Hot Module Replacement (HMR)**: Actualizaciones en tiempo real sin recargar la página
- **Optimizado para producción**: Genera builds pequeñas y eficientes
+++


##  Creando Proyecto React con Next.js

### Explicación paso a paso

+++steps
### Paso 1: Verificar requisitos previos
Asegúrate de tener **Node.js** instalado (versión 18.17 o superior). Verifícalo con:

`node -v`

### Paso 2: Crear proyecto con Next.js
Next.js proporciona un comando para inicializar un proyecto. Ejecuta:

`npx create-next-app@latest`

Esto inicia un asistente interactivo donde seleccionarás:
- **Nombre del proyecto**: `mi-proyecto-next`
- **TypeScript**: `No` (para este ejemplo usaremos JavaScript)
- **ESLint**: `Yes` para activar linting
- **Tailwind CSS**: `No` para mantenerlo simple
- **src directory**: `No` para usar la estructura estándar
- **App Router**: `Yes` para usar el nuevo App Router (recomendado)
- **Customize default import alias**: `No` para simplicidad

### Paso 3: Comando alternativo
Para evitar el asistente, puedes usar un solo comando:


`npx create-next-app@latest mi-proyecto-next --javascript --eslint --no-tailwind --no-src-dir --app --import-alias "@/*"`

### Paso 4: Navegar al proyecto

`cd mi-proyecto-next`

### Paso 5: Iniciar servidor de desarrollo

`npm run dev`

Esto inicia el servidor en `http://localhost:3000`. Abre la URL en tu navegador para ver la página predeterminada de Next.js.
+++

### Estructura del proyecto Next.js
Con el App Router, la estructura será similar a esta:

+++file-tree
mi-proyecto-next/
├── node_modules/
├── public/
│   ├── next.svg
│   └── vercel.svg
├── app/
│   ├── globals.css
│   ├── layout.jsx
│   ├── page.jsx
│   └── favicon.ico
├── .gitignore
├── next.config.mjs
├── package.json
├── README.md
└── .eslintrc.json
+++

**Archivos principales:**
- `app/page.jsx`: Página principal de la aplicación
- `app/layout.jsx`: Layout raíz que envuelve todas las páginas
- `next.config.mjs`: Configuración de Next.js

### Personalizar el proyecto Next.js
Edita `app/page.jsx` para crear una página con un contador simple. Reemplaza su contenido con:

```jsx
'use client'; // Indica que este componente se renderiza en el cliente

import { useState } from 'react';
import styles from './page.module.css';

export default function Home() {
  const [count, setCount] = useState(0);

  return (
    <main className={styles.main}>
      <h1>Contador con Next.js</h1>
      <p>Has hecho clic {count} veces</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </main>
  );
}
```

**Explicación:**
- `'use client'`: Necesario para componentes que usan hooks como `useState` en el App Router, ya que Next.js renderiza por defecto en el servidor
- `styles.main`: Usa los estilos CSS Modules generados automáticamente en `page.module.css`

### Ajustar los estilos (opcional)
En `app/page.module.css`, puedes modificar o agregar estilos, por ejemplo:

```css
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  gap: 1rem;
}
```

### Comandos de producción Next.js
Para generar una versión optimizada y previsualizarla:

```bash
# Construir para producción
npm run build

# Iniciar la aplicación en producción
npm run start
```

- Los archivos optimizados se generan en la carpeta `.next/`
- `npm run start` previsualiza la build localmente

### Ejemplo completo Next.js
Para crear un proyecto llamado `contador-next-app`, ejecuta:

```bash
npx create-next-app@latest contador-next-app --javascript --eslint --no-tailwind --no-src-dir --app --import-alias "@/*"
cd contador-next-app
npm run dev
```

Edita `app/page.jsx` con el código del contador mostrado arriba. Abre `http://localhost:3000` en tu navegador y verás un contador funcional.

### URL oficial de Next.js
La documentación y recursos oficiales están disponibles en:
- [https://nextjs.org/](https://nextjs.org/){target = "_blank"}

+++admonition
---
type: "success"
title: "Ventajas de Next.js"
---
- **Renderizado híbrido**: Soporta SSR, SSG y CSR (renderizado del lado del cliente)
- **Optimización automática**: Compresión de imágenes, precarga de recursos y más
- **App Router**: Estructura moderna para manejar rutas y layouts
- **API Routes**: Crea endpoints API fácilmente dentro del proyecto
- **Ecosistema robusto**: Integración con Vercel, Tailwind CSS, y más
+++

+++admonition
---
type: "info"
title: "Diferencias: Next.js vs Vite"
---
- **Next.js**: Framework completo con enrutamiento, renderizado del servidor y optimizaciones integradas
- **Vite**: Herramienta de construcción más ligera enfocada en velocidad
- **Next.js** es ideal para aplicaciones que requieren SEO o renderizado del servidor
- **Vite** es mejor para aplicaciones cliente ligeras
+++

---

## Extensiones de VS Code Específicas para React

+++cards
---
columns: 2
items:
  - title: "ES7+ React/Redux/React-Native snippets"
    icon: "CodeBracketIcon"
    href: "https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets"
    content: "Proporciona snippets para React, Redux, React Native y GraphQL, incluyendo componentes funcionales, hooks y boilerplate común. Acelera la escritura de código React (ej. `rafce` para un componente funcional con exportación)."
  - title: "Simple React Snippets"
    icon: "LightningBoltIcon"
    href: "https://marketplace.visualstudio.com/items?itemName=burkeholland.simple-react-snippets"
    content: "Ofrece snippets minimalistas para React, como importaciones, componentes funcionales y PropTypes, sin abarcar Redux o React Native. Ideal para principiantes o proyectos React puros."
  - title: "Auto Rename Tag"
    icon: "ArrowsPointingOutIcon"
    href: "https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag"
    content: "Renombra automáticamente etiquetas JSX/HTML emparejadas al editar una, evitando errores en el código. Simplifica la edición de JSX en React, especialmente en componentes complejos."
  - title: "VSCode React Refactor"
    icon: "ArrowPathIcon"
    href: "https://marketplace.visualstudio.com/items?itemName=planbcoding.vscode-react-refactor"
    content: "Facilita la refactorización de código JSX, permitiendo extraer fragmentos a funciones o archivos separados. Mantiene el código React organizado, especialmente en proyectos grandes."
---
+++

---

## ¿Qué es JSX?

**JSX** (JavaScript XML) es una extensión de sintaxis para JavaScript que permite escribir código similar a HTML dentro de archivos JavaScript. Es una característica clave de **React**, utilizada para describir la interfaz de usuario (UI) de manera declarativa.

```admonition
---
type: "info"
title: "Características clave de JSX"
---
- **Creado por**: Facebook, como parte de React
- **Propósito**: Facilitar la creación de componentes de UI combinando lógica (JavaScript) y estructura (HTML-like) en un solo lugar
- **No es HTML**: JSX es una abstracción que se compila a JavaScript puro, lo que lo hace más flexible y potente
- **Azúcar sintáctico**: Se transforma en llamadas a funciones JavaScript (específicamente, a `React.createElement`)
```

---

## ¿Cómo funciona JSX?

JSX permite escribir estructuras de UI que se asemejan a HTML, pero bajo el capó, se convierten en objetos JavaScript que React usa para construir el DOM virtual. Aquí está el proceso:

### 1. Escribir JSX
```jsx
const element = <h1>Hola, React!</h1>;
```

### 2. Compilación con Babel
JSX no es interpretado directamente por los navegadores. Herramientas como **Babel** (usadas automáticamente en Vite y Next.js) lo transforman en JavaScript:

```javascript
const element = React.createElement("h1", null, "Hola, React!");
```

### 3. Renderizado
React toma estas llamadas a `React.createElement` para crear un **DOM virtual** (una representación ligera del DOM real). Luego, React compara el DOM virtual con el DOM real y actualiza solo las partes que cambiaron (reconciliación).

### 4. Integración en componentes
JSX se usa dentro de componentes React para definir su estructura. Por ejemplo:

```jsx
function Welcome() {
  return <h1>Bienvenido a React</h1>;
}
```

---

## Características principales de JSX

### 1. Sintaxis similar a HTML
- Usa etiquetas como `<div>`, `<h1>`, o componentes personalizados (`<MyComponent />`)
- Soporta atributos como `className`, `onClick`, etc.

### 2. Expresiones JavaScript
Puedes insertar código JavaScript dentro de JSX usando llaves `{}`:

```jsx
const name = "Juan";
const element = <h1>Hola, {name}!</h1>;
```

### 3. Componentes como etiquetas
Los componentes React (funcionales o de clase) se usan como etiquetas JSX:

```jsx
function Greeting() {
  return <h1>¡Hola!</h1>;
}
const element = <Greeting />;
```

### 4. No es un template engine
A diferencia de Handlebars o Pug, JSX es JavaScript puro, lo que permite usar toda la potencia de JS (map, filter, etc.) dentro de la UI.

### 5. Seguridad integrada
JSX escapa automáticamente el contenido para prevenir ataques XSS (Cross-Site Scripting):

```jsx
const userInput = "<script>alert('hack')</script>";
return <div>{userInput}</div>; // Se renderiza como texto, no como script
```

---

## Reglas de JSX

Para usar JSX correctamente, debes seguir estas reglas:

### 1. Siempre cerrar etiquetas
Todas las etiquetas deben cerrarse, incluso las vacías:

```jsx
<img src="image.jpg" alt="example" /> // Correcto
<img src="image.jpg"> // Incorrecto
```

### 2. Un solo elemento raíz
JSX debe devolver un solo elemento raíz. Si necesitas múltiples elementos, envuélvelos en un `<div>` o un **Fragment** (`<></>`):

```jsx
return (
  <>
    <h1>Título</h1>
    <p>Descripción</p>
  </>
);
```

### 3. Usar `className` en lugar de `class`
En JSX, los atributos HTML que coinciden con palabras reservadas de JavaScript se renombran:

```jsx
<div className="container">Contenido</div>
```

Otros ejemplos: `htmlFor` en lugar de `for`, `onClick` en lugar de `onclick`.

### 4. CamelCase para eventos y props
Los eventos y propiedades personalizadas usan camelCase:

```jsx
<button onClick={handleClick}>Clic</button>
```

### 5. Expresiones en llaves `{}`
Solo se permiten expresiones JavaScript (no bloques como `if` o `for`) dentro de `{}`:

```jsx
<p>{2 + 2}</p> // Correcto
<p>{if (true) return "Hola"}</p> // Incorrecto
```

### 6. Componentes comienzan con mayúscula
Los componentes personalizados deben empezar con mayúscula para diferenciarlos de etiquetas HTML:

```jsx
<MyComponent /> // Componente
<mycomponent /> // Error: se interpreta como HTML
```

---

## Ejemplo práctico de JSX en React

A continuación, un ejemplo completo que puedes usar en tus proyectos con **Vite** o **Next.js**. Este ejemplo crea un componente con un contador, usando varias características de JSX.

### Código completo
Crea o edita `src/App.jsx` (en Vite) o `app/page.jsx` (en Next.js con App Router):

```jsx
'use client'; // Solo para Next.js (App Router)

import { useState } from 'react';
import './App.css'; // En Vite
// o import styles from './page.module.css'; // En Next.js

function App() {
  const [count, setCount] = useState(0);
  const isHigh = count > 5;

  // Lista dinámica con map
  const items = ['React', 'JSX', 'Vite/Next.js'];
  const listItems = items.map((item, index) => (
    <li key={index}>{item}</li>
  ));

  // Manejo de eventos
  const handleIncrement = () => setCount(count + 1);

  return (
    <div className="container">
      <h1>Contador con JSX</h1>
      <p>Conteo: {count}</p>
      <button onClick={handleIncrement}>Incrementar</button>
      {/* Renderizado condicional */}
      {isHigh ? (
        <p>¡El conteo es alto!</p>
      ) : (
        <p>El conteo es bajo</p>
      )}
      {/* Lista dinámica */}
      <h2>Tecnologías usadas:</h2>
      <ul>{listItems}</ul>
      {/* Componente reutilizable */}
      <Greeting name="Usuario" />
    </div>
  );
}

// Componente reutilizable
function Greeting({ name }) {
  return <p>¡Hola, {name}!</p>;
}

export default App;
```

### Explicación del ejemplo
- **Estructura JSX**: Usa etiquetas como `<div>`, `<h1>`, `<button>`, y el componente personalizado `<Greeting />`
- **Expresiones**: `{count}` muestra el estado, `{listItems}` renderiza una lista dinámica
- **Eventos**: `onClick={handleIncrement}` maneja clics
- **Renderizado condicional**: `{isHigh ? ... : ...}` muestra un mensaje según el estado
- **Lista dinámica**: `items.map` genera `<li>` con una `key` para optimizar el renderizado
- **Props**: El componente `Greeting` recibe `name` como prop
- **Estilos**: Usa `className` para aplicar estilos desde `App.css` (Vite) o `page.module.css` (Next.js)

### Cómo probarlo

+++steps
### Paso 1: Crear proyecto
Crea un proyecto con `npm create vite@latest` (Vite) o `npx create-next-app@latest` (Next.js).

### Paso 2: Reemplazar código
Reemplaza `src/App.jsx` (Vite) o `app/page.jsx` (Next.js) con el código anterior.

### Paso 3: Ejecutar proyecto
Ejecuta `npm run dev` y abre:
- `http://localhost:5173` (Vite)
- `http://localhost:3000` (Next.js)
+++

---

## JSX en Vite vs. Next.js

### Vite
- JSX se usa principalmente para renderizado del lado del cliente (CSR)
- La configuración de Babel está incluida en Vite, por lo que no necesitas configurarlo manualmente
- Ideal para aplicaciones ligeras donde el SEO no es crítico

### Next.js
- JSX se usa en componentes que pueden renderizarse en el servidor (SSR) o estáticamente (SSG)
- En el **App Router**, los componentes con hooks como `useState` requieren `'use client'` para indicar que son client-side
- Next.js optimiza el renderizado de JSX para SEO y rendimiento

---

## Ventajas y limitaciones de JSX

+++admonition
---
type: "success"
title: "Ventajas de JSX"
---
- **Declarativo**: Describe cómo debe verse la UI sin preocuparte por los detalles de manipulación del DOM
- **Integración con JavaScript**: Combina lógica y UI en un solo lugar, permitiendo usar funciones, bucles, etc.
- **Reutilización**: Los componentes JSX son modulares y reutilizables
- **Ecosistema**: Compatible con herramientas como Vite, Next.js, y extensiones de VS Code
+++

+++admonition
---
type: "warning"
title: "Limitaciones de JSX"
---
- **Curva de aprendizaje**: La mezcla de HTML y JavaScript puede ser confusa al principio
- **Dependencia de herramientas**: Necesitas un transpilador como Babel, aunque Vite/Next.js lo manejan por ti
- **Restricciones sintácticas**: Reglas como el elemento raíz único o `className` requieren atención
+++

---

## Consejos para trabajar con JSX

### 1. Usa extensiones de VS Code
Como mencionamos anteriormente, extensiones como **ES7+ React Snippets** (`rafce`) y **Auto Rename Tag** facilitan la escritura y edición de JSX.

### 2. Fragments para evitar `<div>` innecesarios
```jsx
return (
  <>
    <h1>Título</h1>
    <p>Texto</p>
  </>
);
```

### 3. Claves en listas
Siempre asigna una `key` única en elementos de listas para optimizar el renderizado:

```jsx
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

### 4. Evita inline styles complejos
Usa `className` con CSS externo o CSS Modules (como en Next.js) en lugar de:

```jsx
<div style={{ backgroundColor: 'blue', padding: '10px' }} />
```

---

+++cta
---
title: "¡Felicidades! Has completado tu primera lección de React"
buttons:
  - text: "Siguiente Lección: JavaScript ES6+"
    url: "/v1.0/es/01-contenido/02-contenido"
    variant: "primary"
  - text: "Ver Proyecto Final"
    url: "/v1.0/es/01-contenido/10-contenido"
    variant: "secondary"
---
Has aprendido los conceptos fundamentales de React, JSX y cómo configurar tu entorno de desarrollo. En la siguiente lección profundizaremos en JavaScript moderno para prepararte mejor para React.
+++

