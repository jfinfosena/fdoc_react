---
title: "Actividad 4: Lista con `.map()` y componente Tarjeta"
position: 5
date: 2025-11-12
---

- Objetivo: usar `.map()` para renderizar una lista desde un arreglo de objetos llamando un componente tipo tarjeta.
- Entrega: crear los dos componentes y verificar que se renderiza una tarjeta por cada elemento.


## Antes de empezar: Fork y clonación del repositorio

- **Paso 1:** Realiza un fork del repositorio original en GitHub.
  - Abre `https://github.com/jfinfosena/fdoc_react_act4.git` y pulsa `Fork`.
  - Tu fork quedará en `https://github.com/<tu-usuario>/fdoc_react_act4.git`.

- **Paso 2:** Clona tu fork en tu equipo.
  - En la consola:
    - `git clone https://github.com/<tu-usuario>/fdoc_react_act4.git`
    - `cd fdoc_react_act4`

- **Paso 3:** Instala dependencias y arranca el proyecto.
  - `npm install`
  - `npm run dev`

---

### Indicaciones: `components/ProductCard.jsx`

- Crea un componente `ProductCard` como declaración de función.
- Recibe una prop `product` con propiedades `title`, `category` y `price`.
- Agrega estilo utilizando Tailwind CSS.
.

### Indicaciones: `components/ProductList.jsx`

- Importa el componente `ProductCard` desde el archivo correspondiente.
- Declara un arreglo `products` de objetos con propiedades `id`, `title`, `price` y `category`.
- Implementa el componente `ProductList` como función que:
  - Agrega estilo utilizando Tailwind CSS.
  - Itera sobre `products` usando `.map()` para crear una tarjeta por cada elemento.
  - Pasa el objeto actual como prop `product` a `ProductCard`.
  - Asigna la prop `key` utilizando el `id` de cada objeto.
- Exporta el componente como valor por defecto.

Validación
- Al montar `ProductList` en una página, verifica que se renderice una tarjeta por cada elemento del arreglo.
- Cambia los valores del arreglo para confirmar que la UI se actualiza correctamente.

### Consigna

- Inserta ambos archivos en tu proyecto y muéstralos en una página.
- Puedes modificar el arreglo `products` para probar diferentes datos.

