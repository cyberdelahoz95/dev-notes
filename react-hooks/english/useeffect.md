# useEffect

This hook allows us to call side functions, in other words, this can help us to execute functions or actions as a parallel process related to the component rendering.

Side effects may include calling APIs, changes in the context of the site, for example SEO by manipulating the document title or some meta attributes population, etc.

```jsx
import React, { useState, useEffect } from "react";

const Cryptos = ({ darkModeActive }) => {
    const [cryptos, setCryptos] = useState([]);

    //closure to be executed, this closure is on charge of performing http request.
    //second argument is a variable that is going to be.
    useEffect(() => {
        fetch("https://api.nomics.com/v1/currencies/ticker?key=123456789&per-page=50&page=1")
            .then((res) => res.json())
            .then((data) => setCryptos(data));
    }, []); // Only re-run the effect if second argument changes

  // return component to be rendered
};
export default Cryptos;
```

useEffect receives 2 parameters. First one is required, second one is optional.

First parameter is a closure that is going to be executed in order to perform the side effect, in this example the side effect is calling an API.

Second parameter is a "previous state", when  we set this parameter and the component is going to re-render React is going to compare the previous value with the new one, if they are equal React will not perform the side effect.

