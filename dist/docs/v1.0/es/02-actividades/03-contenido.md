---
title: "Actividad 3 - Enrutamiento en Next.js 13+ (App Router)"
position: 3
date: 2025-10-28
---

# Actividad 3: Enrutamiento en Next.js 13+ (App Router)

## Objetivo
Implementar y dominar el sistema de enrutamiento del App Router de Next.js 13+, creando rutas básicas, dinámicas y de captura total, además de manejar la navegación programática.

---

## Antes de empezar: Fork y clonación del repositorio

- **Paso 1:** Realiza un fork del repositorio original en GitHub.
  - Abre `https://github.com/jfinfosena/fdoc_react_act3.git` y pulsa `Fork`.
  - Tu fork quedará en `https://github.com/<tu-usuario>/fdoc_react_act3.git`.

- **Paso 2:** Clona tu fork en tu equipo.
  - En la consola:
    - `git clone https://github.com/<tu-usuario>/fdoc_react_act3.git`
    - `cd fdoc_react_act3`

- **Paso 3:** Instala dependencias y arranca el proyecto.
  - `npm install`
  - `npm run dev`

---

## Ejercicio 1: Configuración Inicial y Rutas Básicas

### Instrucciones:
1. **Crear un nuevo proyecto Next.js 13+**

   ```bash
   npx create-next-app@latest mi-tienda-online --typescript --tailwind --eslint --app
   cd mi-tienda-online
   ```

2. **Crear la estructura básica de rutas:**

   ```bash
   app/
   ├── page.tsx (página de inicio)
   ├── about/
   │   └── page.tsx
   ├── products/
   │   └── page.tsx
   └── contact/
       └── page.tsx
   ```

3. **Implementar cada página con contenido básico:**

**`app/page.tsx`:**
```tsx
export default function Home() {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-4xl font-bold mb-4">Mi Tienda Online</h1>
      <p className="text-lg">Bienvenido a nuestra tienda virtual</p>
    </div>
  );
}
```

**`app/about/page.tsx`:**
```tsx
export default function About() {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">Acerca de Nosotros</h1>
      <p>Somos una empresa dedicada a...</p>
    </div>
  );
}
```

4. **Verificar:** Navega a cada ruta manualmente en el navegador.

---

## Ejercicio 2: Rutas Dinámicas

### Instrucciones:
1. **Crear rutas dinámicas para productos:**

   ```bash
   app/
   └── products/
       ├── page.tsx (lista de productos)
       └── [id]/
           └── page.tsx (detalle del producto)
   ```

2. **Implementar la página de detalle del producto:**

**`app/products/[id]/page.tsx`:**
```tsx
interface ProductPageProps {
  params: {
    id: string;
  };
}

export default function ProductPage({ params }: ProductPageProps) {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">Producto #{params.id}</h1>
      <div className="bg-gray-100 p-6 rounded-lg">
        <h2 className="text-xl font-semibold mb-2">Detalles del Producto</h2>
        <p><strong>ID:</strong> {params.id}</p>
        <p><strong>Precio:</strong> $99.99</p>
        <p><strong>Descripción:</strong> Producto de alta calidad</p>
      </div>
    </div>
  );
}
```

3. **Crear rutas anidadas para categorías:**

   ```bash
   app/
   └── categories/
       └── [category]/
           └── products/
               └── [id]/
                   └── page.tsx
   ```

**`app/categories/[category]/products/[id]/page.tsx`:**
```tsx
interface CategoryProductProps {
  params: {
    category: string;
    id: string;
  };
}

export default function CategoryProduct({ params }: CategoryProductProps) {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">
        Producto {params.id} en {params.category}
      </h1>
      <p>Categoría: {params.category}</p>
      <p>ID del producto: {params.id}</p>
    </div>
  );
}
```

4. **Probar:** Visita `/products/123` y `/categories/electronics/products/456`

---

## Ejercicio 3: Rutas de Captura Total (Catch-All)

### Instrucciones:
1. **Crear una ruta de documentación con captura total:**

   ```bash
   app/
   └── docs/
       └── [...slug]/
           └── page.tsx
   ```

2. **Implementar la página de documentación:**

**`app/docs/[...slug]/page.tsx`:**
```tsx
interface DocsPageProps {
  params: {
    slug: string[];
  };
}

export default function DocsPage({ params }: DocsPageProps) {
  const path = params.slug.join('/');
  
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">Documentación</h1>
      <div className="bg-blue-50 p-4 rounded-lg mb-4">
        <p><strong>Ruta actual:</strong> /docs/{path}</p>
        <p><strong>Segmentos:</strong> {JSON.stringify(params.slug)}</p>
      </div>
      <div className="prose">
        <h2>Contenido de la documentación</h2>
        <p>Esta es la página para: {path}</p>
      </div>
    </div>
  );
}
```

3. **Probar rutas como:**

   - `/docs/getting-started`
   - `/docs/api/authentication/oauth`
   - `/docs/guides/deployment/vercel`

---

## Ejercicio 4: Navegación con Link y Programática

### Parte A: Navegación con Link

#### Instrucciones:
1. **Crear un componente de navegación:**

**`app/components/Navigation.tsx`:**
```tsx
import Link from 'next/link';

export default function Navigation() {
  return (
    <nav className="bg-blue-600 text-white p-4">
      <div className="container mx-auto flex space-x-6">
        <Link href="/" className="hover:text-blue-200">
          Inicio
        </Link>
        <Link href="/about" className="hover:text-blue-200">
          Acerca de
        </Link>
        <Link href="/products" className="hover:text-blue-200">
          Productos
        </Link>
        <Link href="/contact" className="hover:text-blue-200">
          Contacto
        </Link>
      </div>
    </nav>
  );
}
```

2. **Crear enlaces dinámicos en la lista de productos:**

**`app/products/page.tsx`:**
```tsx
import Link from 'next/link';

const products = [
  { id: '1', name: 'Laptop Gaming', price: 1299, category: 'electronics' },
  { id: '2', name: 'Mouse Inalámbrico', price: 49, category: 'accessories' },
  { id: '3', name: 'Teclado Mecánico', price: 129, category: 'accessories' },
];

export default function ProductsPage() {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-6">Nuestros Productos</h1>
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        {products.map((product) => (
          <div key={product.id} className="border rounded-lg p-4 shadow-md">
            <h3 className="text-lg font-semibold">{product.name}</h3>
            <p className="text-gray-600">${product.price}</p>
            <div className="mt-4 space-y-2">
              <Link 
                href={`/products/${product.id}`}
                className="block bg-blue-500 text-white px-4 py-2 rounded text-center hover:bg-blue-600"
              >
                Ver Detalles
              </Link>
              <Link 
                href={`/categories/${product.category}/products/${product.id}`}
                className="block bg-green-500 text-white px-4 py-2 rounded text-center hover:bg-green-600"
              >
                Ver en Categoría
              </Link>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}
```

### Parte B: Navegación Programática Básica

#### Instrucciones:
1. **Crear un componente con botones de navegación programática:**

**`app/components/NavigationButtons.tsx`:**
```tsx
'use client';
import { useRouter } from 'next/navigation';

export default function NavigationButtons() {
  const router = useRouter();

  const handleGoHome = () => {
    router.push('/');
  };

  const handleGoBack = () => {
    router.back();
  };

  const handleGoForward = () => {
    router.forward();
  };

  const handleRefresh = () => {
    router.refresh();
  };

  return (
    <div className="bg-gray-100 p-4 rounded-lg mb-6">
      <h3 className="text-lg font-semibold mb-4">Navegación Programática</h3>
      <div className="flex flex-wrap gap-2">
        <button 
          onClick={handleGoHome}
          className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        >
          Ir al Inicio
        </button>
        <button 
          onClick={handleGoBack}
          className="bg-gray-500 text-white px-4 py-2 rounded hover:bg-gray-600"
        >
          Atrás
        </button>
        <button 
          onClick={handleGoForward}
          className="bg-gray-500 text-white px-4 py-2 rounded hover:bg-gray-600"
        >
          Adelante
        </button>
        <button 
          onClick={handleRefresh}
          className="bg-orange-500 text-white px-4 py-2 rounded hover:bg-orange-600"
        >
          Refrescar
        </button>
      </div>
    </div>
  );
}
```

2. **Crear un formulario de búsqueda con navegación:**

**`app/components/SearchForm.tsx`:**
```tsx
'use client';
import { useState } from 'react';
import { useRouter } from 'next/navigation';

export default function SearchForm() {
  const [searchTerm, setSearchTerm] = useState('');
  const [category, setCategory] = useState('');
  const router = useRouter();

  const handleSearch = (e: React.FormEvent) => {
    e.preventDefault();
    
    if (searchTerm.trim()) {
      // Navegar a una página de resultados de búsqueda
      const searchParams = new URLSearchParams();
      searchParams.set('q', searchTerm);
      if (category) {
        searchParams.set('category', category);
      }
      
      router.push(`/search?${searchParams.toString()}`);
    }
  };

  const handleQuickSearch = (term: string) => {
    router.push(`/search?q=${encodeURIComponent(term)}`);
  };

  return (
    <div className="bg-white p-6 rounded-lg shadow-md mb-6">
      <h3 className="text-lg font-semibold mb-4">Búsqueda de Productos</h3>
      
      <form onSubmit={handleSearch} className="space-y-4">
        <div>
          <input
            type="text"
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            placeholder="Buscar productos..."
            className="w-full p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>
        
        <div>
          <select
            value={category}
            onChange={(e) => setCategory(e.target.value)}
            className="w-full p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
          >
            <option value="">Todas las categorías</option>
            <option value="electronics">Electrónicos</option>
            <option value="accessories">Accesorios</option>
            <option value="clothing">Ropa</option>
          </select>
        </div>
        
        <button
          type="submit"
          className="w-full bg-blue-500 text-white p-3 rounded-lg hover:bg-blue-600"
        >
          Buscar
        </button>
      </form>

      <div className="mt-4">
        <p className="text-sm text-gray-600 mb-2">Búsquedas rápidas:</p>
        <div className="flex flex-wrap gap-2">
          <button
            onClick={() => handleQuickSearch('laptop')}
            className="bg-gray-200 text-gray-700 px-3 py-1 rounded text-sm hover:bg-gray-300"
          >
            Laptop
          </button>
          <button
            onClick={() => handleQuickSearch('mouse')}
            className="bg-gray-200 text-gray-700 px-3 py-1 rounded text-sm hover:bg-gray-300"
          >
            Mouse
          </button>
          <button
            onClick={() => handleQuickSearch('teclado')}
            className="bg-gray-200 text-gray-700 px-3 py-1 rounded text-sm hover:bg-gray-300"
          >
            Teclado
          </button>
        </div>
      </div>
    </div>
  );
}
```

3. **Crear la página de resultados de búsqueda:**

**`app/search/page.tsx`:**
```tsx
'use client';
import { useSearchParams, useRouter } from 'next/navigation';
import { Suspense } from 'react';

function SearchResults() {
  const searchParams = useSearchParams();
  const router = useRouter();
  const query = searchParams.get('q') || '';
  const category = searchParams.get('category') || '';

  const handleClearSearch = () => {
    router.push('/products');
  };

  return (
    <div className="container mx-auto p-8">
      <div className="mb-6">
        <h1 className="text-3xl font-bold mb-2">Resultados de Búsqueda</h1>
        <div className="bg-blue-50 p-4 rounded-lg">
          <p><strong>Término de búsqueda:</strong> {query}</p>
          {category && <p><strong>Categoría:</strong> {category}</p>}
        </div>
      </div>

      <div className="mb-4">
        <button
          onClick={handleClearSearch}
          className="bg-gray-500 text-white px-4 py-2 rounded hover:bg-gray-600"
        >
          Ver Todos los Productos
        </button>
      </div>

      <div className="bg-yellow-50 p-4 rounded-lg">
        <p className="text-gray-700">
          Aquí se mostrarían los resultados filtrados para: <strong>{query}</strong>
          {category && ` en la categoría: ${category}`}
        </p>
        <p className="text-sm text-gray-600 mt-2">
          (En una aplicación real, aquí harías una consulta a tu base de datos)
        </p>
      </div>
    </div>
  );
}

export default function SearchPage() {
  return (
    <Suspense fallback={<div>Cargando resultados...</div>}>
      <SearchResults />
    </Suspense>
  );
}
```

4. **Integrar los componentes en las páginas existentes:**

**Actualizar `app/page.tsx`:**
```tsx
import NavigationButtons from './components/NavigationButtons';
import SearchForm from './components/SearchForm';

export default function Home() {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-4xl font-bold mb-4">Mi Tienda Online</h1>
      <p className="text-lg mb-6">Bienvenido a nuestra tienda virtual</p>
      
      <NavigationButtons />
      <SearchForm />
      
      <div className="bg-gray-50 p-6 rounded-lg">
        <h2 className="text-2xl font-semibold mb-4">Funcionalidades Implementadas</h2>
        <ul className="list-disc list-inside space-y-2">
          <li>Navegación con componente Link</li>
          <li>Navegación programática con useRouter</li>
          <li>Formulario de búsqueda con parámetros URL</li>
          <li>Botones de navegación (atrás, adelante, refrescar)</li>
        </ul>
      </div>
    </div>
  );
}
```

### Ejercicios Prácticos de Navegación:

1. **Prueba la navegación con Link:**
   - Navega entre las páginas usando el menú de navegación
   - Observa cómo no se recarga la página completa

2. **Prueba la navegación programática:**
   - Usa los botones de navegación en la página de inicio
   - Prueba el formulario de búsqueda con diferentes términos

3. **Experimenta con parámetros URL:**
   - Realiza búsquedas y observa cómo cambia la URL
   - Usa el botón "atrás" del navegador y observa el comportamiento

4. **Verifica el comportamiento:**
   - Comprueba que `router.back()` funciona correctamente
   - Verifica que `router.refresh()` actualiza la página
   - Confirma que los parámetros de búsqueda se mantienen en la URL

