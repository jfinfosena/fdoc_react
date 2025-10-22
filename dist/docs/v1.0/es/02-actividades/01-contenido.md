---
title: "Actividad 1: Proyecto Next.js + TypeScript y componentes con props"
position: 1
date: 2025-10-22
---

Objetivo: Crear un proyecto Next.js con TypeScript y definir componentes que cubran todas las formas de props explicadas en el material de clase 3.

## Pasos simples
1) Crear proyecto Next.js con TypeScript

2) Estructura mínima (solo archivos y comentarios)
- Crea `app/page.tsx` con comentarios guía (sin importar componentes ni escribir JSX final).
- Crea `app/components/` con estos 8 archivos:
  - `SaludoBasico.tsx`
  - `SaludoDesestructurado.tsx`
  - `PerfilConObjeto.tsx`
  - `BotonConCallback.tsx`
  - `ContenedorRenderProp.tsx`
  - `CajaChildren.tsx`
  - `SaludoConDefaultProps.tsx`
  - `PerfilConPropTypes.tsx`
- En cada archivo, escribe al inicio un bloque de comentarios con:
  - Propósito (1–2 líneas).
  - Props esperadas y tipos.

3) Especificaciones de componentes (sin resolver)
- `SaludoBasico.tsx`
  - Props: `nombre: string` (requerida), `entusiasta?: boolean` (opcional).
  - Debe mostrar "Hola, {nombre}" y, si `entusiasta` es true, un énfasis adicional.
  - Casos: con y sin `entusiasta`.

- `SaludoDesestructurado.tsx`
  - Props: `nombre: string`, `edad: number`.
  - Debe usar desestructuración en la firma del componente.
  - Muestra `nombre` y `edad` claramente.

- `PerfilConObjeto.tsx`
  - Prop: `usuario: { nombre: string; hobbies: string[] }`.
  - Debe listar hobbies separados por comas.
  - Casos: `hobbies` vacío (muestra "Sin hobbies") y con varios elementos.

- `BotonConCallback.tsx`
  - Props: `onClick: () => void`, `texto: string`.
  - Debe invocar `onClick` cuando se hace clic.
  - Describe el comportamiento esperado del botón (sin función real).

- `ContenedorRenderProp.tsx`
  - Props: `contenido: React.ReactNode` o `render: () => React.ReactNode`.
  - Debe renderizar el contenido pasado.
  - Indica qué patrón eliges y por qué (breve justificación).

- `CajaChildren.tsx`
  - Prop especial: `children: React.ReactNode`.
  - Debe envolver y mostrar `children` dentro de un contenedor simple.
  - Casos: título + párrafo; lista con varios ítems.

- `SaludoConDefaultProps.tsx`
  - Props: `nombre?: string`.
  - Debe tener valor por defecto para `nombre` (en TS: valor por defecto en parámetro o fallback interno; evita `defaultProps`).
  - Caso: cuando no se pasa `nombre`, muestra "Invitado".

- `PerfilConPropTypes.tsx`
  - Props: `nombre: string`, `edad: number`, `activo?: boolean`.
  - Documenta uso de `prop-types` para validación en tiempo de ejecución.
  - Instalación (solo indicar): `npm install prop-types`.
  - Explica brevemente por qué puede complementar a TypeScript.

4) Página de demostración (`app/page.tsx`)
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


