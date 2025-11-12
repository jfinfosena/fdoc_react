---
title: "Clase 6 - Renderizado Dinámico de Arreglos Locales en React y Next.js"
position: 6
date: 2025-11-11
---

El renderizado dinámico de arreglos locales es una práctica común en **React** y **Next.js** para mostrar listas de datos en la interfaz de usuario. Esto se logra principalmente utilizando el método `.map()` de JavaScript, fragmentos (`<></>`) para agrupar elementos sin nodos adicionales en el DOM, y la prop `key` para optimizar el rendimiento y evitar problemas de reconciliación en el DOM virtual. Este documento explica en detalle cómo implementar el renderizado dinámico de arreglos locales en componentes de React dentro de un proyecto Next.js, incluyendo ejemplos prácticos, buenas prácticas y el uso de **Tailwind CSS** para estilos. Los ejemplos se basan en una aplicación de tienda en línea que muestra una lista de productos.

---

## Conceptos clave

### 1. Renderizado dinámico con `.map()`
El método `.map()` de JavaScript itera sobre un arreglo y devuelve un nuevo arreglo con los elementos transformados. En React, se usa para generar elementos JSX dinámicamente a partir de un arreglo de datos.

- **Uso**: Transforma cada elemento del arreglo en un componente o elemento JSX.
- **Ejemplo**: `array.map(item => <div>{item}</div>)`

### 2. Fragmentos (`<></>` o `<React.Fragment>`)
Los fragmentos permiten agrupar múltiples elementos JSX sin introducir un nodo adicional en el DOM (como un `<div>`).

- **Uso**: Envolver múltiples elementos devueltos por `.map()` para cumplir con la regla de React de devolver un solo elemento.
- **Ventaja**: Reduce el clutter en el DOM, mejorando el rendimiento y la semántica.
- **Ejemplo**: `<>{array.map(item => <div>{item}</div>)}</>`

### 3. Prop `key`
La prop `key` es un identificador único que React usa para optimizar la reconciliación del DOM virtual, evitando renders innecesarios y manteniendo el estado de los componentes.

- **Uso**: Se asigna a cada elemento generado por `.map()` usando un valor único (como un ID).
- **Regla**: No uses índices del arreglo (`index`) como `key` si los datos pueden cambiar (ordenar, agregar, eliminar), ya que puede causar problemas de rendimiento o comportamiento inesperado.
- **Ejemplo**: `<div key={item.id}>{item.nombre}</div>`

---

## Sesión JavaScript (JSX)

Ejemplos y explicaciones usando JSX de React.

## Ejemplo práctico: Lista de productos

Crearemos un componente que renderiza dinámicamente una lista de productos almacenada en un arreglo local. Usaremos `.map()`, fragmentos, y la prop `key`, con estilos de Tailwind CSS.

### Código del componente

#### Archivo: `components/ProductList.jsx`

```jsx
import React from 'react';

const products = [
  { id: 1, name: 'Laptop', price: 999.99, category: 'Electrónica' },
  { id: 2, name: 'Smartphone', price: 699.99, category: 'Electrónica' },
  { id: 3, name: 'Zapatillas', price: 89.99, category: 'Moda' },
  { id: 4, name: 'Libro', price: 19.99, category: 'Libros' },
];

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Lista de Productos</h1>
      <>
        {products.map((product) => (
          <div
            key={product.id}
            className="bg-white shadow-md rounded-lg p-4 mb-4 flex justify-between items-center"
          >
            <div>
              <h2 className="text-xl font-semibold">{product.name}</h2>
              <p className="text-gray-600">Categoría: {product.category}</p>
            </div>
            <p className="text-lg font-bold text-green-600">${product.price.toFixed(2)}</p>
          </div>
        ))}
      </>
    </div>
  );
}

export default ProductList;
```

#### Archivo: `app/page.jsx`

```jsx
import ProductList from '../components/ProductList';

export default function Home() {
  return (
    <main>
      <ProductList />
    </main>
  );
}
```

### Explicación del ejemplo

- **Arreglo local** (`products`):
  - Definimos un arreglo estático de objetos que representan productos, cada uno con `id`, `name`, `price` y `category`.
  - En un escenario real, este arreglo podría provenir de una API o base de datos.

- **Uso de `.map()`**:
  - Iteramos sobre `products` con `.map()` para generar un `<div>` por cada producto.
  - Cada `<div>` muestra el nombre, categoría y precio del producto.

- **Fragmentos (`<></>`)**:
  - Envolvemos los elementos generados por `.map()` en un fragmento (`<></>`) para cumplir con la regla de React de devolver un solo elemento raíz.
  - Esto evita agregar un `<div>` innecesario en el DOM.

- **Prop `key`**:
  - Usamos `key={product.id}` para asignar un identificador único a cada `<div>`.
  - El `id` es único y estable, lo que ayuda a React a rastrear los elementos y optimizar las actualizaciones.

- **Estilos con Tailwind CSS**:
  - Usamos clases de Tailwind como `max-w-4xl`, `mx-auto`, `p-4`, `shadow-md`, etc., para centrar el contenido, agregar sombras, bordes redondeados y estilos responsivos.
  - Los productos se muestran en tarjetas con un diseño limpio y moderno.

- **Estructura del DOM**:
  - El componente genera una lista de tarjetas, cada una representando un producto, sin nodos adicionales gracias al fragmento.

---

## Variaciones y casos avanzados

### 1. Renderizado condicional dentro de `.map()`
A veces, necesitas renderizar elementos solo si cumplen ciertas condiciones. Por ejemplo, mostrar solo productos de una categoría específica.

#### Código

```jsx
import React from 'react';

const products = [
  { id: 1, name: 'Laptop', price: 999.99, category: 'Electrónica' },
  { id: 2, name: 'Smartphone', price: 699.99, category: 'Electrónica' },
  { id: 3, name: 'Zapatillas', price: 89.99, category: 'Moda' },
  { id: 4, name: 'Libro', price: 19.99, category: 'Libros' },
];

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Productos Electrónicos</h1>
      <>
        {products.map((product) =>
          product.category === 'Electrónica' ? (
            <div
              key={product.id}
              className="bg-white shadow-md rounded-lg p-4 mb-4 flex justify-between items-center"
            >
              <div>
                <h2 className="text-xl font-semibold">{product.name}</h2>
                <p className="text-gray-600">Categoría: {product.category}</p>
              </div>
              <p className="text-lg font-bold text-green-600">${product.price.toFixed(2)}</p>
            </div>
          ) : null
        )}
      </>
    </div>
  );
}

export default ProductList;
```

#### Explicación
- **Condicional**: Dentro de `.map()`, usamos un operador ternario (`?:`) para renderizar solo los productos de la categoría "Electrónica". Si la condición no se cumple, devolvemos `null`, lo que no genera ningún elemento en el DOM.
- **Fragmentos**: Seguimos usando `<></>` para envolver los elementos generados.
- **Key**: Cada elemento renderizado tiene un `key` basado en `product.id`.

---

### 2. Componente reutilizable para cada elemento
Para mejorar la modularidad, podemos extraer la lógica de renderizado de cada producto a un componente separado.

#### Código

```jsx
// components/ProductItem.jsx
import React from 'react';

function ProductItem({ product }) {
  return (
    <div className="bg-white shadow-md rounded-lg p-4 mb-4 flex justify-between items-center">
      <div>
        <h2 className="text-xl font-semibold">{product.name}</h2>
        <p className="text-gray-600">Categoría: {product.category}</p>
      </div>
      <p className="text-lg font-bold text-green-600">${product.price.toFixed(2)}</p>
    </div>
  );
}

export default ProductItem;

// components/ProductList.jsx
import React from 'react';
import ProductItem from './ProductItem';

const products = [
  { id: 1, name: 'Laptop', price: 999.99, category: 'Electrónica' },
  { id: 2, name: 'Smartphone', price: 699.99, category: 'Electrónica' },
  { id: 3, name: 'Zapatillas', price: 89.99, category: 'Moda' },
  { id: 4, name: 'Libro', price: 19.99, category: 'Libros' },
];

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Lista de Productos</h1>
      <>
        {products.map((product) => (
          <ProductItem key={product.id} product={product} />
        ))}
      </>
    </div>
  );
}

export default ProductList;
```

#### Explicación
- **Componente `ProductItem`**: Encapsula la lógica de renderizado de un solo producto, recibiendo `product` como prop.
- **Reutilización**: `ProductList` usa `ProductItem` dentro de `.map()`, pasando cada `product` como prop.
- **Key**: La prop `key` se asigna en el nivel de `ProductItem` para mantener la optimización.
- **Ventajas**: Mejora la mantenibilidad y permite reutilizar `ProductItem` en otros contextos.

---

### 3. Manejo de arreglos vacíos
Es importante manejar casos donde el arreglo esté vacío para evitar mostrar una lista vacía sin retroalimentación al usuario.

#### Código

```jsx
import React from 'react';
import ProductItem from './ProductItem';

const products = []; // Arreglo vacío para este ejemplo

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Lista de Productos</h1>
      {products.length === 0 ? (
        <p className="text-center text-gray-500">No hay productos disponibles.</p>
      ) : (
        <>
          {products.map((product) => (
            <ProductItem key={product.id} product={product} />
          ))}
        </>
      )}
    </div>
  );
}

export default ProductList;
```

#### Explicación
- **Condicional**: Usamos un operador ternario para verificar si `products.length === 0`. Si está vacío, mostramos un mensaje; si no, renderizamos la lista.
- **Fragmentos**: Cuando hay productos, usamos `<></>` para envolver los elementos generados por `.map()`.
- **Buena práctica**: Siempre proporcionar retroalimentación al usuario cuando no hay datos.

### 4. Condicionales en React y Operador Ternario

#### Código (JSX)

```jsx
import React, { useState } from 'react';

function ConditionalExamples() {
  const [isLoading, setIsLoading] = useState(false);
  const [hasError, setHasError] = useState(false);
  const [items, setItems] = useState(['Laptop', 'Libro']);

  if (hasError) {
    return <p className="text-red-600">Ocurrió un error</p>;
  }

  return (
    <div className="space-y-4">
      {isLoading && <p className="text-gray-500">Cargando...</p>}
      <button
        className={`px-4 py-2 rounded ${isLoading ? 'bg-gray-400' : 'bg-blue-600 text-white'}`}
        onClick={() => setIsLoading(!isLoading)}
      >
        {isLoading ? 'Detener' : 'Cargar'}
      </button>
      {items.length === 0 ? (
        <p className="text-gray-500">Sin elementos</p>
      ) : (
        <ul className="list-disc pl-5">
          {items.map((it) => (
            <li key={it}>{it}</li>
          ))}
        </ul>
      )}
    </div>
  );
}

export default ConditionalExamples;
```

#### Explicación

- `&&` para mostrar elementos cuando una condición es verdadera.
- Ternario para elegir entre dos ramas de UI.
- Retorno temprano para errores o estados vacíos.
- Estilos dinámicos con `className` usando ternario.

### 5. Fallback con `||` y `??`

#### Código (JSX)

```jsx
import React from 'react';

const product = { name: '', category: undefined, price: 0 };

function FallbackExamples() {
  const title = product.name || 'Sin título';
  const category = product.category ?? 'General';
  const priceLabel = (product.price ?? 0).toFixed(2);

  return (
    <div className="space-y-2">
      <p>Nombre: {title}</p>
      <p>Categoría: {category}</p>
      <p>Precio: ${priceLabel}</p>
    </div>
  );
}

export default FallbackExamples;
```

#### Explicación

- `||` considera falsy: `''`, `0`, `null`, `undefined`.
- `??` solo evalúa `null`/`undefined` sin afectar `''` o `0`.
- Combinación precisa para etiquetas y valores numéricos.

### 6. Patrón de estados con `switch`

#### Código (JSX)

```jsx
import React from 'react';

function StatefulView({ status, data = [] }) {
  switch (status) {
    case 'idle':
      return <p>Listo para buscar</p>;
    case 'loading':
      return <p className="text-gray-500">Cargando...</p>;
    case 'error':
      return <p className="text-red-600">Error al cargar</p>;
    case 'success':
      return data.length === 0 ? (
        <p className="text-gray-500">Sin datos</p>
      ) : (
        <ul className="list-disc pl-5">
          {data.map((it) => (
            <li key={it}>{it}</li>
          ))}
        </ul>
      );
    default:
      return null;
  }
}

export default StatefulView;
```

#### Explicación

- `switch` organiza estados de vista sin múltiples ternarios.
- Combinación con ternario para listas vacías en `success`.
- `default` cubre estados no contemplados.

---


## Sesión TypeScript (TSX)

Esta sección incluye explicaciones y ejemplos equivalentes usando TypeScript (TSX) para obtener tipado estático, mejor autocompletado y detección temprana de errores.

### Tipos y componentes tipados

Definimos un tipo `Product` y tipamos las props de los componentes.

#### Archivo: `components/ProductItem.tsx`

```tsx
import React from 'react';

export type Product = {
  id: number;
  name: string;
  price: number;
  category: string;
};

type Props = { product: Product };

function ProductItem({ product }: Props) {
  return (
    <div className="bg-white shadow-md rounded-lg p-4 mb-4 flex justify-between items-center">
      <div>
        <h2 className="text-xl font-semibold">{product.name}</h2>
        <p className="text-gray-600">Categoría: {product.category}</p>
      </div>
      <p className="text-lg font-bold text-green-600">${product.price.toFixed(2)}</p>
    </div>
  );
}

export default ProductItem;
```

#### Archivo: `components/ProductList.tsx`

```tsx
import React from 'react';
import ProductItem, { Product } from './ProductItem';

const products: Product[] = [
  { id: 1, name: 'Laptop', price: 999.99, category: 'Electrónica' },
  { id: 2, name: 'Smartphone', price: 699.99, category: 'Electrónica' },
  { id: 3, name: 'Zapatillas', price: 89.99, category: 'Moda' },
  { id: 4, name: 'Libro', price: 19.99, category: 'Libros' },
];

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Lista de Productos</h1>
      <>
        {products.map((product) => (
          <ProductItem key={product.id} product={product} />
        ))}
      </>
    </div>
  );
}

export default ProductList;
```

#### Archivo: `app/page.tsx`

```tsx
import ProductList from '../components/ProductList';

export default function Home() {
  return (
    <main>
      <ProductList />
    </main>
  );
}
```

### Explicación (TS)

- Tipos estrictos: `Product` tipa la estructura de cada elemento del arreglo.
- Props tipadas: `Props` obliga a pasar un `product` válido a `ProductItem`.
- Seguridad: El compilador valida el uso de propiedades como `price.toFixed(2)`.

---

## Variaciones y casos avanzados (TS)

### 1. Renderizado condicional dentro de `.map()`

#### Código

```tsx
import React from 'react';
import ProductItem, { Product } from './ProductItem';

const products: Product[] = [
  { id: 1, name: 'Laptop', price: 999.99, category: 'Electrónica' },
  { id: 2, name: 'Smartphone', price: 699.99, category: 'Electrónica' },
  { id: 3, name: 'Zapatillas', price: 89.99, category: 'Moda' },
  { id: 4, name: 'Libro', price: 19.99, category: 'Libros' },
];

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Productos Electrónicos</h1>
      <>
        {products.map((product) =>
          product.category === 'Electrónica' ? (
            <ProductItem key={product.id} product={product} />
          ) : null
        )}
      </>
    </div>
  );
}

export default ProductList;
```

#### Explicación
- Condicional: La verificación de `product.category` está tipada y segura.
- Key: Se mantiene `key={product.id}` para optimización de reconciliación.

---

### 2. Manejo de arreglos vacíos

#### Código

```tsx
import React from 'react';
import ProductItem, { Product } from './ProductItem';

const products: Product[] = [];

function ProductList() {
  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6 text-center">Lista de Productos</h1>
      {products.length === 0 ? (
        <p className="text-center text-gray-500">No hay productos disponibles.</p>
      ) : (
        <>
          {products.map((product) => (
            <ProductItem key={product.id} product={product} />
          ))}
        </>
      )}
    </div>
  );
}

export default ProductList;
```

#### Explicación
- Condicional: Se usa un ternario para vacíos con tipos explícitos.
- Fragmentos: `<></>` agrupa la lista sin nodos extra.

### 3. Condicionales en React y Operador Ternario (TS)

#### Código (TSX)

```tsx
import React, { useState } from 'react';

function ConditionalExamples() {
  const [isLoading, setIsLoading] = useState<boolean>(false);
  const [hasError, setHasError] = useState<boolean>(false);
  const [items, setItems] = useState<string[]>(['Laptop', 'Libro']);

  if (hasError) {
    return <p className="text-red-600">Ocurrió un error</p>;
  }

  return (
    <div className="space-y-4">
      {isLoading && <p className="text-gray-500">Cargando...</p>}
      <button
        className={`px-4 py-2 rounded ${isLoading ? 'bg-gray-400' : 'bg-blue-600 text-white'}`}
        onClick={() => setIsLoading((v) => !v)}
      >
        {isLoading ? 'Detener' : 'Cargar'}
      </button>
      {items.length === 0 ? (
        <p className="text-gray-500">Sin elementos</p>
      ) : (
        <ul className="list-disc pl-5">
          {items.map((it) => (
            <li key={it}>{it}</li>
          ))}
        </ul>
      )}
    </div>
  );
}

export default ConditionalExamples;
```

#### Explicación

- Tipado explícito con `useState<boolean>` y `useState<string[]>`.
- `&&` y ternario para control de renderizado sin errores de tipo.
- Retorno temprano con UI segura y tipada.
- Estilos dinámicos con tipos garantizados.

### 4. Fallback con `||` y `??` (TS)

#### Código (TSX)

```tsx
import React from 'react';

type Product = {
  name?: string;
  category?: string | null;
  price?: number | null;
};

function FallbackExamplesTS() {
  const product: Product = { name: '', category: null, price: 0 };

  const title = product.name || 'Sin título';
  const category = product.category ?? 'General';
  const priceValue = product.price ?? 0;

  return (
    <div className="space-y-2">
      <p>Nombre: {title}</p>
      <p>Categoría: {category}</p>
      <p>Precio: ${priceValue.toFixed(2)}</p>
    </div>
  );
}

export default FallbackExamplesTS;
```

#### Explicación

- `||` para falsy y `??` para `null`/`undefined` con tipos.
- El compilador garantiza que `priceValue` es `number` antes de `toFixed`.

### 5. Patrón de estados con `switch` (TS)

#### Código (TSX)

```tsx
import React from 'react';

type Status = 'idle' | 'loading' | 'error' | 'success';

type Props = { status: Status; data?: string[] };

function StatefulViewTS({ status, data = [] }: Props) {
  switch (status) {
    case 'idle':
      return <p>Listo para buscar</p>;
    case 'loading':
      return <p className="text-gray-500">Cargando...</p>;
    case 'error':
      return <p className="text-red-600">Error al cargar</p>;
    case 'success':
      return data.length === 0 ? (
        <p className="text-gray-500">Sin datos</p>
      ) : (
        <ul className="list-disc pl-5">
          {data.map((it) => (
            <li key={it}>{it}</li>
          ))}
        </ul>
      );
  }
}

export default StatefulViewTS;
```

#### Explicación

- Unión de `Status` evita estados inválidos.
- Al cubrir todos los casos no se requiere `default`.
- Tipos de `data` aseguran claves y contenido correcto.




