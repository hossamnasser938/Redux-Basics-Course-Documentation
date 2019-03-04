# A Better Project Structure
* Till now we successfully **connected** ` ReactJS ` and ` Redux ` together in a single application. We also divided our **components** into **containers** and **components** and that will help make our project more manageable, less complex, and readable.
* However, there is a problem that is not clear now since our project is really small and silly. The problem is that we put all ` Reducers `, and  ` Actions ` in a single file which is ` index.js `. In real projects we definitely have many ` Reducers ` and ` Actions `. So putting them all together in a single file certainly lead to **more complexity**, **less maintainability**, **less readability**, as well as **less manageability**. It will be good if we structure and divide this stuff and that is what we will do now.
    1. We create a directory under ` src/app ` named ` reducers ` which will hold our ` Reducers `. We create each ` Reducer ` in an individual file and then we will combine all of them in a single ` Reducer ` using the ` function ` ` combineReducers `. Since we have only a single ` Reducer ` in our previous example we will create a file in ` reducers ` directory named ` PreferencesReducer `.
    ```js
    const initialPreferences = {
        notifications: false
    }

    const preferencesReducer = ( state = initialPreferences, action ) => {
        let copiedState = { ...state };

        switch ( action.type ) {
            case "ENABLE_NOTIFICATIONS":
                copiedState.notifications = true;
                break;
            case "DISABLE_NOTIFICATIONS":
                copiedState.notifications = false;
                break;
        }
        return copiedState;
    };

    export default preferencesReducer;
    ```     
    Note that we **exported** our ` Reducer ` to be **imported** soon.

    2. In our ` src/app ` directory we create a file named ` Store ` to define our application ` Store `.
    ```js
    import { createStore, applyMiddleware } from 'redux';
    import { createLogger } from 'redux-logger';
    import preferencesReducer from './reducers/PreferencesReducer';

    const Store = createStore(
        preferencesReducer,
        {},
        applyMiddleware( createLogger() )
    );

    export default Store;
    ```
    Note that since we have only one ` Reducer ` in our project we use it directly. However, if we had many ` Reducers ` we will use ` combineReducers ` method to combine them in a single one.
    
    3. We create a directory under ` src/app ` named ` actions ` which will hold our ` Actions `. We create a file for each ` Reducer ` ` Actions `. Since we have only one ` Reducer ` in our project we create a single file named ` PreferencesActions `.
    ```js
    export const enableNotifications = () => {
        return {
            type: "ENABLE_NOTIFICATIONS"
        }
    }

    export const disableNotifications = () => {
        return {
            type: "DISABLE_NOTIFICATIONS"
        }
    }
    ```  
    Note that we defined a ` function ` for each ` action ` to return an ` action ` object to be **dispatched** where we need. Note also that we **exported** each ` action ` to be **imported** soon.
* Now our ` index.js ` becomes as simple as this:
```js
import React from 'react';
import { render } from 'react-dom';
import { Provider } from 'react-redux';
import App from './containers/App';
import Store from "./Store"

render(
  <Provider store={ Store }><App/></Provider>,
   window.document.getElementById("app")
);
```
* Now we also can change our ` mapDispatchToProps ` ` function ` to use our ` actions `.
```js
const mapDispatchToProps = ( dispatch ) => {
    return {
        ENABLE_NOTIFICATIONS: () => {
            dispatch( enableNotifications() );
        },
        DISABLE_NOTIFICATIONS: () => {
            dispatch( disableNotifications() );
        }
    };
};
```
Note that we need to ` import ` these ` actions `.
```js
import { enableNotifications, disableNotifications } from '../actions/PreferencesActions'
```  
