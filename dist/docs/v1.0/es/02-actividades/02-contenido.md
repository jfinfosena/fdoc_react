---
title: "Actividad 2 - Estilos CSS en Next.js (JavaScript)"
position: 2
date: 2025-10-24
---

## Antes de empezar: Fork y clonación del repositorio

- Paso 1: Realiza un fork del repositorio original en GitHub.
  - Abre `https://github.com/jfinfosena/fdoc_react_act2.git` y pulsa `Fork`.
  - Tu fork quedará en `https://github.com/<tu-usuario>/fdoc_react_act2.git`.

- Paso 2: Clona tu fork en tu equipo.
  - En la consola:
    - `git clone https://github.com/<tu-usuario>/fdoc_react_act2.git`

- Paso 3: Instala dependencias y arranca el proyecto.
  - `npm install`
  - `npm run dev`

```admonition
---
type: note
title: ""
---
Sustituye `<tu-usuario>`, `<ORGANIZACION>` y `<REPO>` por tus valores reales.
```

```admonition
---
type: info
title: "Actividad guiada"
---
Esta actividad está alineada con la **Clase 4** y te pide crear **componentes diferentes y simples** que demuestren los cuatro métodos de estilo en Next.js: **CSS Global**, **Módulos CSS**, **Estilos en línea** y **Tailwind CSS**. 
```

## Objetivo

- Diseñar cuatro componentes distintos y sencillos, cada uno usando un método de estilo diferente.
- Integrarlos en `app/page.jsx` para verlos en pantalla.

## Requisitos

- Proyecto Next.js en **JavaScript** ya creado.
- Router `app` activo (`app/page.jsx`, `app/globals.css`).
- **Tailwind CSS** opcional; si no lo tienes, completa los otros tres métodos.

## Estructura esperada

- `app/page.jsx` (página que importa y renderiza todos los componentes)
- `app/globals.css` (estilos globales y, si aplica, directivas de Tailwind)
- `app/components/PerfilGlobal.jsx` (CSS Global)
- `app/components/FichaProducto.jsx` y `app/components/FichaProducto.module.css` (Módulo CSS)
- `app/components/AvisoInline.jsx` (Estilos en línea)
- `app/components/BadgeEstado.jsx` (Tailwind CSS)

```admonition
---
type: note
title: ""
---
Cada componente debe incluir comentarios breves explicando por qué ese método de estilo es apropiado y sus limitaciones.
```

## Indicaciones por componente

### 1) PerfilGlobal.jsx (CSS Global)
- Crea una tarjeta de perfil con: nombre, texto descriptivo y botón.
- Usa clases definidas en `app/globals.css` (por ejemplo `.tarjeta`, `.boton`).
- Aplicar estilos libres

### 2) FichaProducto.jsx + FichaProducto.module.css (Módulos CSS)
- Muestra nombre del producto, precio y un botón “Agregar”.
- Define clases en `FichaProducto.module.css` y aplícalas como objeto `styles`.
- Aplicar estilos libres

### 3) AvisoInline.jsx (Estilos en línea)
- Crea un recuadro de aviso con título corto y texto.
- Aplica estilos usando el atributo `style={{ ... }}` directamente en JSX.
- Aplicar estilos libres

### 4) BadgeEstado.jsx (Tailwind CSS)
- Renderiza una insignia (badge) con el estado: "Activo" / "Inactivo".
- Aplicar estilos libres

## Integración en la página (sin código)
- Importa los cuatro componentes en `app/page.jsx`.




