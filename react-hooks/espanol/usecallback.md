# useCallback

_**useCallback**_ se utiliza para retornar una función memoizada. A diferencia de _**useMemo**_ que memoiza  un valor, este hook memoiza una función como tal.

Para saber cuándo y cómo usar este hook, debemos entender qué problema busca solucionar.

### Igualdad de funciones

Notemos la siguiente función en javascript.

```javascript
function factory() {
  return (a, b) => a + b;
}
```

Esta función retorna una nueva función que recibe 2 parámetros y devuelve el cálculo de dichos parámetros.

Ahora ejecutemos _**factory**_ y veamos los resultados.

```javascript
const sum1 = factory();
const sum2 = factory();

sum1(1, 2); // retorna 3
sum2(1, 2); // retorna 3
```

Todo normal, comportamiento esperado de acuerdo con lo que explicamos previamente. 

Pero, ¿qué pasa si hacemos una revisión de igualdad de _**sum1**_ con _**sum2**_ ?

```javascript
sum1 === sum2; // => false
sum1 === sum1; // => true
```

si _**sum1**_ y _**sum2**_ ejecutan el mismo proceso y son productos de la misma función _**factory**_, ¿por qué la revisión de igualdad da falso? 

En JS las funciones también son objetos. Las funciones, al igual que muchos otros tipos de dato, provienen del prototipo Object \(también conocido como la clase Object\) por lo tanto, podemos decir que _**sum1**_ y _**sum2**_ en realidad son 2 objetos completamente diferentes y por eso la revisión de igualdad nos arroja falso.

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

Respecto al código anterior es acertado decir que, **cada vez que MyComponent se renderiza handleClick será un nuevo objeto function.**

Esto no es un problema en sí, las arrow functions no generan un impacto grande en términos de memoria. Si necesitamos unas cuantas funciones similares en forma a handleClick, lo más probable es que no nos veamos afectados por el hecho de que se crean nuevas instancias de las funciones cada vez que el componente se renderiza.

Sin embargo, en algunos escenarios es necesario u óptimo, mantener la instancia de una función a través del ciclo de vida un componente, en otras palabras, utilizar la misma instancia de la función para cada renderización del componente. Y es en este escenario en el que podemos utilizar el hook _**useCallback**_

```javascript
useCallback(callbackFun, deps)
```

_**deps**_ es un arreglo de dependencias \(las dependencias son variables comunes y corrientes\), cada vez que se reciban las mismas dependencias, useCallback retornará la instancia de la función memoizada para dichas dependencias, de esta manera es posible mantener la instancia de la función entre diferentes renderizaciones del componente.

### Cuándo sí usar useCallbak

Piensa en el siguiente caso de uso. Tienes una gran lista de registros que deseas renderizar en un componente llamado MyBigList.

```javascript
import React from 'react';

function MyBigList({ term, onItemClick }) {
 // código para obtener los items desde una API

  const map = item => <div onClick={onItemClick}>{item}</div>;

  return <div>{items.map(map)}</div>;
}

export default React.memo(MyBigList);
```

Para este código queremos enfocarnos principalmente, en la línea 6 y 11. Como puedes ver, para cada item estaremos utilizando un handler para manipular el evento click de cada elemento de la lista de registros. Finalmente usamos **React.memo** para optimizar la renderización de nuestra lista ya que tiene muchos items

Ahora vamos a incluir este componente en un componente padre

```javascript
import React, { useCallback } from 'react';

export default function MyParent({ term }) {
  const onItemClick = useCallback(event => {
    console.log('You clicked ', event.currentTarget);
  }, [term]);

  return (
    <MyBigList
      term={term}
      onItemClick={onItemClick}
    />
  );
}
```

Como puedes observar, _**onItemClick**_ es una función memoizada mediante _**useCallback, cada vez que la prop term tenga el mismo valor entre renderizaciones, onItemClick será una instancia existente de la función, no se creará una instancia nueva.**_ 

**¿Por qué es esto cool? Recuerda que nuestra lista MyBigList utiliza React.memo, por lo tanto si queremos optimizar nuestra renderización, es fundamental que las props no cambien innecesariamente a través del ciclo de vida de la app, useCallback logra precisamente ese propósito, nuestra función onItemClick no cambiará en tanto la propiedad term no cambie y voulá, estamos usando useCallback de la forma adecuada, logrando que la renderización de nuestra lista sea eficiente.**

Cabe señalara que este enfoque no es siempre el mejor, por ejemplo si no es una lista tan extensa, si los datos cambian con mucha frecuencia, tal vez no sea tan aplicable la memoización.

 _****_

