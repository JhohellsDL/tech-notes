# Componentes en React Native

Los componentes son la unidad básica de construcción en React Native.

## Componente funcional básico

```jsx
import React from 'react';
import { View, Text } from 'react-native';

export default function Saludo() {
  return (
    <View>
      <Text>¡Hola, mundo!</Text>
    </View>
  );
}
```

## Props y estado

```jsx
function Contador({ inicial }) {
  const [count, setCount] = React.useState(inicial);

  return (
    <View>
      <Text>Contador: {count}</Text>
      <Button title="Sumar" onPress={() => setCount(count + 1)} />
    </View>
  );
}
```

## Buenas prácticas

- Utiliza componentes pequeños y reutilizables.
- Separa lógica y presentación.
