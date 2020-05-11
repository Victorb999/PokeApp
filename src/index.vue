<template>
    <root>
      <app-navigation></app-navigation>
    </root>
</template>


<script>
import React from "react";
import { StackNavigator, DrawerNavigator, navigationService } from "vue-native-router";
import { Root } from "native-base";

import Sidebar from "./screen/sidebar.vue";
import HomeScreen from "./screen/home.vue";


const Drawer = DrawerNavigator(
  {
    Home: { screen: HomeScreen },

  },
  {
    initialRouteName: "Home",
     contentComponent: props => <Sidebar {...props} />
  }
);

const AppNavigation = StackNavigator(
  {
    Drawer: { screen: Drawer }
  },
  {
    initialRouteName: "Drawer",
    headerMode: "none"
  }
);
export default {
  components: { Root, AppNavigation }
};
</script> 