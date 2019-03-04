# Connect ReactJS and Redux
* Now we know how to use ` Redux ` as a standalone tool, let's connect it with ` ReactJS ` to achieve our goal of **abstracting** *state change* and *communication* among **components**.
* To do that we need to install a package named ` react-redux ` which simplifies this connection. To install it hit ` npm install react-redux --save `.
* We will build a very simple application that shows how ` ReactJS ` and ` Redux ` work together. This application holds 3 **Components**:
    1. ` App ` which will be the root ` Component ` of our application and will wrap the other 2 **components**.
    2. ` AdjustPreferences ` is a simple ` Component ` that enables changing the **preferences** of the user and for now the preferences is only enabling/disabling notifications.
    3. ` ShowPreferences ` is a simple ` Component ` that shows the user preferences.
* In this app we will have a ` button ` in ` AdjustPreferences ` ` Component ` that enables/disables notifications based on its current state(enabled or disabled). In ` ShowPreferences ` we will have a simple ` div ` that shows whether notifications is enabled or disabled.
* Let's start:
    1. In ` index.js ` we will define our ` Redux ` stuff such as ` Reducer `, ` Store ` and ` render ` our **root** ` Component ` which is ` App `.
    ```js
    import React from 'react';
    import { render } from 'react-dom';
    import { createStore, applyMiddleware } from 'redux';
    import { Provider } from 'react-redux';
    import { createLogger } from 'redux-logger';
    import { App } from './components/App';

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

    const Store = createStore(
        preferencesReducer,
        {},
        applyMiddleware( createLogger() )
    );

    render(
      <Provider store={ Store }><App/></Provider>,
       window.document.getElementById("app")
    );
    ```
    * **Notes**:  
      * ` Reducer `, and ` Store ` do not have something new. We just defined two ` actions ` one for enabling notifications and the other for disabling it. We set the initial state of notifications to disabled. We passed this ` Reducer ` and the ` redux-logger ` to ` createStore ` to create our ` Store `.
      * The new thing appears in ` render ` method. We wrapped the ` App ` ` Component ` in ` <Provider> ` ` Component ` and passed our ` Store ` as a ` prop `. This part registers our application to receive ` state ` from ` Store `. Note that this does not mean that all **components** in our application will receive ` state ` by its own. You need to specify which **components** need ` state ` and which ` dispatch ` ` actions ` because not all **components** necessarily do.
      * Note that no need to ` subscribe ` to the ` Store ` now. This is done for us by ` react-redux `.

    2. In ` App ` ` Component ` we will just ` render ` a **JSX** that wraps both ` AdjustPreferences ` ` Component ` and ` ShowPreferences  `` Component `.
    ```js
    import React from 'react';
    import AdjustPreferences from './AdjustPreferences';
    import ShowPreferences from './ShowPreferences';
    export const App = ( props ) => {
        return(
            <div>
              <AdjustPreferences/>
              <hr/>
              <ShowPreferences/>
            </div>
        );
    };
    ```
    * **Notes**: 
      * The way we ` import ` the **components** ` AdjustPreferences ` and ` ShowPreferences ` is a bit strange. We used to do it like that 
      ```js
      import { ShowPreferences } from './ShowPreferences' 
      ```
      This strange new syntax:
      ```js
      import ShowPreferences from './ShowPreferences';
      ```
      is used with ` default ` ` export ` and we will see this ` default ` ` export ` in both **components**.

    3. In ` AdjustPreferences ` ` Component ` we need to do 2 things related to ` Redux `:
        * We need to get the ` notifications ` attribute value from ` state ` object to configure the ` button ` well. If ` notifications ` is ` true ` we make the button ` text ` and ` onClick ` to disable notifications. If it is ` false ` we set its ` text ` and ` onClick ` to enable notifications. For that we need to get the ` state ` as well as the ` actions ` to ` dispatch `. 
          * To get the ` state ` we define a method named(for convention only) ` mapStateToProps ` which, as the names suggests, maps what we need from the global ` state ` to attributes in the ` props ` of this ` Component `. ` mapStateToProps ` accepts the global ` state ` object and returns an object whose keys are ` props ` attributes and values are ` state ` attributes. 
          *  To get a reference to ` actions ` that we can ` dispatch ` we define another method named ` mapDispatchToProps ` which also, as the name suggests, maps ` actions ` that can be dispatched to attributes in the ` props ` that we can pass as a reference anywhere including ` onClick ` method of the ` button `. ` mapDispatchToProps ` accepts a reference to ` dispatch ` ` function ` which we use to ` dispatch ` ` actions ` and returns an object whose keys are attributes of ` props ` and values are functions that inside call ` dispatch ` actions with a suitable ` action ` object.        
        * By that we defined all the things that we need to ` connect ` this ` Component ` with ` Redux ` so the next step is to ` connect ` it using ` connect ` method which we ` import ` fom ` 'react-redux' `. ` connect ` method accepts ` mapStateToProps ` and ` mapDispatchToProps ` **functions** that we have just defined. ` connect ` method return us a method that we can call by passing it the ` Component ` we need to wire up with ` Redux `. And last we need to ` default ` ` export ` the result which is the **wired-up** ` Component `.
    ```js
    import React from 'react';
    import { connect } from 'react-redux';

    const AdjustPreferences = ( props ) => {
        return(
            <div>
              <h2>Adjust Preferences</h2>
              <button 
                onClick={ (props.notifications)? 
                  props.DISABLE_NOTIFICATIONS: 
                  props.ENABLE_NOTIFICATIONS }>
                { (props.notifications)? "Disable": "Enable" } Notifications
              </button>
            </div>
        );
    };

    const mapStateToProps = ( state ) => {
        return {
            notifications: state.notifications
        };
    };

    const mapDispatchToProps = ( dispatch ) => {
        return {
            ENABLE_NOTIFICATIONS: () => {
                dispatch( {
                    type: "ENABLE_NOTIFICATIONS"
                } );
            },
            DISABLE_NOTIFICATIONS: () => {
                dispatch( {
                  type: "DISABLE_NOTIFICATIONS"
                } );
            }
        };
    };

    export default connect( mapStateToProps, mapDispatchToProps )( AdjustPreferences );
    ```
    4. In ` Show ` ` Component ` we just need to access the attribute ` notifications ` value in the global ` state ` object which we can do exactly as we did in ` AdjustPreferences `. However, here we do not need access to ` actions ` so we define ` mapDispatchToProps ` to return an empty object.
    ```js
    import React from 'react';
    import { connect } from 'react-redux';

    const ShowPreferences = ( props ) => {
        return(
            <div>
              <h2>Current Preferences</h2>
              <p>Notifications: { ( props.notifications )? "Enabled": "Disabled" }</p>
            </div>
        );
    };

    const mapStateToProps = ( state ) => {
        return {
            notifications: state.notifications
        }
    };
    
    export default connect( mapStateToProps, () => { return {} } )( ShowPreferences );
    ```    
