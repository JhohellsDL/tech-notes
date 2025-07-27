# Estilos en React Native

React Native utiliza un sistema de estilos similar a CSS, pero basado en JavaScript.

## Ejemplo de uso de StyleSheet

```jsx
import { StyleSheet, View, Text } from 'react-native';

const styles = StyleSheet.create({
  contenedor: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  texto: {
    color: 'blue',
    fontSize: 20,
  },
});

export default function App() {
  return (
    <View style={styles.contenedor}>
      <Text style={styles.texto}>Hola React Native</Text>
    </View>
  );
}
```

## Tips

- Usa StyleSheet para mejorar el rendimiento.
- Prefiere nombres descriptivos en los estilos.
