<template>
  <nb-container>
    <nb-content :style="stylesObj.drawerContent" :bounces="false">
      <nb-list>
        <nb-list-item>
          <nb-text>FODAS</nb-text>
        </nb-list-item>
        <nb-list-item>
          <nb-text >Logout</nb-text>
        </nb-list-item>
      </nb-list>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform } from "react-native";
import { NavigationActions } from 'vue-native-router';

const deviceHeight = Dimensions.get("window").height;
const deviceWidth = Dimensions.get("window").width;
const resetAction = NavigationActions.reset({
                        index: 0,
                        actions: [NavigationActions.navigate({routeName: "Home"})]
                    })
export default {
  props: {
    navigation: {
      type: Object
    }
  },  
  data: {
    stylesObj: {
      profilePic: {
            height: 40,
            width: 40,
            borderRadius: Platform.OS === "android" ? 40 : 20
        },
        drawerContent: {
            paddingTop: Platform.OS === "android" ? 20 : 30
        }
    },
  }
};
</script>