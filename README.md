# RNRF-example
A comprehensive RNRF example

![Demo](https://github.com/hellomaya/RNRF-example/blob/master/example.gif)

RNRF (react-native-router-flux) is a good navigation component, but the documentation wasn't with details of complete usage.

Here is how to make a side-menu with tab along:

- Create a navigation drawer from 'react-native-drawer'

```jsx
import React, {Component} from 'react';
import PropTypes from 'prop-types';
import {
    View,
    Text
} from 'react-native';

import Drawer from 'react-native-drawer';
import {Actions, DefaultRenderer} from 'app-wjz-router-flux';

export default class NavigationDrawer extends Component {
    render(){
        const state = this.props.navigationState;
        const children = state.children;
        return (
            <Drawer
                ref="navigation"
                //initializeOpen={true}
                open={state.open}
                onOpen={()=>Actions.refresh({key:state.key, open: true})}
                onClose={()=>Actions.refresh({key:state.key, open: false})}
                type="displace"
                content={<SideMenu navigation={this.refs['navigation']} />}
                tapToClose={true}
                openDrawerOffset={0.3}
                panCloseMask={0.3}
                negotiatePan={true}
                tweenHandler={(ratio) => ({
                    main: { opacity:Math.max(0.54,1-ratio) }
                })}>
                <DefaultRenderer navigationState={children[0]} onNavigate={this.props.onNavigate} />
            </Drawer>
        );
    }
}

class SideMenu extends Component {

    static PropTypes = {
        navigation:PropTypes.object
    };

    static DefaultPropTypes = {

        navigation:null
    }

    render() {

        return (
            <View style={{flex: 1, marginTop: 55, backgroundColor: '#008000'}}>
                <Text>Hello world!!</Text>
            </View>
        );
    }
}
```

- Your app component and router will be :

```jsx
export default class Home extends Component {

    componentWillMount() {

        //DataTest.initClientOnly(10000)
        //DataTest.initData(50);
        //DataTest.cleanData();
    }

    render() {

        const reducerCreate = params => {
            const defaultReducer = Reducer(params);
            return (state, action) => {
                return defaultReducer(state, action);
            }
        };

        return (
            <Router
                createReducer={reducerCreate}
                sceneStyle={{backgroundColor: '#E0E0E0'}}
            >
                    {require('./HomeTabbar')}
            </Router>
        )
    }
}
```

- Then ```HomeTabbar.js```

```jsx
import React from 'react';
import {
    StyleSheet
} from 'react-native';
import {
    Scene,
    Router,
    TabBar,
    Modal,
    Schema,
    Actions,
    Reducer,
    ActionConst
} from 'app-wjz-router-flux';

let Styles = StyleSheet.create({
    tabBarStyle: {
        backgroundColor: '#ECECEC',
        opacity: 1
    },
    tabBarIconContainerStyle: {
        opacity: 1
    }
});

import NavigationDrawer from './NavigationDrawer';

module.exports =
        <Scene key="drawer"
               component={NavigationDrawer}
               open={false}
               hideNavBar={true}>
            <Scene key='tabbar'
                   tabs={true}
                //hideNavBar={true}
                   tabBarStyle={Styles.tabBarStyle}
                   tabBarIconContainerStyle={Styles.tabBarIconContainerStyle}>

// all of those are tabs
// for each of them, owned a navigation stack

                {require('./zscene_tab/...')}
                {require('./zscene_tab/...')}

            </Scene>


// all of those views are not inside tab
// you can put them into the navigation stack
// but the scene will be show like modal

            {require('./zscene_tab/...')}
            </Scene>

```

- A example about Tab scene:

```jsx
module.exports =
    <Scene key='tabOrder'
           initial={true}
           title={LocaleString.Order}
           type={ActionConst.REFRESH}
           icon={TabIcon}
    >
        <Scene key='orderList'
               hideNavBar={true}
               component={OrderView}
               title={LocaleString.Orders}
        >
        </Scene>

// put all views in the same tab there,
// perhaps they are in the same navigation stack
// those scenes will be show inside tab-bar

        {require('./....')}
        {require('./....')}
        {require('./....')}
    </Scene>;
```

Then use this to hide/show menu

```jsx
// then you could open/hide/toggle drawer anywhere using 'refresh' modifiers:
Actions.refresh({key: 'drawer', open: value => !value });
```


