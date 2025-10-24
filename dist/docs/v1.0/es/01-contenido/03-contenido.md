---
title: "Clase 3 - ¿Qué son las props en React?"
position: 3
date: 2025-10-22
---

Las **props** (propiedades) son un mecanismo en React para pasar datos de un componente padre a un componente hijo. Son inmutables desde el componente hijo, lo que significa que el hijo no puede modificar las props directamente. Las props permiten que los componentes sean reutilizables y dinámicos.

- **Características principales**:
  - Son un objeto (`props`) que contiene los datos pasados al componente.
  - Pueden incluir cualquier tipo de dato: cadenas, números, objetos, arreglos, funciones, componentes, etc.
  - Son unidireccionales (del padre al hijo).
  - Son opcionales, pero se pueden establecer valores por defecto.

---

## **Formas de usar props en React**

### 1. **Pasar props básicas (cadenas, números, booleanos)**

Las props más simples son valores primitivos como cadenas, números o booleanos.

**Ejemplo**:

```jsx
// Componente hijo
function Saludo(props) {
  return <h1>Hola, {props.nombre}!</h1>;
}

// Componente padre
function App() {
  return <Saludo nombre="Juan" />;
}
```

**Explicación**:

- El componente padre `App` pasa la prop `nombre` con el valor `"Juan"`.
- El componente hijo `Saludo` accede a la prop a través del objeto `props` (`props.nombre`).
- En el renderizado, se muestra "Hola, Juan!".

**Salida**:
```
Hola, Juan!
```

---

### 2. **Desestructuración de props**

En componentes funcionales, puedes desestructurar las props directamente en los parámetros para escribir un código más limpio.

**Ejemplo**:

```jsx
// Componente hijo con desestructuración
function Saludo({ nombre, edad }) {
  return <h1>Hola, {nombre}. Tienes {edad} años.</h1>;
}

// Componente padre
function App() {
  return <Saludo nombre="Ana" edad={25} />;
}
```

**Explicación**:

- En lugar de usar `props.nombre` y `props.edad`, desestructuramos `{ nombre, edad }` en los parámetros.
- Esto hace que el código sea más legible y evita repetir `props.`.

**Salida**:
```
Hola, Ana. Tienes 25 años.
```

---

### 3. **Pasar objetos o arreglos como props**

Puedes pasar estructuras de datos más complejas, como objetos o arreglos, como props.

**Ejemplo**:

```jsx
// Componente hijo
function Perfil({ usuario }) {
  return (
    <div>
      <h2>{usuario.nombre}</h2>
      <p>Hobbies: {usuario.hobbies.join(", ")}</p>
    </div>
  );
}

// Componente padre
function App() {
  const usuario = {
    nombre: "María",
    hobbies: ["leer", "caminar", "programar"],
  };
  return <Perfil usuario={usuario} />;
}
```

 chatbot
**Explicación**:

- La prop `usuario` es un objeto que contiene `nombre` y `hobbies`.
- En el componente hijo, accedemos a las propiedades del objeto con `usuario.nombre` y `usuario.hobbies`.
- Usamos `join(", ")` para convertir el arreglo de hobbies en una cadena separada por comas.

**Salida**:
```
María
Hobbies: leer, caminar, programar
```

---

### 4. **Pasar funciones como props**

Puedes pasar funciones como props para permitir que el componente hijo ejecute lógica definida en el componente padre.

**Ejemplo**:

```jsx
// Componente hijo
function Boton({ onClick, texto }) {
  return <button onClick={onClick}>{texto}</button>;
}

// Componente padre
function App() {
  const manejarClick = () => {
    alert("¡Botón clicado!");
  };

  return <Boton onClick={manejarClick} texto="Clic aquí" />;
}
```

**Explicación**:

- La prop `onClick` es una función que se pasa desde el componente padre.
- Cuando el usuario hace clic en el botón, se ejecuta la función `manejarClick` definida en el componente padre.

**Salida**:
Un botón con el texto "Clic aquí" que muestra una alerta al hacer clic.

---

### 5. **Pasar componentes como props (Render Props)**

Puedes pasar un componente como prop, lo que permite renderizar contenido dinámico en el componente hijo.

**Ejemplo**:

```jsx
// Componente hijo
function Contenedor({ contenido }) {
  return <div>{contenido}</div>;
}

// Componente padre
function App() {
  const ContenidoDinamico = <h1>¡Este es un componente dinámico!</h1>;
  return <Contenedor contenido={ContenidoDinamico} />;
}
```

**Explicación**:
- La prop `contenido` es un elemento JSX que se renderiza directamente en el componente hijo.
- Esto es útil para patrones como "render props", donde el hijo decide cómo renderizar contenido pasado por el padre.

**Salida**:
```
¡Este es un componente dinámico!
```

---

### 6. **Props con children**

La prop especial `children` permite pasar contenido entre las etiquetas de apertura y cierre de un componente.

**Ejemplo**:

```jsx
// Componente hijo
function Caja({ children }) {
  return <div style={{ border: "1px solid black", padding: "10px" }}>{children}</div>;
}

// Componente padre
function App() {
  return (
    <Caja>
      <h1>Título dentro de la caja</h1>
      <p>Este es un párrafo.</p>
    </Caja>
  );
}
```

**Explicación**:

- Todo lo que se coloca entre `<Caja>` y `</Caja>` se pasa como la prop `children`.
- El componente hijo renderiza `children` donde desee.

**Salida**:
Un `div` con borde que contiene un título y un párrafo.

---

### 7. **Valores por defecto con defaultProps**

Puedes definir valores por defecto para las props usando `defaultProps` en componentes funcionales o de clase.

**Ejemplo (Componente funcional)**:

```jsx
// Componente hijo
function Saludo({ nombre }) {
  return <h1>Hola, {nombre}!</h1>;
}

Saludo.defaultProps = {
  nombre: "Invitado",
};

// Componente padre
function App() {
  return <Saludo />;
}
```

**Explicación**:

- Si no se pasa la prop `nombre`, se usa el valor por defecto `"Invitado"`.
- Esto es útil para evitar errores cuando las props no se proporcionan.

**Salida**:
```
Hola, Invitado!
```

---

### 8. **Validación de props con PropTypes**

Para asegurar que las props tengan el tipo y formato correctos, puedes usar la librería `prop-types`.

**Instalación**:
```bash
npm install prop-types
```

**Ejemplo**:

```jsx
import PropTypes from 'prop-types';

// Componente hijo
function Perfil({ nombre, edad, activo }) {
  return (
    <div>
      <h2>{nombre}</h2>
      <p>Edad: {edad}</p>
      <p>{activo ? "Activo" : "Inactivo"}</p>
    </div>
  );
}

Perfil.propTypes = {
  nombre: PropTypes.string.isRequired,
  edad: PropTypes.number.isRequired,
  activo: PropTypes.bool,
};

Perfil.defaultProps = {
  activo: false,
};

// Componente padre
function App() {
  return <Perfil nombre="Carlos" edad={30} />;
}
```

**Explicación**:

- `PropTypes.string.isRequired` asegura que `nombre` sea una cadena y sea obligatorio.
- `PropTypes.number.isRequired` asegura que `edad` sea un número y obligatorio.
- `PropTypes.bool` indica que `activo` debe ser un booleano, pero es opcional.
- Si no se pasa `activo`, se usa el valor por defecto `false`.

**Salida**:
```
Carlos
Edad: 30
Inactivo
```

---


## Props en React con TypeScript

## Introducción

En React, los **props** son el mecanismo principal para pasar datos de un componente padre a un componente hijo. Con TypeScript, se puede añadir tipado estático para mejorar la seguridad y mantenibilidad del código, asegurando que los props tengan el tipo correcto y evitando errores en tiempo de ejecución. 

TypeScript permite definir interfaces o tipos para los props, proporcionando autocompletado, validación de tipos y mejor documentación. A continuación, se presentan ejemplos prácticos en formato MkDocs Material, en español.

---

## Configuración Básica

Para usar React con TypeScript, asegúrate de tener un proyecto configurado con las siguientes dependencias:

```bash
npm install typescript @types/react @types/react-dom
```

Los archivos de componentes deben tener la extensión `.tsx`.

---

## Definición de Props con TypeScript

Sin usar `React.FC`, definimos los props mediante una **interfaz** y tipamos explícitamente los parámetros de la función del componente.

### Ejemplo 1: Componente con Props Básicos

Un componente que muestra el nombre y la edad de una persona.

```tsx
// Definimos la interfaz para los props
interface PersonaProps {
  nombre: string;
  edad: number;
}

// Componente funcional con props tipadas
function Persona({ nombre, edad }: PersonaProps) {
  return (
    <div>
      <h2>{nombre}</h2>
      <p>Edad: {edad}</p>
    </div>
  );
}

// Uso del componente
function App() {
  return <Persona nombre="Ana" edad={25} />;
}

export default App;
```

**Explicación**:
- La interfaz `PersonaProps` define las propiedades `nombre` (string) y `edad` (number).
- En lugar de `React.FC`, tipamos los props directamente en la desestructuración de la función (`{ nombre, edad }: PersonaProps`).
- TypeScript asegura que al usar el componente `Persona`, se pasen las props con los tipos correctos.

---

## Props Opcionales

Las props opcionales se marcan con `?` en la interfaz.

### Ejemplo 2: Props Opcionales

```tsx
interface SaludoProps {
  mensaje: string;
  nombre?: string; // Prop opcional
}

function Saludo({ mensaje, nombre }: SaludoProps) {
  return (
    <div>
      <h1>{mensaje}</h1>
      {nombre && <p>Dirigido a: {nombre}</p>}
    </div>
  );
}

function App() {
  return (
    <>
      <Saludo mensaje="¡Hola, mundo!" />
      <Saludo mensaje="¡Bienvenido!" nombre="Carlos" />
    </>
  );
}

export default App;
```

**Explicación**:
- La prop `nombre` es opcional (`nombre?: string`).
- Usamos una condición (`nombre && ...`) para manejar el caso en que `nombre` no se pase.

---

## Props con Valores por Defecto

Los valores por defecto se asignan mediante desestructuración en los parámetros de la función.

### Ejemplo 3: Props con Valores por Defecto

```tsx
interface BotonProps {
  texto: string;
  color?: string;
}

function Boton({ texto, color = "azul" }: BotonProps) {
  return <button style={{ backgroundColor: color }}>{texto}</button>;
}

function App() {
  return (
    <>
      <Boton texto="Clic aquí" /> {/* Usa color por defecto: azul */}
      <Boton texto="Enviar" color="verde" />
    </>
  );
}

export default App;
```

**Explicación**:
- La prop `color` es opcional y tiene un valor por defecto de `"azul"` en la desestructuración.
- Esto elimina la necesidad de verificar si `color` existe en el componente.

---

## Props con Tipos Complejos

TypeScript permite definir props con estructuras más complejas, como objetos, arrays o funciones.

### Ejemplo 4: Props con Objetos y Funciones

```tsx
interface Producto {
  id: number;
  nombre: string;
  precio: number;
}

interface CarritoProps {
  productos: Producto[];
  onEliminar: (id: number) => void;
}

function Carrito({ productos, onEliminar }: CarritoProps) {
  return (
    <div>
      <h2>Carrito de Compras</h2>
      <ul>
        {productos.map((producto) => (
          <li key={producto.id}>
            {producto.nombre} - ${producto.precio}
            <button onClick={() => onEliminar(producto.id)}>Eliminar</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

function App() {
  const productosEjemplo: Producto[] = [
    { id: 1, nombre: "Laptop", precio: 1000 },
    { id: 2, nombre: "Teléfono", precio: 500 },
  ];

  const handleEliminar = (id: number) => {
    console.log(`Eliminando producto con ID: ${id}`);
  };

  return <Carrito productos={productosEjemplo} onEliminar={handleEliminar} />;
}

export default App;
```

**Explicación**:
- La interfaz `Producto` define la estructura de un objeto producto.
- `CarritoProps` incluye un array de productos (`productos`) y una función (`onEliminar`).
- TypeScript garantiza que la función `onEliminar` y los elementos de `productos` cumplan con los tipos definidos.

---

## Props de Componentes como Props (Children)

Los componentes pueden recibir otros componentes o elementos como props a través de `children`.

### Ejemplo 5: Usando Children

```tsx
interface ContenedorProps {
  children: React.ReactNode;
  titulo: string;
}

function Contenedor({ children, titulo }: ContenedorProps) {
  return (
    <div className="contenedor">
      <h2>{titulo}</h2>
      {children}
    </div>
  );
}

function App() {
  return (
    <Contenedor titulo="Mi Contenedor">
      <p>Este es el contenido dentro del contenedor.</p>
      <button>Acción</button>
    </Contenedor>
  );
}

export default App;
```

**Explicación**:
- La prop `children` usa el tipo `React.ReactNode`, que permite pasar cualquier contenido React válido.
- El componente `Contenedor` renderiza el título y el contenido pasado como `children`.
