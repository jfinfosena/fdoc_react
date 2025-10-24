---
title: "Clase 4 - Agregar CSS a Componentes en Next.js"
position: 4
date: 2025-10-24
---


```admonition
---
type: info
title: ""
---
Este tutorial explica de forma sencilla las maneras de agregar estilos CSS a componentes en **Next.js**, excluyendo Styled-Components. Incluye ejemplos prácticos y está optimizado para MkDocs Material, con una estructura clara y ejemplos de componentes funcionales.
```

## Introducción

En **Next.js**, hay varias formas de aplicar estilos CSS a tus componentes React. Este tutorial cubre los métodos más comunes (excepto CSS-in-JS con Styled-Components), con ejemplos simples para que puedas entender y aplicar cada uno. Los ejemplos usan componentes funcionales y se centran en una tarjeta de usuario estilizada.


```admonition
---
type: note
title: ""
---
Asegúrate de tener una aplicación Next.js configurada (`npx create-next-app@latest`). Los ejemplos asumen que estás usando la carpeta `app` (App Router) de Next.js.
```


## 1. CSS Global (Archivo CSS en `/app/globals.css`)

**Explicación**: Los estilos globales se definen en un archivo CSS general (como `globals.css`) y afectan a toda la aplicación. Útil para estilos base o temas generales.

**Pasos**:

1. Usa el archivo `/app/globals.css` (creado por defecto en Next.js).
2. Importa `globals.css` en el archivo `layout.js` (ya está importado por defecto).
3. Aplica clases a tus componentes.

**Ejemplo**:

### Archivo: `app/globals.css`

```css
.tarjeta {
  border: 1px solid #ccc;
  padding: 16px;
  border-radius: 8px;
  background-color: #f9f9f9;
  text-align: center;
}

.tarjeta h2 {
  color: #007bff;
}

.tarjeta button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.tarjeta button:hover {
  background-color: #0056b3;
}
```

### Archivo: `app/page.jsx`

```jsx
export default function Home() {
  return (
    <div className="tarjeta">
      <h2>Usuario</h2>
      <p>¡Bienvenido a Next.js!</p>
      <button>Saludar</button>
    </div>
  );
}
```

```admonition
---
type: tip
title: ""
---
Los estilos globales son ideales para temas generales, pero evita usarlos para estilos específicos para no sobrecargar el archivo.
```

## 2. Módulos CSS (Archivos `.module.css`)

**Explicación**: Los módulos CSS permiten estilos locales y evitan conflictos al generar nombres de clases únicos. Cada componente tiene su propio archivo `.module.css`.

**Pasos**:
1. Crea un archivo CSS con la extensión `.module.css` (ejemplo: `Tarjeta.module.css`).
2. Importa el módulo en tu componente y usa las clases como propiedades de un objeto.

**Ejemplo**:

### Archivo: `app/components/Tarjeta.module.css`

```css
.tarjeta {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 10px;
  background-color: #f0f4f8;
  text-align: center;
}

.titulo {
  color: #28a745;
  font-size: 1.5rem;
}

.boton {
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.boton:hover {
  background-color: #218838;
}
```

### Archivo: `app/components/Tarjeta.jsx`

```jsx
import styles from './Tarjeta.module.css';

export default function Tarjeta({ nombre }) {
  return (
    <div className={styles.tarjeta}>
      <h2 className={styles.titulo}>{nombre}</h2>
      <p>Componente con módulo CSS</p>
      <button className={styles.boton}>Saludar</button>
    </div>
  );
}
```

### Archivo: `app/page.jsx`

```jsx
import Tarjeta from './components/Tarjeta';

export default function Home() {
  return <Tarjeta nombre="Ana" />;
}
```

```admonition
---
type: note
title: ""
---
Los módulos CSS son ideales para componentes reutilizables, ya que los estilos son locales y no afectan a otros componentes.
```

## 3. Estilos en Línea (Inline CSS)

**Explicación**: Los estilos en línea se aplican directamente en los elementos JSX usando el atributo `style` con un objeto JavaScript. Útil para estilos dinámicos, pero menos escalable.

**Ejemplo**:

### Archivo: `app/components/Tarjeta.jsx`

```jsx
export default function Tarjeta({ nombre }) {
  const tarjetaEstilo = {
    border: '1px solid #aaa',
    padding: '16px',
    borderRadius: '8px',
    backgroundColor: '#f9f9f9',
    textAlign: 'center',
  };

  const botonEstilo = {
    padding: '8px 16px',
    backgroundColor: '#dc3545',
    color: 'white',
    border: 'none',
    borderRadius: '4px',
    cursor: 'pointer',
  };

  return (
    <div style={tarjetaEstilo}>
      <h2 style={{ color: '#dc3545' }}>{nombre}</h2>
      <p>Estilos en línea</p>
      <button style={botonEstilo}>Saludar</button>
    </div>
  );
}
```


```admonition
---
type: warning
title: ""
---
Los estilos en línea no admiten pseudoclases como `:hover`. Úsalos solo para casos simples o dinámicos.
```

## 4. Tailwind CSS (Integrado en Next.js)

**Explicación**: Tailwind CSS es una biblioteca de utilidad que permite estilizar directamente en el JSX con clases predefinidas. Next.js tiene soporte nativo para Tailwind.

**Pasos**:
1. Instala Tailwind: `npm install -D tailwindcss postcss autoprefixer`, luego ejecuta `npx tailwindcss init -p`.
2. Configura `tailwind.config.js` y añade las directivas en `app/globals.css`.
3. Usa clases de Tailwind en tus componentes.

**Ejemplo**:

### Archivo: `tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx}',
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### Archivo: `app/globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Archivo: `app/components/Tarjeta.jsx`

```jsx
export default function Tarjeta({ nombre }) {
  return (
    <div className="border border-gray-300 p-4 rounded-lg bg-gray-50 text-center">
      <h2 className="text-2xl text-blue-600">{nombre}</h2>
      <p>Estilizado con Tailwind CSS</p>
      <button className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">
        Saludar
      </button>
    </div>
  );
}
```

!!! tip
    Tailwind es ideal para desarrollo rápido y consistente. Usa la documentación oficial para explorar todas las clases disponibles.

## Ejemplo Completo con Todos los Métodos

### Archivo: `app/components/Tarjeta.jsx`

```jsx
import styles from './Tarjeta.module.css';

export default function Tarjeta({ nombre, metodo }) {
  if (metodo === 'global') {
    return (
      <div className="tarjeta">
        <h2>{nombre}</h2>
        <p>Estilos Globales</p>
        <button>Saludar</button>
      </div>
    );
  }

  if (metodo === 'modulo') {
    return (
      <div className={styles.tarjeta}>
        <h2 className={styles.titulo}>{nombre}</h2>
        <p>Estilos con Módulo CSS</p>
        <button className={styles.boton}>Saludar</button>
      </div>
    );
  }

  if (metodo === 'inline') {
    return (
      <div
        style={{
          border: '1px solid #aaa',
          padding: '16px',
          borderRadius: '8px',
          backgroundColor: '#f9f9f9',
          textAlign: 'center',
        }}
      >
        <h2 style={{ color: '#dc3545' }}>{nombre}</h2>
        <p>Estilos en Línea</p>
        <button
          style={{
            padding: '8px 16px',
            backgroundColor: '#dc3545',
            color: 'white',
            border: 'none',
            borderRadius: '4px',
            cursor: 'pointer',
          }}
        >
          Saludar
        </button>
      </div>
    );
  }

  return (
    <div className="border border-gray-300 p-4 rounded-lg bg-gray-50 text-center">
      <h2 className="text-2xl text-blue-600">{nombre}</h2>
      <p>Estilizado con Tailwind CSS</p>
      <button className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">
        Saludar
      </button>
    </div>
  );
}
```

### Archivo: `app/page.jsx`

```jsx
import Tarjeta from './components/Tarjeta';

export default function Home() {
  return (
    <div className="flex flex-wrap gap-4 p-4">
      <Tarjeta nombre="Ana" metodo="global" />
      <Tarjeta nombre="Carlos" metodo="modulo" />
      <Tarjeta nombre="María" metodo="inline" />
      <Tarjeta nombre="Luis" metodo="tailwind" />
    </div>
  );
}
```

### Archivo: `app/components/Tarjeta.module.css`

```css
.tarjeta {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 10px;
  background-color: #f0f4f8;
  text-align: center;
}

.titulo {
  color: #28a745;
  font-size: 1.5rem;
}

.boton {
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.boton:hover {
  background-color: #218838;
}
```

### Archivo: `app/globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.tarjeta {
  border: 1px solid #ccc;
  padding: 16px;
  border-radius: 8px;
  background-color: #f9f9f9;
  text-align: center;
}

.tarjeta h2 {
  color: #007bff;
}

.tarjeta button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.tarjeta button:hover {
  background-color: #0056b3;
}
```

## Conclusión

- **CSS Global**: Útil para estilos generales, pero puede causar conflictos si no se organiza bien.
- **Módulos CSS**: Ideal para estilos locales y reutilizables, con nombres de clases únicos.
- **Estilos en Línea**: Bueno para casos dinámicos, pero limitado para pseudoclases como `:hover`.
- **Tailwind CSS**: Rápido y consistente, perfecto para proyectos que priorizan velocidad de desarrollo.


```admonition
---
type: tip
title: ""
---
Para proyectos grandes, combina **Módulos CSS** o **Tailwind** para mantener los estilos organizados y escalables. Evita estilos en línea para componentes complejos.
```



