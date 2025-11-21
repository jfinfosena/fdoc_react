---
title: "Clase 10 - CConsumir una API REST en Next.js 14+ (App Router) con TypeScript + shadcn/ui + MockAPI.io"
position: 10
date: 2025-11-21
---

## Introducción

En este tutorial vas a aprender a conectar Next.js 14+ (App Router) con una API real usando solo `fetch`, TypeScript y los componentes más bonitos del momento: **shadcn/ui**.

Usaremos **MockAPI.io** como backend falso.  
Al final tendrás dos páginas listas:

- `/basic` → Ejemplo didáctico con `useState` + `useEffect` (perfecto para enseñar)
- `/products` → CRUD completo profesional (crear, leer, editar, eliminar) con diseño premium

---

## Requisitos previos (solo una vez)

```bash
# 1. Crea tu proyecto Next.js con TypeScript
npx create-next-app@latest mi-crud --typescript --tailwind --eslint --app --src-dir

# 2. Instala e inicializa shadcn/ui
cd mi-crud
npx shadcn@latest init

# 3. Agrega los componentes que usaremos
npx shadcn@latest add card badge button input textarea dialog label sonner skeleton
```

### Crea tu API falsa en MockAPI.io 

1. Ve a https://mockapi.io
2. New Project → Ponle cualquier nombre (ej: `productos-2025`)
3. Copia la URL que te da:

```
https://67c9e8c0d0e1f7b1c27xxxxx.mockapi.io/api/v1/products
```

¡Listo! Ya tienes un backend real que responde GET, POST, PUT y DELETE.

---

## Estructura final del proyecto

```bash
src/
├── app/
│   ├── basic/page.tsx         → Ejemplo didáctico (useEffect + useState)
│   └── products/page.tsx      → CRUD completo profesional
├── lib/
│   ├── types/product.ts
│   └── services/productService.ts
```

---

## 1. Tipo de dato compartido

```ts
// src/lib/types/product.ts
export type Product = {
  id?: string;
  title: string;
  price: number;
  description?: string;
  image?: string;
};
```

---

## 2. Servicio reutilizable (src/lib/services/productService.ts)

```ts
// src/lib/services/productService.ts

// ⚠️ CAMBIA ESTA URL POR LA TUYA DE MOCKAPI.IO ⚠️
const API_URL = "https://TU-PROYECTO.mockapi.io/api/v1/products";

const check = async (res: Response) => {
  if (!res.ok) {
    const text = await res.text();
    throw new Error(text || "Error en la petición");
  }
  return res.json();
};

export const productService = {
  // GET ALL
  async getAll() {
    const res = await fetch(API_URL, { cache: "no-store" });
    return check(res) as Promise<Product[]>;
  },

  // CREATE
  async create(data: Omit<Product, "id">) {
    const res = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });
    return check(res) as Promise<Product>;
  },

  // UPDATE
  async update(id: string, data: Partial<Product>) {
    const res = await fetch(`${API_URL}/${id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });
    return check(res) as Promise<Product>;
  },

  // DELETE
  async remove(id: string) {
    await fetch(`${API_URL}/${id}`, { method: "DELETE" });
  },
};
```

---

## Ejemplo 1: Lo más básico con useEffect + useState  
Ruta: `http://localhost:3000/basic`

Perfecto para enseñar el flujo clásico de React.

```tsx
// src/app/basic/page.tsx
'use client';

import { useEffect, useState } from 'react';
import Image from 'next/image';
import { productService } from '@/lib/services/productService';
import type { Product } from '@/lib/types/product';

import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { Skeleton } from '@/components/ui/skeleton';

export default function BasicPage() {
  const [products, setProducts] = useState<Product[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async () => {
      try {
        setLoading(true);
        const data = await productService.getAll();
        setProducts(data);
      } catch (err: unknown) {
        const message = err instanceof Error ? err.message : "Error al cargar productos";
        setError(message);
      } finally {
        setLoading(false);
      }
    };
    load();
  }, []);

  if (loading) {
    return (
      <div className="min-h-screen bg-gray-50 py-12 px-4">
        <div className="max-w-7xl mx-auto">
          <h1 className="text-4xl font-bold text-center mb-12">Cargando productos...</h1>
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
            {[...Array(8)].map((_, i) => (
              <Card key={i}>
                <Skeleton className="h-48 w-full" />
                <CardHeader>
                  <Skeleton className="h-8 w-3/4" />
                  <Skeleton className="h-4 w-full mt-2" />
                </CardHeader>
              </Card>
            ))}
          </div>
        </div>
      </div>
    );
  }

  if (error) {
    return (
      <div className="min-h-screen bg-gray-50 flex items-center justify-center">
        <div className="text-center">
          <p className="text-red-600 text-2xl mb-4">{error}</p>
          <button onClick={() => window.location.reload()} className="px-6 py-3 bg-blue-600 text-white rounded-lg">
            Reintentar
          </button>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 py-12 px-4">
      <div className="max-w-7xl mx-auto">
        <h1 className="text-4xl font-bold text-center mb-12 text-gray-800">
          Ejemplo Básico con useEffect + useState
        </h1>

        {products.length === 0 ? (
          <p className="text-center text-xl text-gray-500">
            No hay productos. ¡Agrega algunos en MockAPI.io!
          </p>
        ) : (
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
            {products.map((p) => (
              <Card key={p.id} className="overflow-hidden hover:shadow-2xl transition-all duration-300">
                <div className="relative aspect-video bg-gray-200">
                  {p.image ? (
                    <Image
                      src={p.image}
                      alt={p.title}
                      fill
                      className="object-cover"
                      sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 25vw"
                    />
                  ) : (
                    <div className="flex items-center justify-center h-full text-gray-400">Sin imagen</div>
                  )}
                </div>
                <CardHeader>
                  <CardTitle className="line-clamp-1">{p.title}</CardTitle>
                  {p.description && <CardDescription className="line-clamp-2">{p.description}</CardDescription>}
                </CardHeader>
                <CardContent>
                  <Badge variant="secondary" className="text-lg font-bold">
                    ${p.price}
                  </Badge>
                </CardContent>
              </Card>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}
```

---

## Ejemplo 2: CRUD Profesional Completo  
Ruta: `http://localhost:3000/products`

```tsx
// src/app/products/page.tsx
'use client';

import { useEffect, useState } from 'react';
import { productService } from '@/lib/services/productService';
import type { Product } from '@/lib/types/product';

import { Card, CardDescription, CardFooter, CardHeader, CardTitle } from '@/components/ui/card';
import Image from 'next/image';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import { Dialog, DialogContent, DialogHeader, DialogTitle, DialogTrigger, DialogFooter } from '@/components/ui/dialog';
import { Label } from '@/components/ui/label';
import { Badge } from '@/components/ui/badge';
import { toast } from 'sonner';
import { Pencil, Trash2, Plus } from 'lucide-react';

export default function ProductsPage() {
  const [products, setProducts] = useState<Product[]>([]);
  const [loading, setLoading] = useState(true);
  const [open, setOpen] = useState(false);
  const [editing, setEditing] = useState<Product | null>(null);

  const load = async () => {
    setLoading(true);
    try {
      setProducts(await productService.getAll());
    } catch {
      toast.error("No se pudieron cargar los productos");
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => { load(); }, []);

  const handleCreate = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const f = new FormData(e.currentTarget);
    const newProduct = {
      title: f.get("title") as string,
      price: Number(f.get("price")),
      description: f.get("description") as string || "",
      image: (f.get("image") as string) || "https://via.placeholder.com/600x400.png?text=Nuevo+Producto",
    };

    try {
      await productService.create(newProduct);
      toast.success("¡Creado! ✅");
      setOpen(false);
      load();
    } catch {
      toast.error("No se pudo crear");
    }
  };

  const handleUpdate = async () => {
    if (!editing?.id) return;
    try {
      await productService.update(editing.id, editing);
      toast.success("¡Actualizado! ✅");
      setEditing(null);
      load();
    } catch {
      toast.error("Error");
    }
  };

  const handleDelete = async (id: string) => {
    if (!confirm("¿Eliminar este producto?")) return;
    try {
      await productService.remove(id);
      toast.success("Eliminado 🗑️");
      load();
    } catch {
      toast.error("Error");
    }
  };

  if (loading) return <div className="py-32 text-center text-2xl">Cargando productos...</div>;

  return (
    <div className="min-h-screen bg-gray-50 py-10 px-4">
      <div className="max-w-7xl mx-auto">
        <div className="flex justify-between items-center mb-10">
          <h1 className="text-4xl font-bold">Gestión de Productos</h1>
          <Dialog open={open} onOpenChange={setOpen}>
            <DialogTrigger asChild>
              <Button size="lg" className="gap-2">
                <Plus className="w-5 h-5" /> Nuevo Producto
              </Button>
            </DialogTrigger>
            <DialogContent className="sm:max-w-md">
              <DialogHeader>
                <DialogTitle>Nuevo Producto</DialogTitle>
              </DialogHeader>
              <form onSubmit={handleCreate} className="space-y-4">
                <div><Label>Título</Label><Input name="title" required /></div>
                <div><Label>Precio</Label><Input name="price" type="number" required /></div>
                <div><Label>Descripción (opcional)</Label><Textarea name="description" /></div>
                <div><Label>URL Imagen (opcional)</Label><Input name="image" placeholder="https://..." /></div>
                <DialogFooter>
                  <Button type="submit">Crear Producto</Button>
                </DialogFooter>
              </form>
            </DialogContent>
          </Dialog>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
          {products.map((p) => (
            <Card key={p.id} className="overflow-hidden group hover:shadow-2xl transition-all">
              <div className="relative h-56">
                <Image
                  src={p.image || "https://via.placeholder.com/600x400.png?text=Sin+Imagen"}
                  alt={p.title}
                  fill
                  className="object-cover group-hover:scale-105 transition-transform"
                  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 25vw"
                  unoptimized
                />
              </div>
              <CardHeader>
                {editing && editing.id === p.id ? (
                  <div className="space-y-3">
                    <Input value={editing.title} onChange={e => setEditing({ ...editing, title: e.target.value })} />
                    <Input type="number" value={editing.price} onChange={e => setEditing({ ...editing, price: Number(e.target.value) })} />
                    <Textarea value={editing.description || ""} onChange={e => setEditing({ ...editing, description: e.target.value })} />
                    <Input value={editing.image || ""} onChange={e => setEditing({ ...editing, image: e.target.value })} placeholder="URL imagen" />
                    <div className="flex gap-2">
                      <Button size="sm" onClick={handleUpdate}>Guardar</Button>
                      <Button size="sm" variant="outline" onClick={() => setEditing(null)}>Cancelar</Button>
                    </div>
                  </div>
                ) : (
                  <>
                    <CardTitle>{p.title}</CardTitle>
                    {p.description && <CardDescription>{p.description}</CardDescription>}
                    <Badge className="mt-2 w-fit">${p.price}</Badge>
                  </>
                )}
              </CardHeader>

              {editing?.id !== p.id && (
                <CardFooter className="flex justify-between">
                  <Button variant="outline" size="sm" onClick={() => setEditing(p)}>
                    <Pencil className="w-4 h-4 mr-1" /> Editar
                  </Button>
                  <Button variant="destructive" size="sm" onClick={() => handleDelete(p.id!)}>
                    <Trash2 className="w-4 h-4" />
                  </Button>
                </CardFooter>
              )}
            </Card>
          ))}
        </div>
      </div>
    </div>
  );
}
```


