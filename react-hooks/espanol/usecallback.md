# useCallback

_**useCallback**_ se utiliza para devolver o retornar una función memoizada. A diferencia de _**useMemo**_ que memoiza  un valor, este hook memoiza una función como tal.

Para saber cuándo y como usar este hook, debemos entender qué problema busca solucionar.

### Igualdad de funciones

Notemos la siguiente función en javascript.

```javascript
function factory() {
  return (a, b) => a + b;
}
```

Esta función retorna una nueva función que recibe 2 parámetros y devuelve el cálculo de dichos parámetros.

Ahora ejecutemos esta función _**factory**_ y veamos los resultados.

```javascript
const sum1 = factory();
const sum2 = factory();

sum1(1, 2); // retorna 3
sum2(1, 2); // retorna 3
```

Todo normal, comportamiento esperado de acuerdo a lo que explicamos previamente. 

Pero, ¿qué pasa si hacemos una revisión de igualdad de _**sum1**_ con _**sum2**_ ?

```javascript
sum1 === sum2; // => false
sum1 === sum1; // => true
```

si _**sum1**_ y _**sum2**_ ejecutan el mismo proceso y son productos de la misma función _**factory**_, ¿por qué la revisión de igualdad da falso? 

En JS las funciones también son objetos, provienen del prototipo Object \(también conocido como la clase Object\) por lo tanto, podemos decir que _**sum1**_ y _**sum2**_ en realidad son 2 objetos completamente diferentes.

Ahora bien, escalemos este principio a React

### useCallback entra en escena

Notemos el siguiente componente

```javascript
import React from 'react';

function MyComponent() {
  const handleClick = () => {
    console.log('Clicked!');
  };

  // resto del código.
}
```

La función _**handleClick**_ 

