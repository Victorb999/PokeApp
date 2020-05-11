<template>
    <view class="container">
      <app />
    </view>
</template>


<script>
import Vue from "vue-native-core";
import { VueNativeBase } from "native-base";
import axios from 'axios';
import App from "./index.vue";

// registering all native-base components to the global scope of the Vue
Vue.use(VueNativeBase);
Vue.prototype.$http = axios

export default {
  components: { App }
};
</script>

<style>
.container {
  flex: 1;
}

</style>