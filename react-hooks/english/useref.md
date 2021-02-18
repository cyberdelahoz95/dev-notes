# useRef

useRef hook is used to establish a  connection between native html inputs and our component's logic.

This connection gives us access to the input attributes, for example the input value.

In the following example we are using useRef in order to get the value and set the state.

```jsx
import React, { useState, useEffect, useReducer, useMemo, useRef } from "react";

const Cryptos = () => {
    const searchInput = useRef(null);
    const [search, setSearch] = useState("");

    const handleSearch = () => setSearch(searchInput.current.value);
    
    return (
        <div>
            <input
                ref={searchInput}            
                type="text" 
                value={search} 
                onChange={handleSearch} 
            />
        </div>
    );
};
export default Cryptos;

```

