# Containers and Components(Smart and Dumb Components)
* In Apps that connect ` ReactJS ` and ` Redux `, we have a popular pattern where we divide the **components** into 2 categories:
    * **Containers** or **smart components**  
    These are the **components** that are connected and affected by ` Redux `(that's why they are named **smart components**. They know what others do not know by being connected to ` Redux `) and they hold inside the next category **components**(That's why they are called **containers**).
    * **Components** or **presentation components** or **dumb components**  
    These are **components** that are not connected and affected by ` Redux `. They live inside **containers** and they receive any data needed from these **containers**. That's why they are called **presentation** or **deaf components**.
* It's a good practice to categorize your **components** and divide them in 2 directories named **Containers** and **Components** both reside in **app** directory(**Containers** and **Components** directories are siblings to each other and children of **app** directory).
* So let's know follow this pattern and modify our last example.
    1. Instead of connecting ` AdjustPreferences ` and ` ShowPreferences ` **components** to ` Redux `, we will just register or ` connect ` ` App ` **component** and let it be our own **Container** and let ` AdjustPreferences ` and ` ShowPreferences ` receive data from ` App ` and by that they are **components**.
        * ` App `  
        We will copy the stuff related to ` Redux ` which are (` mapStateToProps ` method, ` mapDispatchToProps ` method, and ` connect ` method) from ` AdjustPreferences ` and ` ShowPreferences ` to ` App `. Then we will pass the ` state ` attributes and ` actions ` needed by each ` Component ` to it as a ` prop `.
        ```
        import React from 'react';
        import { AdjustPreferences } from '../components/AdjustPreferences';
        import { ShowPreferences } from '../components/ShowPreferences';
        import { connect } from 'react-redux';
        class App extends React.Component {
            render() {
                return(
                    <div>
                      <AdjustPreferences
                      notifications = { this.props.notifications }
                      DISABLE_NOTIFICATIONS = { this.props.DISABLE_NOTIFICATIONS }
                      ENABLE_NOTIFICATIONS = { this.props.ENABLE_NOTIFICATIONS }
                      />
                      <hr/>
                      <ShowPreferences notifications = { this.props.notifications }/>
                    </div>
                );
            }
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
        export default connect( mapStateToProps, mapDispatchToProps )( App );
        ```
        Note that if ` App ` is defined as a **stateless** ` Component `(a ` function ` not a ` class ` that ` extends ` ` React.Component `) it will work fine.
        * ` AdjustPreferences `
        ```
        import React from 'react';
        export const AdjustPreferences = ( props ) => {
            return(
                <div>
                  <h2>Adjust Preferences</h2>
                  <button onClick={ (props.notifications)? props.DISABLE_NOTIFICATIONS: props.ENABLE_NOTIFICATIONS }>{ (props.notifications)? "Disable": "Enable" } Notifications</button>
                </div>
            );
        };
        ```
        * ` ShowPreferences `
        ```
        import React from 'react';
        export const ShowPreferences = ( props ) => {
            return(
                <div>
                  <h2>Current Preferences</h2>
                  <p>Notifications: { ( props.notifications )? "Enabled": "Disabled" }</p>
                </div>
            );
        };
        ```
        Note that we moved **components** and **containers** in right directories. Now in ` index.js ` we need to update the ` import ` statement of ` App `.
        ```
        import App from './containers/App';
        ```
