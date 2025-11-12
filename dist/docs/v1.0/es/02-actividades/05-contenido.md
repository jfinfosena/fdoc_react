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

### Indicaciones: `components/EventCard.jsx`

- Crea un componente `EventCard` como declaración de función.
- Recibe una prop `event` con propiedades `title`, `date`, `location` y `price`.
- Agrega estilo utilizando Tailwind CSS (estructura visual tipo tarjeta).

Pistas
- Muestra el título del evento y debajo la ubicación.
- Formatea la fecha a un formato legible para el usuario.
- Presenta el precio resaltado visualmente.

### Indicaciones: `components/EventList.jsx`

- Importa el componente `EventCard` desde el archivo correspondiente.
- Declara un arreglo `events` de objetos con propiedades `id`, `title`, `date`, `location` y `price`.
- Implementa el componente `EventList` como función que:
  - Agrega estilo utilizando Tailwind CSS.
  - Itera sobre `events` usando `.map()` para crear una tarjeta por cada elemento.
  - Pasa el objeto actual como prop `event` a `EventCard`.
  - Asigna la prop `key` utilizando el `id` de cada objeto.
- Exporta el componente como valor por defecto.

Validación
- Al montar `EventList` en una página, verifica que se renderice una tarjeta por cada elemento del arreglo.
- Cambia los valores del arreglo para confirmar que la UI se actualiza correctamente.

### Consigna

- Inserta ambos archivos en tu proyecto y muéstralos en una página.
 - Puedes modificar el arreglo `events` para probar diferentes datos.

