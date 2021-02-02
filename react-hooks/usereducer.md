# useReducer

This hook can be used to manage the state, is an alternative to useState hook. It is better to use useReducer when the state we want to manage is complex.

useReducer promotes inmutability and data will flow always in one direction.

To implement useReducer we need to follow these steps

### Creating reducers

```javascript
import _ from "lodash";

const initialState = {
    favorites: [],
};
const favoritesReducer = (state, action) => {
    switch (action.type) {
        case "ADD_TO_FAVS":
            return {
                ...state,
                favorites: [...state.favorites, action.payload],
            };
        case "REMOVE_FROM_FAVS":
            const newFavorites = _.pull(state.favorites, action.payload);
            return {
                ...state,
                favorites: newFavorites,
            };
        default:
            return state;
    }
};
```

### Calling useReducer inside the component

```javascript
const Cryptos = () => {
    const [state, dispatch] = useReducer(favoritesReducer, initialState);
    const addToFavs = (newFavorite) => {
        dispatch({ type: "ADD_TO_FAVS", payload: newFavorite });
    };
    const removeFromFavs = (favoriteToBeRemoved) => {
        dispatch({ type: "REMOVE_FROM_FAVS", payload: favoriteToBeRemoved });
    };
    const { favorites } = state
    // Returning component to be rendered
};
```

