---
title: "Actividad 1: Proyecto Next.js (JavaScript) y componentes con props"
position: 1
date: 2025-10-22
---

Objetivo: Crear un proyecto Next.js con JavaScript y definir componentes que cubran todas las formas de props explicadas en el material de clase 3.

## Pasos simples
1) Crear proyecto Next.js con TypeScript

2) Estructura mínima (solo archivos y comentarios)
- Crea `app/page.jsx` con comentarios guía (sin importar componentes ni escribir JSX final).
- Crea `app/components/` con estos 8 archivos:
  - `SaludoBasico.jsx`
  - `SaludoDesestructurado.jsx`
  - `PerfilConObjeto.jsx`
  - `BotonConCallback.jsx`
  - `ContenedorRenderProp.jsx`
  - `CajaChildren.jsx`
  - `SaludoConDefaultProps.jsx`
  - `PerfilConPropTypes.jsx`
- En cada archivo, escribe al inicio un bloque de comentarios con:
  - Propósito (1–2 líneas).
  - Props esperadas y descripción.

3) Especificaciones de componentes (sin resolver)
- `SaludoBasico.jsx`
  - Props: nombre (string, requerida), entusiasta (boolean, opcional).
  - Debe mostrar "Hola, {nombre}" y, si entusiasta es true, un énfasis adicional.
  - Casos: con y sin entusiasta.

- `SaludoDesestructurado.jsx`
  - Props: nombre (string), edad (number).
  - Debe usar desestructuración en la firma del componente.
  - Muestra nombre y edad claramente.

- `PerfilConObjeto.jsx`
  - Prop: usuario (objeto con nombre: string; hobbies: string[]).
  - Debe listar hobbies separados por comas.
  - Casos: hobbies vacío (muestra "Sin hobbies") y con varios elementos.

- `BotonConCallback.jsx`
  - Props: onClick (función), texto (string).
  - Debe invocar onClick cuando se hace clic.
  - Describe el comportamiento esperado del botón (sin función real).

- `ContenedorRenderProp.jsx`
  - Props: contenido (elemento React) o render (función que retorna elemento React).
  - Debe renderizar el contenido pasado.
  - Indica qué patrón eliges y por qué (breve justificación).

- `CajaChildren.jsx`
  - Prop especial: children (contenido React).
  - Debe envolver y mostrar children dentro de un contenedor simple.
  - Casos: título + párrafo; lista con varios ítems.

- `SaludoConDefaultProps.jsx`
  - Props: nombre (string, opcional).
  - Debe tener valor por defecto para nombre (en JS: valor por defecto en parámetro o fallback interno; evita defaultProps).
  - Caso: cuando no se pasa nombre, muestra "Invitado".

4) Página de demostración (`app/page.jsx`)
- Escribe secciones de comentarios con ejemplos de uso previstos para cada componente.
- Incluye una lista de "Pruebas manuales" (qué observar en pantalla si se implementara).
- No importes componentes ni escribas JSX: solo guías claras.

5) Repositorio y README
- Inicializa Git, crea repositorio remoto y sube el proyecto.
- En `README.md` incluye:
  - Objetivo de la actividad.
  - Cómo correr (`npm run dev`).
  - Lista de componentes y nota: "solo documentación, sin implementación".


## Entrega
- Enlace al repositorio remoto GitHub (Entrega el lider del grupo en el archivo de evidencias).


