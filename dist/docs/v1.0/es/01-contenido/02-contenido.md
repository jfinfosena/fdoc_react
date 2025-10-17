---
title: "Clase 2 - ¿Qué es un componente en React?"
position: 2
date: 2025-10-16
---


## **1. ¿Qué es un componente en React?**

Un componente en React es como un **bloque de Lego**. Cada bloque tiene una función específica (mostrar un botón, una lista, un formulario, etc.), y puedes combinarlos para construir interfaces de usuario completas. Los componentes son **reutilizables**, lo que significa que puedes usar el mismo componente en diferentes partes de tu aplicación, y son **modulares**, lo que facilita mantener y organizar el código.

Piensa en una página web como una casa:
- La casa completa es tu aplicación.
- Cada habitación (cocina, sala, baño) es un componente.
- Dentro de cada habitación, los muebles (mesa, sillas, lámpara) son componentes más pequeños.

En React, los componentes pueden ser desde algo tan simple como un botón hasta algo complejo como un formulario completo o una página entera.

---

## **2. Tipos de componentes en React**

Hay dos tipos principales de componentes en React:

### **a) Componentes funcionales**
Son la forma moderna y más sencilla de crear componentes. Se escriben como funciones de JavaScript que devuelven **JSX** (una sintaxis que mezcla HTML con JavaScript). Desde la introducción de los **Hooks** en React 16.8, los componentes funcionales son los más usados.

Ejemplo de un componente funcional:

```jsx
function Saludo() {
  return <h1>¡Hola, mundo!</h1>;
}
```

### **b) Componentes de clase**
Son la forma antigua de crear componentes, usando clases de JavaScript. Aunque aún funcionan, no son tan comunes hoy porque los componentes funcionales son más simples y los Hooks reemplazan las funcionalidades que antes requerían clases.

Ejemplo de un componente de clase:

```jsx
import React from 'react';

class Saludo extends React.Component {
  render() {
    return <h1>¡Hola, mundo!</h1>;
  }
}
```

En esta explicación, nos enfocaremos en los **componentes funcionales**, ya que son los recomendados para proyectos nuevos.

---

## **3. Creando un componente funcional**

Vamos a crear un componente paso a paso. Supongamos que queremos un componente que muestre un saludo personalizado.

### **Paso 1: Crear el componente**
Un componente funcional es simplemente una función de JavaScript que devuelve JSX.

```jsx
function Saludo() {
  return <h1>¡Hola, mundo!</h1>;
}
```

### **Paso 2: Exportar el componente**
Para usar el componente en otros archivos, debes exportarlo.

```jsx
export default function Saludo() {
  return <h1>¡Hola, mundo!</h1>;
}
```

### **Paso 3: Importar y usar el componente**
En otro archivo (por ejemplo, `App.jsx`), puedes importar y usar el componente como si fuera una etiqueta HTML.

```jsx
import Saludo from './Saludo';

function App() {
  return (
    <div>
      <Saludo />
    </div>
  );
}

export default App;
```

Cuando el navegador renderice esto, verás: **¡Hola, mundo!** en la pantalla.

---

## **4. ¿Qué es JSX?**

JSX es una extensión de JavaScript que permite escribir código que parece HTML dentro de archivos JavaScript. Es como una mezcla de HTML y JavaScript que React usa para definir la estructura de los componentes.

Por ejemplo:

```jsx
const elemento = <h1>¡Hola, mundo!</h1>;
```

Aunque parece HTML, en realidad JSX se convierte en llamadas a funciones de JavaScript. Por ejemplo, el código anterior se transforma en:

```javascript
const elemento = React.createElement('h1', null, '¡Hola, mundo!');
```

No necesitas entender esto a fondo, solo saber que JSX hace que escribir interfaces sea más fácil y legible.

### Reglas básicas de JSX:
1. **Siempre debe haber un elemento padre**: Si devuelves varios elementos, envuélvelos en un `<div>` o en un **Fragment** (`<></>`).
   ```jsx
   return (
     <>
       <h1>Título</h1>
       <p>Párrafo</p>
     </>
   );
   ```
2. **Atributos como en HTML, pero con camelCase**: Por ejemplo, en vez de `class`, usas `className`.
   ```jsx
   <div className="contenedor">Contenido</div>
   ```
3. **Código JavaScript dentro de llaves `{}`**: Puedes insertar variables o expresiones dentro de JSX usando llaves.
   ```jsx
   const nombre = "Juan";
   return <h1>¡Hola, {nombre}!</h1>;
   ```

---

## **5. Props: Hacer componentes dinámicos**

Las **props** (propiedades) son como los parámetros de una función. Permiten pasar datos a un componente para que sea dinámico y reutilizable.

### Ejemplo con props:

```jsx
function Saludo(props) {
  return <h1>¡Hola, {props.nombre}!</h1>;
}
```

Usamos el componente así:

```jsx
<Saludo nombre="Juan" />
<Saludo nombre="María" />
```

Esto renderizará:
- **¡Hola, Juan!**
- **¡Hola, María!**

---
