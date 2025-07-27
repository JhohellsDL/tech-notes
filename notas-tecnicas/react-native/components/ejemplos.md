# Ejemplos útiles en React Native

## Botón personalizado

```jsx
import { TouchableOpacity, Text } from 'react-native';

export default function MiBoton({ onPress, label }) {
  return (
    <TouchableOpacity onPress={onPress} style={{ backgroundColor: 'orange', padding: 10 }}>
      <Text style={{ color: 'white' }}>{label}</Text>
    </TouchableOpacity>
  );
}
```

## Llamada a API

```jsx
import { useEffect, useState } from 'react';
import { View, Text } from 'react-native';

export default function Usuario() {
  const [nombre, setNombre] = useState('');

  useEffect(() => {
    fetch('https://api.example.com/user')
      .then(res => res.json())
      .then(data => setNombre(data.nombre));
  }, []);

  return (
    <View>
      <Text>Hola, {nombre}</Text>
    </View>
  );
}
```
