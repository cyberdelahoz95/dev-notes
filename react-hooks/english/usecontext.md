# useContext

This hook enables the component \(function component\) to have access to the React Context, this helps to transfer props from parents to children without explicitly move the whole component tree.

To use this feature we need to follow these steps

### Creating a context

```javascript
import React from "react";

const toggleDarkModel = () => {};

const theme = {
	darkModeActive: false,
	toggleDarkModel,
};

const ThemeContext = React.createContext(theme);

export default ThemeContext;
```

toggleDarkModel is a function that later will be implemented and its purpose is manipulating the state by function components.

### Adding provider to the app

```jsx
import React, { useState } from "react";

import ThemeContext from "./context/ThemeContext";
// Other components

function App() {
	const [darkModeActive, setDarkModeActive] = useState(mode);
	const toggleDarkModel = () => {
		setDarkModeActive(!darkModeActive);
	};
	return (
		<ThemeContext.Provider value={{ toggleDarkModel, darkModeActive }}>
		//App goes here
		</ThemeContext.Provider>
	);
}

export default App;

```

As we can see, the function component has the implementation of toggleDarkModel

### Consuming our context wherever is needed

When we want to consume the context, we will need to import useContext

```jsx
import React, { useContext } from "react";
import ThemeContext from "../context/ThemeContext";

const Header = () => {
    const { toggleDarkModel, darkModeActive } = useContext(ThemeContext);
    return (
        <div className="Header container">
            <h1 className="title">Crypto Swag</h1>
            <button onClick={toggleDarkModel} className={`button is-small ${darkModeActive ? "is-white" : "is-dark"}`}>
                <span className="icon is-small">
                    <i className={`mdi ${darkModeActive ? "mdi-lightbulb-on" : "mdi-lightbulb-off"}`}></i>
                </span>
            </button>
        </div>
    );
};

export default Header;

```

