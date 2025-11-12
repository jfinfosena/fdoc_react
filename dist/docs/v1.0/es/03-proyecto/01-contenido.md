---
title: "Proyecto Integrador: Aplicación Web con Next.js y React"
position: 0
date: 2025-11-10
---


### 1) Crear Fork del Repositorio
Solo el líder del grupo debe crear el repositorio para la actividad.

```bash
https://github.com/jfinfosena/proyecto_integrador_frontend.git
```

Pulsa el botón **Fork** (esquina superior derecha) desde la cuenta del líder.

### 2) El líder clona su fork en su equipo:

```bash
git clone https://github.com/LIDER-DEL-GRUPO/proyecto_integrador_frontend.git
cd proyecto_integrador_frontend
```

### 3) El líder agrega a los integrantes como colaboradores en su fork (Settings → Collaborators) o comparte el enlace del fork para que ellos lo clonen directamente:

```bash
git clone https://github.com/LIDER-DEL-GRUPO/proyecto_integrador_frontend.git
```

### 4) Flujo de trabajo recomendado:
- Rama `main` protegida (sin commits directos).
- Cada integrante crea ramas por funcionalidad: `feature/<nombre>` (ej. `feature/autenticacion`, `feature/productos`).
- Trabaja con Pull Requests y revisiones antes de fusionar.

### 5) Registro del grupo: info.json
Antes de iniciar la preparación, diligencia el archivo `info.json` en la raíz del repositorio del líder con la siguiente estructura:

```json
{
  "nombres": "",
  "apellidos": "",
  "grupo": "web-#"
}
```

Indicaciones:
- `nombres` y `apellidos`: del líder del grupo.
- `grupo`: reemplaza `#` por el número asignado (ej. `web-3`).
- Este archivo sirve para identificar el grupo durante la evaluación.
- Inclúyelo en el primer commit del fork y actualízalo si cambia el liderazgo.

## Indicaciones para el Proyecto Integrador con Next.js

### Instrucciones Generales
Cada grupo debe desarrollar una **aplicación web con Next.js (App Router) y React**, aplicando los conceptos vistos en clase. El proyecto debe partir de una idea elegida por el grupo (por ejemplo: catálogo de productos, gestor de tareas, blog, eventos, reservas, etc.).

### Requisitos mínimos
- App Router con `app/layout.tsx`, `app/page.tsx`, `app/globals.css`, `app/favicon.ico`.
- Grupos de rutas por dominio (ej. `(auth)` y `(content)`), con layouts específicos por grupo.
- Autenticación: páginas de `login` y `register`.
- Listado y detalle con ruta dinámica (ej. `/items` y `/items/[id]`).
- API Routes en `app/api/*` con al menos un `GET` y un `POST` (puede ser persistencia en memoria o mock).
- Estado global y compartido: `contexts/*` (ej. `AuthContext`) y/o `stores/*`.
- Hooks personalizados en `hooks/*` para encapsular acceso a estado/servicios.
- Capa de servicios `services/*` para llamadas a API/servidores externos.
- Tipos TypeScript en `types/*` para entidades del dominio.
- Componentes UI reutilizables en `components/ui/*` y layout en `components/layout/*`.
- Estilos con Tailwind (u otra solución acordada), uso de `loading.tsx` y `not-found.tsx` donde aplique.

### Estructura recomendada
Sigue la organización por capas y dominios que se propone en `docs/estructura-proyecto.md`. Esta estructura facilita el mantenimiento, promueve la reutilización y evita mezclar responsabilidades entre UI, estado y acceso a datos.

### Trabajo en equipo
- Usa ramas por funcionalidad y Pull Requests al fork del líder.
- Asegura consistencia de estilos y convenciones entre módulos.
- Revisa y prueba cada aporte antes de fusionarlo a `main`.

### Entregables
- Código completo en el fork del líder con historial de commits.
- Documentación en el archivo README.md.
- Demostración local corriendo la app; 
- Despliegue (ej. Vercel).


