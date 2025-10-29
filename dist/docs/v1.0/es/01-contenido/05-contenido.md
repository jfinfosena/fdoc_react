---
title: "Clase 5 - Enrutamiento en Next.js 13+ (App Router)"
position: 5
date: 2025-10-28
---

Este tutorial explica de manera detallada cómo funciona el enrutamiento en Next.js 13 o superior utilizando el **App Router** (directorio `app`). 

---

## Introducción al App Router

El **App Router**, introducido en Next.js 13, es un sistema de enrutamiento basado en archivos que aprovecha los **React Server Components** para ofrecer un enrutamiento más flexible y potente. A diferencia del **Pages Router** (basado en el directorio `pages`), el App Router utiliza el directorio `app` y permite crear diseños compartidos, manejar rutas dinámicas, rutas paralelas, interceptación de rutas, y más.

El enrutamiento en el App Router se basa en la estructura de carpetas y archivos dentro del directorio `app`. Cada carpeta representa un **segmento de ruta**, y los archivos específicos (como `page.js`) definen la interfaz de usuario para esa ruta.

---

## Conceptos Fundamentales

Antes de profundizar, es importante entender algunos términos clave utilizados en el App Router:

- **Árbol**: Representa la estructura jerárquica de las rutas, similar a un árbol de componentes o carpetas.
- **Segmento de ruta**: Cada carpeta dentro de `app` representa un segmento de la URL (por ejemplo, `/dashboard/settings` tiene los segmentos `dashboard` y `settings`).
- **Raíz**: La carpeta `app` es la raíz del árbol de enrutamiento.
- **Hoja**: El último segmento de una ruta, que suele contener un archivo `page.js` para renderizar la interfaz de usuario.
- **Ruta URL**: La parte de la URL después del dominio, compuesta por segmentos (por ejemplo, `/dashboard/settings`).

El App Router utiliza convenciones específicas de nombres de archivos para definir comportamientos en las rutas, como `page.js` para páginas, `layout.js` para diseños, y otros archivos especiales que veremos más adelante.[](https://runebook.dev/es/docs/nextjs/app/building-your-application/routing)

---

## Estructura Básica del Enrutamiento

El enrutamiento en Next.js 13+ se define automáticamente según la estructura de carpetas dentro del directorio `app`. A continuación, se explica cómo crear rutas básicas.

### Crear una Ruta Básica

Para crear una página, añade un archivo `page.js` dentro del directorio `app`. Este archivo define el contenido principal de la ruta.

Por ejemplo, para crear la página de inicio (`/`):

```bash
app/
└── page.js
```

**Contenido de `app/page.js`**:

```javascript
export default function Home() {
  return (
    <div>
      <h1>Bienvenido a mi aplicación Next.js</h1>
      <p>Esta es la página de inicio.</p>
    </div>
  );
}
```

Este archivo `page.js` se asigna automáticamente a la ruta raíz (`/`). Cuando un usuario visita `http://localhost:3000/`, verá el contenido definido en este archivo.

### Rutas Anidadas

Para crear rutas anidadas, simplemente crea carpetas dentro de `app` y añade un archivo `page.js` en cada una. Por ejemplo, para crear la ruta `/dashboard/settings`:

```bash
app/
└── dashboard/
    └── settings/
        └── page.js
```

**Contenido de `app/dashboard/settings/page.js`**:

```javascript
export default function Settings() {
  return (
    <div>
      <h1>Configuraciones</h1>
      <p>Esta es la página de configuraciones.</p>
    </div>
  );
}
```

Al visitar `http://localhost:3000/dashboard/settings`, se renderizará el contenido de `page.js` en la carpeta `settings`. La estructura de carpetas define automáticamente la jerarquía de la URL.

---

## Rutas Dinámicas

### Enrutamiento dinámico con TypeScript (App Router)

A continuación se muestran patrones tipados para rutas dinámicas en Next.js 13+ usando el App Router.

#### `[id]` con tipos

Estructura:

```bash
app/
└── products/
    └── [id]/
        └── page.tsx
```

Código (`app/products/[id]/page.tsx`):

```tsx
type ProductPageProps = {
  params: { id: string };
  searchParams?: { [key: string]: string | string[] | undefined };
};

export default function ProductPage({ params }: ProductPageProps) {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">Producto #{params.id}</h1>
      <p className="text-gray-700">ID recibido: {params.id}</p>
    </div>
  );
}
```

#### Dinámicas anidadas tipadas

```bash
app/
└── users/
    └── [userId]/
        └── posts/
            └── [postId]/
                └── page.tsx
```

```tsx
type PostPageProps = {
  params: { userId: string; postId: string };
};

export default function PostPage({ params }: PostPageProps) {
  const { userId, postId } = params;
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold">Post {postId} del usuario {userId}</h1>
    </div>
  );
}
```

#### Catch-All (`[...slug]`) tipado

```bash
app/
└── docs/
    └── [...slug]/
        └── page.tsx
```

```tsx
type DocsPageProps = {
  params: { slug: string[] };
};

export default function DocsPage({ params }: DocsPageProps) {
  const path = params.slug.join('/');
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">Documentación</h1>
      <p>Ruta: /docs/{path}</p>
    </div>
  );
}
```

#### Pre-render con `generateStaticParams` tipado

```tsx
// app/products/[id]/page.tsx
export async function generateStaticParams(): Promise<Array<{ id: string }>> {
  const ids = ['1', '2', '3'];
  return ids.map((id) => ({ id }));
}
```

Notas:
- Los archivos en `app/` son Server Components por defecto; usa `'use client'` solo si empleas hooks del cliente.
- Tipar `params` y `searchParams` evita errores y mejora DX.

Las rutas dinámicas permiten manejar URLs con parámetros variables, como `/products/[id]`. En el App Router, las rutas dinámicas se crean usando corchetes (`[]`) en los nombres de las carpetas.

### Ejemplo de Ruta Dinámica

Para crear una ruta dinámica como `/products/[id]`:

```bash
app/
└── products/
    └── [id]/
        └── page.js
```

**Contenido de `app/products/[id]/page.js`**:

```javascript
export default function Product({ params }) {
  return (
    <div>
      <h1>Producto {params.id}</h1>
      <p>Esta es la página del producto con ID: {params.id}</p>
    </div>
  );
}
```

- La carpeta `[id]` indica que el segmento de la URL es dinámico.
- El parámetro `params` contiene los valores dinámicos (por ejemplo, `{ id: "123" }` para la URL `/products/123`).

### Rutas Dinámicas Anidadas

Puedes anidar rutas dinámicas. Por ejemplo, para `/users/[userId]/posts/[postId]`:

```bash
app/
└── users/
    └── [userId]/
        └── posts/
            └── [postId]/
                └── page.js
```

**Contenido de `app/users/[userId]/posts/[postId]/page.js`**:

```javascript
export default function Post({ params }) {
  return (
    <div>
      <h1>Post {params.postId} del usuario {params.userId}</h1>
    </div>
  );
}
```

- Para la URL `/users/1/posts/42`, `params` será `{ userId: "1", postId: "42" }`.

---

## Rutas de Captura Total (Catch-All Routes)

Las rutas de captura total permiten manejar múltiples segmentos de URL. Se definen usando `[...slug]`.

Por ejemplo, para capturar todas las rutas bajo `/blog`:

```bash
app/
└── blog/
    └── [...slug]/
        └── page.js
```

**Contenido de `app/blog/[...slug]/page.js`**:

```javascript
export default function BlogPost({ params }) {
  return (
    <div>
      <h1>Blog</h1>
      <p>Segmentos: {params.slug.join('/')}</p>
    </div>
  );
}
```

- Para la URL `/blog/2023/octubre/mi-post`, `params.slug` será `["2023", "octubre", "mi-post"]`.
- Útil para manejar estructuras de URL profundas o indeterminadas.

---


## Navegación con el Componente Link

El componente `Link` de `next/link` permite realizar navegación del lado del cliente, mejorando la experiencia de usuario al evitar recargas completas.

**Ejemplo de uso**:

```javascript
import Link from 'next/link';

export default function Home() {
  return (
    <div>
      <h1>Inicio</h1>
      <Link href="/dashboard">Ir al Dashboard</Link>
    </div>
  );
}
```

- `Link` precarga automáticamente las rutas visibles en la ventana gráfica en modo producción, mejorando el rendimiento.[](https://www.freecodecamp.org/espanol/news/manual-de-next-js/)

---


---






