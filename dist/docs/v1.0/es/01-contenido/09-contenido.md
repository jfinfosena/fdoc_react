---
title: "Configurar Next.js con Clerk para autenticación "
position: 9
date: 2025-11-19
---


Este tutorial se centra en el uso del App Router de Next.js y la integración con Clerk para autenticación de usuarios.

[https://clerk.com/docs/quickstarts/nextjs](https://clerk.com/docs/quickstarts/nextjs)


### Configuración completa con rutas públicas y protegidas

```typescript
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'

// Definir rutas públicas (accesibles sin autenticación)
const isPublicRoute = createRouteMatcher([
  '/',
  '/about',
  '/contact',
  '/pricing',
  '/sign-in(.*)',
  '/sign-up(.*)',
  '/api/public(.*)'
])

// Definir rutas que requieren autenticación básica
const isProtectedRoute = createRouteMatcher([
  '/dashboard(.*)',
  '/profile(.*)',
  '/settings(.*)'
])

// Definir rutas que requieren permisos de administrador
const isAdminRoute = createRouteMatcher([
  '/admin(.*)',
  '/management(.*)'
])

export default clerkMiddleware(async (auth, req) => {
  // Si es una ruta pública, permitir acceso
  if (isPublicRoute(req)) {
    return
  }

  // Proteger rutas de administrador con permisos específicos
  if (isAdminRoute(req)) {
    await auth.protect((has) => {
      return has({ permission: 'org:admin:example1' }) || has({ permission: 'org:admin:example2' })
    })
    return
  }

  // Proteger rutas generales que requieren autenticación
  if (isProtectedRoute(req)) {
    await auth.protect()
    return
  }

  // Por defecto, proteger todas las demás rutas
  await auth.protect()
})

export const config = {
  matcher: [
    // Skip Next.js internals and all static files, unless found in search params
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    // Always run for API routes
    '/(api|trpc)(.*)',
  ],
}
```

### Ejemplo alternativo: Proteger todo excepto rutas públicas

```typescript
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'

const isPublicRoute = createRouteMatcher(['/sign-in(.*)', '/sign-up(.*)'])

export default clerkMiddleware(async (auth, request) => {
  if (!isPublicRoute(request)) {
    await auth.protect()
  }
})

export const config = {
  matcher: [
    // Skip Next.js internals and all static files, unless found in search params
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    // Always run for API routes
    '/(api|trpc)(.*)',
  ],
}
```

### Ejemplo con múltiples grupos de rutas

```typescript
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'

const isTenantRoute = createRouteMatcher(['/organization-selector(.*)', '/orgid/(.*)'])

const isTenantAdminRoute = createRouteMatcher(['/orgId/(.*)/memberships', '/orgId/(.*)/domain'])

export default clerkMiddleware(async (auth, req) => {
  // Restrict admin routes to users with specific permissions
  if (isTenantAdminRoute(req)) {
    await auth.protect((has) => {
      return has({ permission: 'org:admin:example1' }) || has({ permission: 'org:admin:example2' })
    })
  }
  // Restrict organization routes to signed in users
  if (isTenantRoute(req)) await auth.protect()
})

export const config = {
  matcher: [
    // Skip Next.js internals and all static files, unless found in search params
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    // Always run for API routes
    '/(api|trpc)(.*)',
  ],
}
```

### Puntos clave:

- **Rutas públicas**: Accesibles sin autenticación usando `createRouteMatcher()`  [(1)](https://clerk.com/docs/references/nextjs/clerk-middleware#protect-api-routes) 
- **Rutas protegidas**: Requieren autenticación usando `auth.protect()` 
- **Rutas con permisos**: Requieren roles o permisos específicos usando `auth.protect((has) => {...})` 
- **Comportamiento por defecto**: `clerkMiddleware` no protege ninguna ruta por defecto, debes optar por la protección 


