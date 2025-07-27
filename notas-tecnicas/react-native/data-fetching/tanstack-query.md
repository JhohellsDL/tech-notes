
# Introducción a TanStack Query (`@tanstack/react-query`)

TanStack Query es una poderosa librería para el manejo de **estado de servidor** en aplicaciones JavaScript/TypeScript, especialmente popular en aplicaciones React. Proporciona una forma declarativa, eficiente y escalable de manejar peticiones HTTP, almacenamiento en caché, sincronización de datos y estados de carga o error.

---

## ¿Por qué usar TanStack Query?

✅ **Manejo automático de caché**  
✅ **Actualización y sincronización eficiente de datos**  
✅ **Control detallado de estados de carga, error y éxito**  
✅ **Integración perfecta con React (hooks)**  
✅ **Soporte para paginación, polling y revalidación en segundo plano**

---

## Instalación

```bash
npm install @tanstack/react-query
```

---

## Configuración inicial

TanStack Query requiere un `QueryClient` para manejar la configuración global de las queries. Este cliente debe ser proporcionado a tu aplicación mediante el componente `QueryClientProvider`.

```tsx
// src/main.tsx o src/index.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

---

## Uso básico – `useQuery`

El hook `useQuery` se utiliza para realizar peticiones y manejar el estado de los datos. En este ejemplo, se obtiene una lista de usuarios desde una API.

```tsx
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

// Función para obtener los datos de la API
const fetchUsers = async () => {
  const { data } = await axios.get('https://api.example.com/users');
  return data;
};

export const UsersList = () => {
  // useQuery maneja automáticamente el estado de carga, error y éxito
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'], // Clave única para identificar la query
    queryFn: fetchUsers, // Función que realiza la petición
  });

  if (isLoading) return <p>Cargando usuarios...</p>;
  if (error) return <p>Error al cargar usuarios</p>;

  return (
    <ul>
      {data.map((user: any) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};
```

---

## Mutaciones – `useMutation`

El hook `useMutation` se utiliza para realizar operaciones que modifican datos, como crear, actualizar o eliminar recursos. Este ejemplo muestra cómo crear un nuevo usuario y actualizar la lista automáticamente.

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query';
import axios from 'axios';

// Función para crear un nuevo usuario
const createUser = async (newUser: { name: string }) => {
  const { data } = await axios.post('https://api.example.com/users', newUser);
  return data;
};

export const CreateUserForm = () => {
  const queryClient = useQueryClient();

  // Configuración de la mutación
  const mutation = useMutation({
    mutationFn: createUser, // Función que realiza la mutación
    onSuccess: () => {
      // Invalida la query para refrescar los datos
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });

  const handleSubmit = () => {
    mutation.mutate({ name: 'Nuevo usuario' });
  };

  return (
    <button onClick={handleSubmit}>Crear Usuario</button>
  );
};
```

---

## Herramientas útiles

### `ReactQueryDevtools`

Esta herramienta proporciona una interfaz visual para inspeccionar y depurar las queries y mutaciones en tu aplicación.

```bash
npm install @tanstack/react-query-devtools
```

```tsx
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

<QueryClientProvider client={queryClient}>
  <App />
  <ReactQueryDevtools initialIsOpen={false} />
</QueryClientProvider>
```

---

## Buenas prácticas

- Usa `queryKey`s descriptivas y únicas para evitar conflictos entre queries.
- Configura `enabled: false` en queries que dependan de condiciones externas para evitar ejecuciones innecesarias.
- Utiliza `invalidateQueries` después de mutaciones para mantener los datos sincronizados.
- Ajusta `staleTime` y `cacheTime` según las necesidades de tu aplicación para optimizar el rendimiento.

---

## Ejemplo completo

A continuación, se muestra un ejemplo completo que combina el uso de `useQuery` y `useMutation` para manejar una lista de usuarios y agregar nuevos usuarios.

```tsx
import React from 'react';
import { QueryClient, QueryClientProvider, useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import axios from 'axios';

const queryClient = new QueryClient();

const fetchUsers = async () => {
  const { data } = await axios.get('https://api.example.com/users');
  return data;
};

const createUser = async (newUser: { name: string }) => {
  const { data } = await axios.post('https://api.example.com/users', newUser);
  return data;
};

const UsersList = () => {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
  });

  if (isLoading) return <p>Cargando usuarios...</p>;
  if (error) return <p>Error al cargar usuarios</p>;

  return (
    <ul>
      {data.map((user: any) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

const CreateUserForm = () => {
  const queryClient = useQueryClient();
  const mutation = useMutation({
    mutationFn: createUser,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });

  const handleSubmit = () => {
    mutation.mutate({ name: 'Nuevo usuario' });
  };

  return <button onClick={handleSubmit}>Crear Usuario</button>;
};

const App = () => (
  <div>
    <h1>Gestión de Usuarios</h1>
    <UsersList />
    <CreateUserForm />
  </div>
);

export default function Root() {
  return (
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  );
}
```

---

## Conclusión

TanStack Query es una solución robusta y moderna para gestionar el estado del servidor. Su enfoque declarativo, junto a su integración con React, lo convierte en una herramienta indispensable para aplicaciones que consumen APIs. Con sus herramientas avanzadas y buenas prácticas, puedes construir aplicaciones más eficientes y fáciles de mantener.
