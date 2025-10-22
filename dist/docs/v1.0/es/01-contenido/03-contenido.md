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

