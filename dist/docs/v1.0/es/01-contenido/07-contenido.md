---
title: "Clase 7 -  Hook useState"
position: 7
date: 2025-11-14
---

## ¿Qué es useState?

El hook `useState` es la forma más simple de agregar **estado** a un componente funcional en React. Piensa en el estado como la "memoria" de tu componente - información que puede cambiar y que React necesita recordar.

### Concepto Simple
```jsx
const [valor, setValor] = useState(valorInicial);
```

- `valor`: Lo que queremos recordar
- `setValor`: Función para cambiar ese valor
- `valorInicial`: El valor al inicio

## Importación

Siempre debemos importar useState antes de usarlo:

```jsx
import { useState } from 'react';
```

## Ejemplo 1: Mi Primer Estado

Comencemos con algo súper simple - un contador:

```jsx
import { useState } from 'react';

function MiContador() {
  // Creamos un estado que empieza en 0
  const [numero, setNumero] = useState(0);

  return (
    <div>
      <h2>El número es: {numero}</h2>
      <button onClick={() => setNumero(numero + 1)}>
        Sumar 1
      </button>
    </div>
  );
}
```

**¿Qué pasa aquí?**
1. `useState(0)` crea un estado que empieza en 0
2. `numero` contiene el valor actual
3. `setNumero` es la función para cambiarlo
4. Cuando hacemos clic, `setNumero(numero + 1)` aumenta el número
5. React automáticamente actualiza la pantalla

## Ejemplo 2: Texto que Cambia

Ahora veamos cómo manejar texto:

```jsx
function SaludoPersonalizado() {
  const [nombre, setNombre] = useState('');

  return (
    <div>
      <input 
        type="text"
        value={nombre}
        onChange={(e) => setNombre(e.target.value)}
        placeholder="Escribe tu nombre"
      />
      <h2>¡Hola {nombre}!</h2>
    </div>
  );
}
```

**Explicación paso a paso:**
1. `useState('')` crea un estado de texto vacío
2. `value={nombre}` conecta el input con nuestro estado
3. `onChange` se ejecuta cada vez que escribimos
4. `e.target.value` es lo que escribió el usuario
5. `setNombre(e.target.value)` actualiza nuestro estado

## Tipos de Estados Comunes

### 1. Números
```jsx
function Calculadora() {
  const [precio, setPrecio] = useState(0);
  const [cantidad, setCantidad] = useState(1);
  
  const total = precio * cantidad;

  return (
    <div>
      <input 
        type="number" 
        value={precio}
        onChange={(e) => setPrecio(Number(e.target.value))}
        placeholder="Precio"
      />
      <input 
        type="number" 
        value={cantidad}
        onChange={(e) => setCantidad(Number(e.target.value))}
        placeholder="Cantidad"
      />
      <h3>Total: ${total}</h3>
    </div>
  );
}
```

### 2. Verdadero/Falso (Boolean)
```jsx
function Interruptor() {
  const [encendido, setEncendido] = useState(false);

  return (
    <div>
      <button onClick={() => setEncendido(!encendido)}>
        {encendido ? 'Apagar' : 'Encender'}
      </button>
      <p>La luz está {encendido ? 'encendida' : 'apagada'}</p>
    </div>
  );
}
```

### 3. Listas (Arrays)
```jsx
function ListaTareas() {
  const [tareas, setTareas] = useState([]);
  const [nuevaTarea, setNuevaTarea] = useState('');

  const agregarTarea = () => {
    if (nuevaTarea.trim()) {
      setTareas([...tareas, nuevaTarea]);
      setNuevaTarea('');
    }
  };

  return (
    <div>
      <input 
        value={nuevaTarea}
        onChange={(e) => setNuevaTarea(e.target.value)}
        placeholder="Nueva tarea"
      />
      <button onClick={agregarTarea}>Agregar</button>
      
      <ul>
        {tareas.map((tarea, index) => (
          <li key={index}>{tarea}</li>
        ))}
      </ul>
    </div>
  );
}
```

**Punto importante:** `[...tareas, nuevaTarea]` crea una nueva lista con todos los elementos anteriores más el nuevo. ¡Nunca modifiques la lista directamente!

### 4. Objetos
```jsx
function PerfilUsuario() {
  const [usuario, setUsuario] = useState({
    nombre: '',
    edad: 0,
    email: ''
  });

  const actualizarNombre = (nuevoNombre) => {
    setUsuario({
      ...usuario,  // Mantener todo lo anterior
      nombre: nuevoNombre  // Cambiar solo el nombre
    });
  };

  return (
    <div>
      <input 
        value={usuario.nombre}
        onChange={(e) => actualizarNombre(e.target.value)}
        placeholder="Nombre"
      />
      <p>Hola {usuario.nombre}</p>
    </div>
  );
}
```

## Reglas Importantes

### ❌ NO hagas esto:
```jsx
// MAL - Modificar directamente
const [lista, setLista] = useState([1, 2, 3]);
lista.push(4);  // ¡NO!
setLista(lista);

// MAL - Modificar objeto directamente
const [persona, setPersona] = useState({nombre: 'Juan'});
persona.nombre = 'Pedro';  // ¡NO!
setPersona(persona);
```

### ✅ SÍ haz esto:
```jsx
// BIEN - Crear nueva lista
const [lista, setLista] = useState([1, 2, 3]);
setLista([...lista, 4]);

// BIEN - Crear nuevo objeto
const [persona, setPersona] = useState({nombre: 'Juan'});
setPersona({...persona, nombre: 'Pedro'});
```

## Ejemplo Práctico: Formulario Simple

```jsx
function FormularioContacto() {
  const [datos, setDatos] = useState({
    nombre: '',
    email: '',
    mensaje: ''
  });
  const [enviado, setEnviado] = useState(false);

  const manejarCambio = (campo, valor) => {
    setDatos({
      ...datos,
      [campo]: valor
    });
  };

  const enviarFormulario = () => {
    console.log('Datos enviados:', datos);
    setEnviado(true);
  };

  if (enviado) {
    return <h2>¡Mensaje enviado! Gracias {datos.nombre}</h2>;
  }

  return (
    <div>
      <h2>Contáctanos</h2>
      
      <input 
        type="text"
        value={datos.nombre}
        onChange={(e) => manejarCambio('nombre', e.target.value)}
        placeholder="Tu nombre"
      />
      
      <input 
        type="email"
        value={datos.email}
        onChange={(e) => manejarCambio('email', e.target.value)}
        placeholder="Tu email"
      />
      
      <textarea 
        value={datos.mensaje}
        onChange={(e) => manejarCambio('mensaje', e.target.value)}
        placeholder="Tu mensaje"
      />
      
      <button onClick={enviarFormulario}>
        Enviar
      </button>
    </div>
  );
}
```

## Consejos Prácticos

### 1. Múltiples Estados
Puedes tener varios estados en un componente:

```jsx
function MiComponente() {
  const [nombre, setNombre] = useState('');
  const [edad, setEdad] = useState(0);
  const [activo, setActivo] = useState(false);
  
  // ... resto del componente
}
```

### 2. Estado Inicial Calculado
Si el valor inicial requiere cálculo, usa una función:

```jsx
const [datos, setDatos] = useState(() => {
  // Esto solo se ejecuta una vez
  return JSON.parse(localStorage.getItem('datos')) || [];
});
```

### 3. Actualización Basada en Estado Anterior
Cuando el nuevo valor depende del anterior, usa una función:

```jsx
const [contador, setContador] = useState(0);

// Mejor práctica
const incrementar = () => {
  setContador(prevContador => prevContador + 1);
};
```

