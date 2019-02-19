# Redux Middleware
* A ` Middleware ` is a layer that **receives** the ` Action ` before it **reaches** to the ` Reducer ` that is intended to respond it. You may use it to **log** the ` Action `, make a kind of **transformation** before passing it to the ` Reducer ` or whatever.
* To apply the ` Middleware ` you need to ` import ` the method ` applyMiddleware ` from ` redux `.
```
import { createStore, combineReducers, applyMiddleware };
```   
* ` applyMiddleware ` accepts a ` function ` defined to work as a ` Middleware `. This ` function ` accepts our ` store ` as an argument(May be because it needs to forward the ` Action ` to it after doing its operation) and returns another ` function ` that accepts ` next ` method which is used to forward the ` Action ` after the operation. This nestaed ` function ` returns another ` function ` that accepts lastely the ` Action `and does whatever it does and last you must call ` next( action ); `.
```
const myMiddleware = function( Store ) {
    return function( next ) {
        return function( action ) {
            console.log( "Action", action );
            next( action );
        }
    }
}
```
This can be less using **arrow functions**.
```
const myMiddleware = ( Store ) => ( next ) => ( action ) => {
    console.log( "Action", action );
    next( action );
};
```
* After that we pass it to ` applyMiddleware ` and pass the result to ` createStore ` method.
```
const Store = createStore(
    combineReducers( { MathReducer, UserReducer } ),
    {},
    applyMiddleware( myMiddleware )
);
```
Recall that ` myMiddleware ` ` function ` must be **defined** before **invoking** ` createStore ` ` function ` since ` createStore ` accepts it as an argument.
* Usually we're not defining our own ` Middleware `. Instead we use one from a third party. As an example we can use ` redux-logger ` . ` redux-logger ` creates beatiful logs with more information: previous state, action, and next state. ` redux-logger ` can be **imported**:
```
import logger from 'redux-logger';
```
of-course you need to install ` redux-logger ` before by hitting ` npm install redux-logger --save `. However, we use this for development purpose the instructor installed it as a production dependency using ` --save `.
Now we can use this ` logger `.
```
const Store = createStore(
    combineReducers( { MathReducer, UserReducer } ),
    {},
    applyMiddleware( logger() )
);
```  
Hint ` redux-logger ` has been changed recently in a later version. To import it use
```
import { createLogger } from 'redux-logger';
```
I think it makes sense now why we invoke ` logger ` or ` makeLogger ` rather than we pass it as a reference. ` createLogger `, as the name suggests, returns a ` logger ` for us.
