# Using Redux
* Now let's convert it from **theory** to **action**.
* ` Redux ` does not necessarily work with ` ReactJS `. Actually it works with other frameworks and it works by itself as well.
* Now we will leave ` ReactJS ` away and see how ` Redux ` works and then in the future we're gonna combine ` ReactJS ` and ` Redux ` to see how they work together.
* We start by creating a project and setup **WebPack** environment as we did in ` ReactJS Basics `  tutorial. Then we need to install ` Redux ` by hitting: ` npm install redux --save `.
* Now we will write a very basic script that shows the constructs of ` Redux ` in a very basic way.
    1. We need to import only one method from ` redux ` to help us create ` Store `.
    ```js
    import { createStore } from 'redux';
    ```
    2. We define our ` Reducer ` and specify two simple ` actions `. ` Reducer ` is a method that:
        * **accepts**:
            1. ` state ` which is the current application state.
            2. ` action ` which is the ` action ` to be performed represented in an object with two attributes: ` type ` which indicates what type of ` action ` it is, and ` payload ` which is any data to be used with the ` action `. These names are for you. However, these names are **conventions** and when you use third-party libraries they will expect you to use these conventions.
        * **returns** the new ` state ` that reflects the ` action ` performed.
    ```js
    const reducer = ( state, action ) => {
        let copiedState = state;
        switch( action.type ) {
            case "ADD":
                copiedState += action.payload;
                break;
            case "SUBTRACT":
                copiedState -= action.payload;
                break;
        }
        return copiedState;
    };
    ```
    Note that what we did is considered an **immutable way** of updating ` state ` only because our ` state ` here is a **primitive** type. However, if our ` state ` is an object we need to do it in another way as we will see soon.

    3. We create ` Store ` by invoking ` createStore ` method which:
        * **accepts**:
            1. ` Reducer ` to be used to respond to actions.
            2. optional initial ` state `.
            3. optional **middleware**(will be discussed soon).
        * **returns**: our application ` Store `.
        ```js
        const initialState = 0;
        const store = createStore( reducer, initialState );
        ```  
    4. We ` subscribe ` to ` Store ` to be notified with new ` state `. We do that by invoking ` subscribe ` method on ` Store ` which accepts a ` function ` to be called when a new ` state ` comes to ` Store`. Inside this ` function ` we need to log the new ` state ` by calling the ` function ` ` getState ` on ` Store`. [This part will be handled by ` ReactJS ` when ` Redux ` is combined with ` ReactJS ` soon].  
    ```js
    store.subscribe( () => {
        console.log( "New state", store.getState() );
    } );
    ```
    5. Now we can ` dispatch ` ` actions ` by calling the  ` function ` ` dispatch ` on ` Store ` which will send this ` action ` to ` Reducer `. ` dispatch ` method accepts an object representing the ` action ` to be dispatched.
    ```js
    store.dispatch( {
        type: "ADD",
        payload: 20  
    } );
    store.dispatch( {
        type: "SUBTRACT",
        payload: 5  
    } );
    ```
    Now we get this **output**:  
    *New state 20  
    New state 15*  
    Which represents our ` dispatched ` ` actions `.
