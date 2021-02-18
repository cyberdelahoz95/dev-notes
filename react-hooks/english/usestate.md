# useState

This hook enables the functionality of managing the state in function components.

```jsx
import React, { useState } from 'react'

const Header = () => {
    //setting initial state up and getting state variable
    // and function that is going to manage it
    const [darkModeActive, setDarkModeActive] = useState(false)
    // This inner function makes use of the setDarkModeActive to set the new state.
    const handleClick = () => {
        setDarkModeActive(!darkModeActive)
    }
    return (
        <div className="Header">
            <h1>Crypto Swag</h1>
            <button 
                type="button" 
                onClick={
                    handleClick}>{darkModeActive ? 
                    'Light Mode' 
                    : 'Dark Mode'}
            </button>
        </div>
    )
}

export default Header
```

In the above example, Header is a component and we are using useState to control the darkMode of a website.

useState hook receives the initial state we want to control and then it returns an array with 2 elements. First element is the variable containing the current state and a second element is the function on charge of changing the state itself.

