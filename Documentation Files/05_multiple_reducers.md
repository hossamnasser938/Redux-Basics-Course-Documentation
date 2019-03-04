# Multiple Reducers
* In a practical application,  its global ` state ` is a big object and the application has so many ` actions ` that update this ` state `. Certainly it is not efficient to handle all these ` actions ` in one ` Reducer`. So now it's time to see how to use **multiple** ` Reducers `.
* It's very simple. All you need to do is to define a ` function ` for each ` Reducer ` and ` import ` the method ` combineReducers ` from ` redux ` which, as the name suggests, is used to **combine** multiple ` Reducers ` together and pass the result to ` createStore ` method. ` combineReducers ` method accepts an object that wraps all ` Reducers ` as values for keys. [Note that you need to choose **unique** names for ` actions ` since we have multiple ` Reducers ` but only one ` Store ` that will receive the ` action ` and then it should decide which ` Reducer ` can handle that ` action ` from its name].
* Let's move our previous example to use **multiple** ` Reducers `.
    1. ` import ` the method ` combineReducers `.
    ```js
    import { createStore, combineReducers } from 'redux';
    ```
    2. Rename our ` initialState ` to ` MathInitialState `.
    ```js
    const MathInitialState = {
    ```
    3. Rename our ` Reducer ` to ` MathReducer `.
    ```js
    const MathReducer = ( state = MathInitialState, action ) => {
    ```
    4. let's define another initial ` state ` for another ` Reducer `.
    ```js
    const UserInitialState = {
        name: "Hossam",
        age: 22
    };
    ```
    5. Let's define another ` Reducer `.
    ```js
    const UserReducer = ( state = UserInitialState, action ) => {
        let newState = {
            ...state
        }
        switch( action.type ) {
            case "UPDATE_NAME":
                newState.name = action.payload;
                break;
            case "UPDATE_AGE":
                newState.age = action.payload;
                break;
        }
        return newState;
    }
    ```
    6. let's **combine** both ` Reducers `.
    ```js
    const Store = createStore( combineReducers( {
       "MathReducer": MathReducer,
        "UserReducer": UserReducer
      })
    );
    ```
    ES6 has a good feature that lets you pass only values in an object and it will create keys of the same name as the values.
    ```js
    const Store = createStore( combineReducers( { MathReducer, UserReducer } ) );
    ```
    7. Now we can ` dispatch ` ` actions ` related to both ` Reducers `.
    ```js
    Store.dispatch( {
        type: "ADD",
        payload: 4
    } );
    Store.dispatch( {
        type: "ADD",
        payload: 9
    } );
    Store.dispatch( {
        type: "UPDATE_NAME",
        payload: "Ismail"
    } );
    Store.dispatch( {
        type: "UPDATE_AGE",
        payload: "24"
    } );
    ```
