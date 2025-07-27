
# Configuración de Axios – `src/api/axios.ts`

El archivo `src/api/axios.ts` es un componente esencial en la arquitectura de tu aplicación. Actúa como un punto centralizado para configurar y gestionar todas las interacciones HTTP con tu API utilizando Axios.

## ¿Qué es `src/api/axios.ts`?

Es un módulo que exporta una instancia personalizada de Axios (`axiosInstance`), preconfigurada con ajustes comunes como la URL base, cabeceras predeterminadas e interceptores. En lugar de usar Axios directamente en cada módulo, puedes importar esta instancia y mantener una configuración homogénea en toda la aplicación.

---

## ¿Por qué centralizar la configuración?

Centralizar la configuración de Axios aporta múltiples beneficios:

### 1. `baseURL` (URL base)

- **Propósito:** Define el prefijo común para todas las rutas de la API.
- **Ejemplo:** Si tus endpoints son `https://api.example.com/v1/users` y `https://api.example.com/v1/products`, la base puede ser `https://api.example.com/v1`, permitiendo que las rutas se simplifiquen a `/users` o `/products`.
- **Ventajas:**
  - Facilita el cambio entre entornos (desarrollo, staging, producción).
  - Reduce redundancia y errores al escribir URLs.
- **Implementación:** Se configura con la constante `API_BASE_URL` definida en `src/constants/api.ts`.

```ts
// src/constants/api.ts
export const API_BASE_URL = 'https://api.example.com/v1';
```

---

### 2. `headers` (Cabeceras por defecto)

- **Propósito:** Establece cabeceras comunes a todas las peticiones.
- **Ejemplo:**
  - `Content-Type: application/json` para peticiones JSON.
  - `Authorization: Bearer <token>` para autenticación con tokens.
- **Ventajas:**
  - Asegura consistencia en las peticiones.
  - Elimina la necesidad de repetir configuraciones en cada solicitud.

---

### 3. `interceptors` (Interceptores de peticiones y respuestas)

- **Propósito:** Permiten ejecutar lógica antes de enviar una petición o al recibir una respuesta.
- **Tipos:**
  - **Interceptores de Petición:**
    - Usados para inyectar dinámicamente tokens de autenticación en la cabecera `Authorization`.

  - **Interceptores de Respuesta:**
    - Manejo global de errores (como redirección al login en un `401 Unauthorized`).
    - Transformación de respuestas o registro de errores.

```ts
// src/api/axios.ts
import axios from 'axios';
import { API_BASE_URL } from '../constants/api';

const axiosInstance = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptor de petición
axiosInstance.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('authToken');
    if (token) {
      config.headers['Authorization'] = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Interceptor de respuesta
axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Redirigir al login o refrescar token
      console.warn('No autorizado. Redirigiendo al login...');
    }
    return Promise.reject(error);
  }
);

export default axiosInstance;
```

---

## Beneficios clave de usar un archivo `axios.ts` dedicado

✅ **Centralización:** Toda la configuración de Axios en un solo lugar.  
✅ **Reusabilidad:** Instancia lista para usarse en cualquier módulo de la app.  
✅ **Escalabilidad:** Cambios como modificar headers o lógica de autenticación solo se realizan una vez.  
✅ **Consistencia:** Asegura que todas las peticiones sigan las mismas reglas.  
✅ **Mantenibilidad:** Agiliza el debugging y evolución del código.

---

## Conclusión

El archivo `src/api/axios.ts` actúa como una puerta de entrada robusta y centralizada hacia tu backend. Configurar Axios de esta manera garantiza peticiones HTTP seguras, uniformes y fáciles de mantener, contribuyendo directamente a la escalabilidad y calidad del proyecto.
