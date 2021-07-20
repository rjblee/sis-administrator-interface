<template>
  <div id="app">
    <keep-alive v-if="loggedIn">
      <router-view></router-view>
    </keep-alive>
    <router-view v-else></router-view>
  </div>
</template>

<script>
//import {store} from "./store/Store"
//import routes from './util/Routes.js'
import { mapState } from "vuex";
export default {
  name: "App",
  computed: mapState({
    loggedIn: (state) => state.loggedIn,
  }),
};
/*window.onbeforeunload=function(){
  console.log("On or before unload")
  routes.push({name:"Login"})
console.log("On or before unload")

}*/ //?This function will be called before the user is logged out and if working should reset everything and send them back to the default page
</script>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
