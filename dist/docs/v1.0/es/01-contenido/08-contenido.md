---
title: ""Clase 8 - Hook useEffect - Guía Simple y Detallada"
position: 8
date: 2025-11-18
---

## ¿Qué es useEffect?

El hook `useEffect` te permite realizar **efectos secundarios** en tus componentes funcionales. Piensa en los efectos como "cosas que necesitas hacer" además de mostrar la interfaz:

- Obtener datos de una API
- Cambiar el título de la página
- Configurar temporizadores
- Limpiar recursos cuando el componente se desmonta

### Concepto Simple
```jsx
useEffect(() => {
  // Código que se ejecuta después del render
});
```

## Importación

Siempre debemos importar useEffect junto con useState:

```jsx
import { useState, useEffect } from 'react';
```

## Ejemplo 1: Mi Primer Efecto

Comencemos con algo simple - cambiar el título de la página:

```jsx
import { useState, useEffect } from 'react';

function CambiadorTitulo() {
  const [contador, setContador] = useState(0);

  // Este efecto se ejecuta después de cada render
  useEffect(() => {
    document.title = `Contador: ${contador}`;
  });

  return (
    <div>
      <h2>Contador: {contador}</h2>
      <button onClick={() => setContador(contador + 1)}>
        Incrementar
      </button>
    </div>
  );
}
```

**¿Qué pasa aquí?**
1. Cada vez que el componente se renderiza, `useEffect` se ejecuta
2. El efecto cambia el título de la página del navegador
3. Cuando incrementamos el contador, se actualiza el título automáticamente

## Ejemplo 2: Efecto que se Ejecuta Solo Una Vez

A veces queremos que algo pase solo cuando el componente aparece por primera vez:

```jsx
function SaludoInicial() {
  const [usuario, setUsuario] = useState('');

  // Este efecto se ejecuta SOLO una vez, al montar el componente
  useEffect(() => {
    console.log('¡El componente se montó!');
    alert('¡Bienvenido a la aplicación!');
  }, []); // El array vacío [] es la clave

  return (
    <div>
      <input 
        value={usuario}
        onChange={(e) => setUsuario(e.target.value)}
        placeholder="Tu nombre"
      />
      <p>Hola {usuario}</p>
    </div>
  );
}
```

**Punto clave:** El `[]` al final hace que el efecto se ejecute solo una vez.

## Los Tres Tipos de useEffect

### 1. Sin Dependencias (se ejecuta siempre)
```jsx
useEffect(() => {
  console.log('Me ejecuto después de cada render');
});
```

### 2. Con Array Vacío (se ejecuta solo una vez)
```jsx
useEffect(() => {
  console.log('Me ejecuto solo al montar el componente');
}, []);
```

### 3. Con Dependencias (se ejecuta cuando cambian)
```jsx
useEffect(() => {
  console.log('Me ejecuto cuando cambia el contador');
}, [contador]);
```

## Ejemplo 3: Obtener Datos de una API

Este es uno de los usos más comunes de useEffect:

```jsx
function ListaUsuarios() {
  const [usuarios, setUsuarios] = useState([]);
  const [cargando, setCargando] = useState(true);

  useEffect(() => {
    // Simular llamada a API
    const obtenerUsuarios = async () => {
      setCargando(true);
      
      // Simular delay de red
      await new Promise(resolve => setTimeout(resolve, 2000));
      
      // Datos simulados
      const datosSimulados = [
        { id: 1, nombre: 'Ana García', email: 'ana@email.com' },
        { id: 2, nombre: 'Carlos López', email: 'carlos@email.com' },
        { id: 3, nombre: 'María Rodríguez', email: 'maria@email.com' }
      ];
      
      setUsuarios(datosSimulados);
      setCargando(false);
    };

    obtenerUsuarios();
  }, []); // Solo se ejecuta una vez al montar

  if (cargando) {
    return <div>Cargando usuarios...</div>;
  }

  return (
    <div>
      <h2>Lista de Usuarios</h2>
      <ul>
        {usuarios.map(usuario => (
          <li key={usuario.id}>
            {usuario.nombre} - {usuario.email}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## Ejemplo 4: Efecto con Dependencias

Veamos cómo reaccionar a cambios específicos:

```jsx
function BuscadorProductos() {
  const [busqueda, setBusqueda] = useState('');
  const [productos, setProductos] = useState([]);
  const [cargando, setCargando] = useState(false);

  // Este efecto se ejecuta cada vez que cambia 'busqueda'
  useEffect(() => {
    if (busqueda.trim() === '') {
      setProductos([]);
      return;
    }

    const buscarProductos = async () => {
      setCargando(true);
      
      // Simular búsqueda
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      const todosLosProductos = [
        'Laptop', 'Mouse', 'Teclado', 'Monitor', 'Auriculares',
        'Cámara', 'Tablet', 'Smartphone', 'Impresora', 'Router'
      ];
      
      const resultados = todosLosProductos.filter(producto => 
        producto.toLowerCase().includes(busqueda.toLowerCase())
      );
      
      setProductos(resultados);
      setCargando(false);
    };

    buscarProductos();
  }, [busqueda]); // Se ejecuta cuando cambia 'busqueda'

  return (
    <div>
      <h2>Buscador de Productos</h2>
      <input 
        type="text"
        value={busqueda}
        onChange={(e) => setBusqueda(e.target.value)}
        placeholder="Buscar productos..."
      />
      
      {cargando && <p>Buscando...</p>}
      
      <ul>
        {productos.map((producto, index) => (
          <li key={index}>{producto}</li>
        ))}
      </ul>
      
      {busqueda && !cargando && productos.length === 0 && (
        <p>No se encontraron productos</p>
      )}
    </div>
  );
}
```

## Limpieza de Efectos (Cleanup)

Algunos efectos necesitan "limpiarse" cuando el componente se desmonta:

```jsx
function Temporizador() {
  const [segundos, setSegundos] = useState(0);
  const [activo, setActivo] = useState(false);

  useEffect(() => {
    let intervalo = null;
    
    if (activo) {
      intervalo = setInterval(() => {
        setSegundos(segundos => segundos + 1);
      }, 1000);
    }

    // Función de limpieza
    return () => {
      if (intervalo) {
        clearInterval(intervalo);
      }
    };
  }, [activo]); // Se ejecuta cuando cambia 'activo'

  const reiniciar = () => {
    setSegundos(0);
    setActivo(false);
  };

  return (
    <div>
      <h2>Temporizador: {segundos} segundos</h2>
      <button onClick={() => setActivo(!activo)}>
        {activo ? 'Pausar' : 'Iniciar'}
      </button>
      <button onClick={reiniciar}>
        Reiniciar
      </button>
    </div>
  );
}
```

**Punto importante:** La función que devuelves en useEffect se ejecuta para limpiar el efecto.

## Ejemplo Práctico: Chat en Tiempo Real (Simulado)

```jsx
function ChatSimulado() {
  const [mensajes, setMensajes] = useState([]);
  const [nuevoMensaje, setNuevoMensaje] = useState('');
  const [conectado, setConectado] = useState(false);

  // Efecto para simular conexión al chat
  useEffect(() => {
    if (conectado) {
      console.log('Conectando al chat...');
      
      // Simular mensajes que llegan
      const intervalo = setInterval(() => {
        const mensajesAleatorios = [
          'Hola, ¿cómo están?',
          '¿Alguien sabe de React?',
          'Estoy aprendiendo useEffect',
          '¡Este tutorial está genial!',
          '¿Dudas sobre hooks?'
        ];
        
        const mensajeAleatorio = mensajesAleatorios[
          Math.floor(Math.random() * mensajesAleatorios.length)
        ];
        
        setMensajes(prev => [...prev, {
          id: Date.now(),
          texto: mensajeAleatorio,
          usuario: 'Usuario' + Math.floor(Math.random() * 100),
          timestamp: new Date().toLocaleTimeString()
        }]);
      }, 3000);

      // Limpieza: desconectar cuando el componente se desmonte
      return () => {
        console.log('Desconectando del chat...');
        clearInterval(intervalo);
      };
    }
  }, [conectado]);

  const enviarMensaje = () => {
    if (nuevoMensaje.trim()) {
      setMensajes(prev => [...prev, {
        id: Date.now(),
        texto: nuevoMensaje,
        usuario: 'Tú',
        timestamp: new Date().toLocaleTimeString()
      }]);
      setNuevoMensaje('');
    }
  };

  return (
    <div style={{ maxWidth: '400px', margin: '0 auto', padding: '20px' }}>
      <h2>Chat Simulado</h2>
      
      <button 
        onClick={() => setConectado(!conectado)}
        style={{
          backgroundColor: conectado ? '#dc3545' : '#28a745',
          color: 'white',
          border: 'none',
          padding: '10px 20px',
          borderRadius: '4px',
          marginBottom: '20px'
        }}
      >
        {conectado ? 'Desconectar' : 'Conectar'}
      </button>
      
      <div style={{
        height: '300px',
        border: '1px solid #ddd',
        borderRadius: '4px',
        padding: '10px',
        overflowY: 'scroll',
        backgroundColor: '#f8f9fa',
        marginBottom: '10px'
      }}>
        {mensajes.map(mensaje => (
          <div key={mensaje.id} style={{ marginBottom: '10px' }}>
            <strong>{mensaje.usuario}</strong> 
            <small style={{ color: '#666' }}> ({mensaje.timestamp})</small>
            <br />
            {mensaje.texto}
          </div>
        ))}
        {mensajes.length === 0 && (
          <p style={{ color: '#666', textAlign: 'center' }}>
            {conectado ? 'Esperando mensajes...' : 'Desconectado'}
          </p>
        )}
      </div>
      
      {conectado && (
        <div style={{ display: 'flex' }}>
          <input 
            type="text"
            value={nuevoMensaje}
            onChange={(e) => setNuevoMensaje(e.target.value)}
            onKeyPress={(e) => e.key === 'Enter' && enviarMensaje()}
            placeholder="Escribe un mensaje..."
            style={{
              flex: 1,
              padding: '10px',
              border: '1px solid #ddd',
              borderRadius: '4px 0 0 4px'
            }}
          />
          <button 
            onClick={enviarMensaje}
            style={{
              backgroundColor: '#007bff',
              color: 'white',
              border: 'none',
              padding: '10px 20px',
              borderRadius: '0 4px 4px 0'
            }}
          >
            Enviar
          </button>
        </div>
      )}
    </div>
  );
}
```

## Reglas Importantes de useEffect

### ✅ SÍ haz esto:
```jsx
// Especifica dependencias correctamente
useEffect(() => {
  console.log(contador);
}, [contador]);

// Limpia efectos cuando sea necesario
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
  return () => clearInterval(timer);
}, []);
```

### ❌ NO hagas esto:
```jsx
// MAL - Olvidar dependencias
useEffect(() => {
  console.log(contador); // Usa 'contador' pero no está en dependencias
}, []); // ¡Falta [contador]!

// MAL - No limpiar efectos
useEffect(() => {
  setInterval(() => {}, 1000); // ¡No se limpia!
}, []);
```

## Casos de Uso Comunes

### 1. Obtener Datos al Cargar
```jsx
useEffect(() => {
  fetch('/api/datos')
    .then(response => response.json())
    .then(data => setDatos(data));
}, []);
```

### 2. Escuchar Eventos del Navegador
```jsx
useEffect(() => {
  const manejarResize = () => {
    console.log('Ventana redimensionada');
  };
  
  window.addEventListener('resize', manejarResize);
  
  return () => {
    window.removeEventListener('resize', manejarResize);
  };
}, []);
```

### 3. Actualizar Documento
```jsx
useEffect(() => {
  document.title = `${nombre} - Mi App`;
}, [nombre]);
```

## Ejercicios para Practicar

### Ejercicio 1: Reloj Digital
Crea un reloj que:
- Muestre la hora actual
- Se actualice cada segundo
- Tenga botón para pausar/reanudar

### Ejercicio 2: Contador de Clics
Crea un componente que:
- Cuente clics en la página
- Guarde el conteo en localStorage
- Cargue el conteo al iniciar

### Ejercicio 3: Buscador con Debounce
Crea un buscador que:
- Espere 500ms después de escribir
- Luego haga la búsqueda
- Cancele búsquedas anteriores

## Errores Comunes y Soluciones

### Error 1: "Bucle Infinito"
```jsx
// MAL - Causa bucle infinito
const [datos, setDatos] = useState([]);
useEffect(() => {
  setDatos([...datos, 'nuevo']); // ¡Cambia 'datos' sin dependencias!
});

// BIEN - Especifica dependencias
useEffect(() => {
  // Solo ejecutar bajo ciertas condiciones
}, []);
```

### Error 2: "Dependencias Faltantes"
```jsx
// MAL - Usa 'usuario' pero no está en dependencias
useEffect(() => {
  console.log(usuario.nombre);
}, []); // ¡Falta [usuario]!

// BIEN - Incluye todas las dependencias
useEffect(() => {
  console.log(usuario.nombre);
}, [usuario]);
```

### Error 3: "No Limpiar Efectos"
```jsx
// MAL - No limpia el temporizador
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
}, []);

// BIEN - Limpia el temporizador
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
  return () => clearInterval(timer);
}, []);
```

## Consejos Avanzados

### 1. Múltiples useEffect
Puedes usar varios useEffect para diferentes propósitos:

```jsx
function MiComponente() {
  // Efecto para datos
  useEffect(() => {
    obtenerDatos();
  }, []);
  
  // Efecto para título
  useEffect(() => {
    document.title = titulo;
  }, [titulo]);
  
  // Efecto para temporizador
  useEffect(() => {
    const timer = setInterval(() => {}, 1000);
    return () => clearInterval(timer);
  }, []);
}
```

### 2. Efecto Condicional
```jsx
useEffect(() => {
  if (usuario && usuario.id) {
    obtenerPerfilUsuario(usuario.id);
  }
}, [usuario]);
```

### 3. Evitar Efectos Innecesarios
```jsx
// En lugar de esto:
useEffect(() => {
  setTotal(precio * cantidad);
}, [precio, cantidad]);

// Mejor hacer esto:
const total = precio * cantidad; // Cálculo directo
```

## Ejemplos con API Playground y TypeScript

Vamos a ver ejemplos prácticos usando la API Playground de Mockoon con TypeScript para gestionar usuarios.

### Tipos TypeScript para Usuarios

Primero, definamos tipos simples para trabajar con usuarios:

```tsx
// Tipos para el recurso users
type Usuario = {
  id: string;
  name: string;
  email: string;
  phone: string;
  address: string;
  birthdate: string;
  isActive: boolean;
};

// Tipo para estado de carga
type EstadoCarga = {
  cargando: boolean;
  error: string | null;
  completado: boolean;
};
```

### Ejemplo 1: Obtener Usuarios con useEffect

Este ejemplo muestra cómo obtener una lista de usuarios al cargar el componente:

```tsx
import { useState, useEffect } from 'react';

function ListaUsuarios() {
  const [usuarios, setUsuarios] = useState<Usuario[]>([]);
  const [estado, setEstado] = useState<EstadoCarga>({
    cargando: false,
    error: null,
    completado: false
  });

  useEffect(() => {
    // Función para obtener usuarios
    const obtenerUsuarios = async () => {
      setEstado({ cargando: true, error: null, completado: false });
      
      try {
        const respuesta = await fetch('https://playground.mockoon.com/users');
        
        if (!respuesta.ok) {
          throw new Error(`Error: ${respuesta.status}`);
        }
        
        const datos = await respuesta.json();
        setUsuarios(datos);
        setEstado({ cargando: false, error: null, completado: true });
      } catch (error) {
        console.error('Error al obtener usuarios:', error);
        setEstado({ 
          cargando: false, 
          error: error instanceof Error ? error.message : 'Error desconocido', 
          completado: true 
        });
      }
    };

    obtenerUsuarios();
  }, []); // Se ejecuta solo al montar el componente

  return (
    <div>
      <h2>Lista de Usuarios</h2>
      
      {estado.cargando && <p>Cargando usuarios...</p>}
      
      {estado.error && (
        <div style={{ color: 'red', padding: '10px', backgroundColor: '#ffeeee' }}>
          Error: {estado.error}
        </div>
      )}
      
      {estado.completado && !estado.error && usuarios.length === 0 && (
        <p>No se encontraron usuarios</p>
      )}
      
      <ul>
        {usuarios.map(usuario => (
          <li key={usuario.id} style={{ 
            padding: '10px', 
            margin: '5px 0',
            backgroundColor: usuario.isActive ? '#e6ffe6' : '#ffe6e6',
            borderRadius: '4px'
          }}>
            <strong>{usuario.name}</strong> ({usuario.email})
            <br />
            <span>Teléfono: {usuario.phone}</span>
            <span style={{ 
              marginLeft: '10px',
              color: usuario.isActive ? 'green' : 'red'
            }}>
              {usuario.isActive ? '● Activo' : '● Inactivo'}
            </span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Ejemplo 2: useEffect con Dependencias para Búsqueda

Este ejemplo muestra cómo buscar usuarios cuando cambia un término de búsqueda:

```tsx
import { useState, useEffect } from 'react';

function BuscadorUsuarios() {
  const [termino, setTermino] = useState('');
  const [usuarios, setUsuarios] = useState<Usuario[]>([]);
  const [estado, setEstado] = useState<EstadoCarga>({
    cargando: false,
    error: null,
    completado: false
  });

  // Este efecto se ejecuta cuando cambia el término de búsqueda
  useEffect(() => {
    // Si el término está vacío, no hacemos nada
    if (termino.trim() === '') {
      setUsuarios([]);
      setEstado({ cargando: false, error: null, completado: false });
      return;
    }

    // Función para buscar usuarios
    const buscarUsuarios = async () => {
      setEstado({ cargando: true, error: null, completado: false });
      
      try {
        // URL con parámetro de búsqueda
        const url = `https://playground.mockoon.com/users?q=${encodeURIComponent(termino)}`;
        const respuesta = await fetch(url);
        
        if (!respuesta.ok) {
          throw new Error(`Error: ${respuesta.status}`);
        }
        
        const datos = await respuesta.json();
        setUsuarios(datos);
        setEstado({ cargando: false, error: null, completado: true });
      } catch (error) {
        console.error('Error al buscar usuarios:', error);
        setEstado({ 
          cargando: false, 
          error: error instanceof Error ? error.message : 'Error desconocido', 
          completado: true 
        });
      }
    };

    // Debounce: esperamos 500ms después de escribir para buscar
    const timeoutId = setTimeout(() => {
      buscarUsuarios();
    }, 500);

    // Limpieza: cancelamos el timeout si el término cambia antes de los 500ms
    return () => clearTimeout(timeoutId);
  }, [termino]); // Se ejecuta cuando cambia el término

  return (
    <div>
      <h2>Buscador de Usuarios</h2>
      
      <input
        type="text"
        value={termino}
        onChange={(e) => setTermino(e.target.value)}
        placeholder="Buscar por nombre o email..."
        style={{
          padding: '10px',
          width: '100%',
          marginBottom: '20px',
          borderRadius: '4px',
          border: '1px solid #ccc'
        }}
      />
      
      {estado.cargando && <p>Buscando usuarios...</p>}
      
      {estado.error && (
        <div style={{ color: 'red', padding: '10px', backgroundColor: '#ffeeee' }}>
          Error: {estado.error}
        </div>
      )}
      
      {estado.completado && !estado.error && usuarios.length === 0 && (
        <p>No se encontraron usuarios que coincidan con "{termino}"</p>
      )}
      
      <ul style={{ padding: 0, listStyle: 'none' }}>
        {usuarios.map(usuario => (
          <li key={usuario.id} style={{ 
            padding: '10px', 
            margin: '5px 0',
            backgroundColor: '#f5f5f5',
            borderRadius: '4px',
            borderLeft: `4px solid ${usuario.isActive ? 'green' : 'red'}`
          }}>
            <strong>{usuario.name}</strong>
            <br />
            <span>{usuario.email}</span>
            <br />
            <span style={{ 
              display: 'inline-block',
              padding: '2px 6px',
              backgroundColor: '#f0f0f0',
              borderRadius: '4px',
              fontSize: '0.8em',
              marginTop: '5px'
            }}>
              {usuario.birthdate}
            </span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Ejemplo 3: Múltiples Efectos para Gestión de Usuarios

Este ejemplo muestra cómo usar múltiples efectos para diferentes aspectos de la gestión de usuarios:

```tsx
import { useState, useEffect } from 'react';

function GestorUsuarios() {
  const [usuarios, setUsuarios] = useState<Usuario[]>([]);
  const [usuarioSeleccionado, setUsuarioSeleccionado] = useState<string | null>(null);
  const [detalleUsuario, setDetalleUsuario] = useState<Usuario | null>(null);
  const [estadisticas, setEstadisticas] = useState({
    total: 0,
    activos: 0,
    inactivos: 0
  });
  const [estado, setEstado] = useState<EstadoCarga>({
    cargando: false,
    error: null,
    completado: false
  });

  // Efecto 1: Cargar lista de usuarios
  useEffect(() => {
    const cargarUsuarios = async () => {
      setEstado({ cargando: true, error: null, completado: false });
      
      try {
        const respuesta = await fetch('https://playground.mockoon.com/users');
        
        if (!respuesta.ok) {
          throw new Error(`Error: ${respuesta.status}`);
        }
        
        const datos = await respuesta.json();
        setUsuarios(datos);
        setEstado({ cargando: false, error: null, completado: true });
      } catch (error) {
        setEstado({ 
          cargando: false, 
          error: error instanceof Error ? error.message : 'Error desconocido', 
          completado: true 
        });
      }
    };

    cargarUsuarios();
  }, []); // Solo al montar

  // Efecto 2: Calcular estadísticas cuando cambia la lista de usuarios
  useEffect(() => {
    const activos = usuarios.filter(u => u.isActive).length;
    
    setEstadisticas({
      total: usuarios.length,
      activos: activos,
      inactivos: usuarios.length - activos
    });
  }, [usuarios]); // Se ejecuta cuando cambia la lista de usuarios

  // Efecto 3: Cargar detalles cuando se selecciona un usuario
  useEffect(() => {
    if (usuarioSeleccionado === null) {
      setDetalleUsuario(null);
      return;
    }

    const cargarDetalleUsuario = async () => {
      try {
        const respuesta = await fetch(`https://playground.mockoon.com/users/${usuarioSeleccionado}`);
        
        if (!respuesta.ok) {
          throw new Error(`Error: ${respuesta.status}`);
        }
        
        const datos = await respuesta.json();
        setDetalleUsuario(datos);
      } catch (error) {
        console.error('Error al cargar detalle:', error);
        setDetalleUsuario(null);
      }
    };

    cargarDetalleUsuario();
  }, [usuarioSeleccionado]); // Se ejecuta cuando cambia el usuario seleccionado

  return (
    <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
      <h2>Gestor de Usuarios</h2>
      
      {/* Panel de estadísticas */}
      <div style={{ 
        display: 'flex', 
        justifyContent: 'space-around',
        padding: '15px',
        backgroundColor: '#f0f0f0',
        borderRadius: '8px'
      }}>
        <div>
          <strong>Total:</strong> {estadisticas.total}
        </div>
        <div style={{ color: 'green' }}>
          <strong>Activos:</strong> {estadisticas.activos}
        </div>
        <div style={{ color: 'red' }}>
          <strong>Inactivos:</strong> {estadisticas.inactivos}
        </div>
      </div>
      
      {/* Lista de usuarios */}
      <div style={{ display: 'flex', gap: '20px' }}>
        <div style={{ flex: 1 }}>
          <h3>Lista de Usuarios</h3>
          
          {estado.cargando && <p>Cargando usuarios...</p>}
          
          {estado.error && (
            <div style={{ color: 'red', padding: '10px', backgroundColor: '#ffeeee' }}>
              Error: {estado.error}
            </div>
          )}
          
          <ul style={{ padding: 0, listStyle: 'none' }}>
            {usuarios.map(usuario => (
              <li 
                key={usuario.id} 
                onClick={() => setUsuarioSeleccionado(usuario.id)}
                style={{ 
                  padding: '10px', 
                  margin: '5px 0',
                  backgroundColor: usuarioSeleccionado === usuario.id ? '#e6f7ff' : '#f5f5f5',
                  borderRadius: '4px',
                  cursor: 'pointer',
                  borderLeft: `4px solid ${usuario.isActive ? 'green' : 'red'}`
                }}
              >
                <strong>{usuario.name}</strong>
                <br />
                <small>{usuario.email}</small>
              </li>
            ))}
          </ul>
        </div>
        
        {/* Detalle de usuario */}
        <div style={{ 
          flex: 1, 
          padding: '15px', 
          backgroundColor: '#f9f9f9',
          borderRadius: '8px',
          minHeight: '200px'
        }}>
          <h3>Detalle de Usuario</h3>
          
          {!usuarioSeleccionado && (
            <p>Selecciona un usuario para ver sus detalles</p>
          )}
          
          {usuarioSeleccionado && !detalleUsuario && (
            <p>Cargando detalles...</p>
          )}
          
          {detalleUsuario && (
            <div>
              <h4>{detalleUsuario.name}</h4>
              <p><strong>Email:</strong> {detalleUsuario.email}</p>
              <p><strong>Teléfono:</strong> {detalleUsuario.phone}</p>
              <p><strong>Dirección:</strong> {detalleUsuario.address}</p>
              <p><strong>Fecha de nacimiento:</strong> {detalleUsuario.birthdate}</p>
              <p>
                <strong>Estado:</strong> 
                <span style={{ 
                  color: detalleUsuario.isActive ? 'green' : 'red',
                  marginLeft: '5px'
                }}>
                  {detalleUsuario.isActive ? 'Activo' : 'Inactivo'}
                </span>
              </p>
              <button
                onClick={() => setUsuarioSeleccionado(null)}
                style={{
                  padding: '8px 15px',
                  backgroundColor: '#f0f0f0',
                  border: 'none',
                  borderRadius: '4px',
                  cursor: 'pointer',
                  marginTop: '10px'
                }}
              >
                Cerrar detalle
              </button>
            </div>
          )}
        </div>
      </div>
    </div>
  );
}
```




