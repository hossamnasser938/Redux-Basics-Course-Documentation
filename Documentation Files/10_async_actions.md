# Async Actions
* Not all ` actions ` we use are **synchronous**. Sometimes we need to get data from **Api** or **Database** which means **asynchronous** ` actions `.
* We have many ways to handle **asynchronous** ` actions ` using ` Redux `. Here we mention 2 of those:
    1. Let's use ` setTimeout ` to simulate our **asynchronous** ` action `. In ` PreferencesActions ` we will modify the ` enableNotifications ` action to be **asynchronous** using ` setTimeout `.
    ```js
    export const enableNotifications = () => {
        return ( dispatch ) => {
            setTimeout( () => {
                dispatch( {
                    type: "ENABLE_NOTIFICATIONS"
                } );
            } , 3000 );
        }
    }
    ```
    Note here that instead of returning an object we returned another ` function ` that accepts ` dispatch ` ` function ` and call ` dispatch ` in the callback of ` setTimeout ` ` function ` after 3 seconds.
    Now if we run our application we get an error ` Error: Actions must be plain objects. Use custom middleware for async actions `. So now we need a third-party **middleware** to do that for us. We can use ` redux-thunk ` which we install by hitting ` npm install redux-thunk --save `. Then we just need to pass that **middleware** to our ` Store `.
    ```js
    import { createStore, applyMiddleware } from 'redux';
    import { createLogger } from 'redux-logger';
    import preferencesReducer from './reducers/PreferencesReducer';
    import thunk from 'redux-thunk';

    const Store = createStore(
        preferencesReducer,
        {},
        applyMiddleware( createLogger(), thunk )
    );

    export default Store;
    ```
    However this works I wondered why we need this **middleware** (` redux-thunk `)? What if we handled that ourselves in ` App ` when we ` dispatch ` the ` action `. I mean that we defined our ` action ` to return a ` function ` that accepts ` dispatch ` ` function ` and it calls ` setTimeout ` and when that ` setTimeout ` ` function ` returns in the callback we ` dispatch ` the ` action ` so why not in our ` App ` we do that:
    ```js
    const mapDispatchToProps = ( dispatch ) => {
        return {
            ENABLE_NOTIFICATIONS: () => {
                enableNotifications()( dspatch );
            },
            DISABLE_NOTIFICATIONS: () => {
                dispatch( disableNotifications() );
            }
        };
    };
    ```
    instead of:
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
    Now no need for a third-party and It works. I think the reason wht we do not use this, is to keep ` dispatching ` ` events ` in a **standard** way.  

    2. The second approach is in our ` action ` we return an action object but instead of returning the action ` payload ` as a value immediately we return a ` Promise ` and let that handled by another third-party middleware which called ` redux-promise-middleware `. We ` import ` that **middleware**:
    ```js
    import promise from 'redux-promise-middleware';
    ```
    and we pass that **middleware** to our ` Store `:
    ```js
    const Store = createStore(
        preferencesReducer,
        {},
        applyMiddleware( createLogger(), promise )
    );
    ```
    Since our ` action ` in this case does not require ` payload `, we will not use this type. However, assuming that it does, it will be like this:
    ```js
    export const enableNotifications = () => {
        return {
            type: "DISABLE_NOTIFICATIONS",
            payload: new Promise( ( resolve ) => {
                setTimeout( () => {
                    resolve( "value" );
                } , 3000 );
            } )
        };
    }
    ```
    This type requires us to append this word(` _FULFILLED `) to ` action ` name. So in our ` Reducer ` we change the name of our ` action ` from ` ENABLE_NOTIFICATIONS ` to ` ENABLE_NOTIFICATIONS_FULFILLED `  to be handled correctly by that **middleware**. Note that in **dispatching** the ` action ` we do not change its name. This is done by the **middleware**.
