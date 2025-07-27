# Navegación en React Native

Para la navegación entre pantallas se suele usar React Navigation.

## Instalación

```bash
npm install @react-navigation/native
npm install @react-navigation/native-stack
```

## Ejemplo básico

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Inicio" component={InicioScreen} />
        <Stack.Screen name="Perfil" component={PerfilScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

## Consejos

- Mantén las rutas simples.
- Centraliza la configuración de navegación.
