# Working with State and Immutability
* Now let's modify our example and use a ` state ` as an object not a **primitive** ` Number ` value since in a practical application ` state ` is an object that may has nested objects or arrays.
* Let's update our initial ` state ` to be an object with 2 attributes: ` val ` which will be the same value we used in the previous example, and ` history ` which will be an array to hold previous ` states ` values.
```
const initialState = {
    val: 0,
    history: []
};
```
* Recall that when we had a **primitive-type** ` state ` in the previous example we could make a copy of it like this ` let copiedState = state; `. Since it is a primitive-type those two variables will have different locations in memory and for that reason that was an **immutable** way of modifying ` state ` because any change on the copy does not affect the original ` state `. However, in case of ` state ` as an object this ` let copiedState = state; ` is not considered an **immutable** way since ` state ` is now just a **reference** to where ` state ` object resides in memory and by assigning this **reference** to another variable, both variables will refer to the same location and by updating any property in the object using any reference of the two the other will be affected since both of them reference the same location.
* So we need an **immutable** way that works with objects. Actually there are more than one. One of them is using the **spread syntax** introduced in **ES6**.
```
let copiedState = {
    ...state
};
```
However, if we have an attribute in ` state ` object that is assigned an object too(so object inside an object) like our case the ` history ` attribute is assigned an array which is an object, in this case we need also to use **spreed syntax** to copy this object.
```
let copiedState = {
    ...state,
    history: [...state.history]
};
```
You might ask: ` ...state ` copied the attribute ` history ` with its value, so how we assign it again? Actually this is handled by assigning it the last value which is what we need.
* After all of that you might ask why we need to do that in an **immutable** way? Actually until now I have no adequate answer but here is what I know:
    1. It is a **functional programming concept**: *Avoiding state change and mutable data*.
    2. One of the main functions of ` Store ` in ` Redux ` is to **store** or **keep track** of different versions of ` state ` during application running. Doing that in a **mutable** way makes it only a single ` state `.
    3. Doing that in an **immutable** way allows knowing where you are when you need to.
* Enough talking let's use an object for ` state `.
    1. We've done the first step already by defining the initial ` state`.
    2. We need to modify the ` Reducer `.
    ```
    const reducer = ( state = initialState, action ) {
        let newState = {
            ...state,
            history: [...state.history]
        }
        switch( action.type ) {
            case "ADD":
                newState.val += action.payload;
                newState.history.push( action.payload );
                /* Actually pushing like this is not an immutable way but since we have a copy of the array that does not lead to a problem */
                break;
            case "ADD":
                newState.val -= action.payload;
                newState.history.push( action.payload );
                break;
        }
        return newState;
    };
    ```
    Notice here we used **default args** which is a feature introduced in **ES6** so that when no ` state ` is passed to ` Store `, it uses the initial ` state `.
    3. Now no need to pass ` createStore ` the initial ` state ` since we used the **default args**.
    ```
    const store = createStore( reducer );
    ```     
