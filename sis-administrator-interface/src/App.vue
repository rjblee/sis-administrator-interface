<template>
  <div id="app">
    <div>
      <b-button v-b-toggle.sidebar-no-header>Open Sidebar</b-button>
      <b-sidebar
        id="sidebar-no-header"
        aria-labelledby="sidebar-no-header-title"
        no-header
        shadow
      >
        <template #default="{ hide }">
          <div class="p-3">
            <h4 id="sidebar-no-header-title">Swift Learning</h4>
            <img src="./assets/Swift Learning Logo.png" alt="" />
            <nav class="mb-3">
              <b-nav vertical>
                <b-nav-item active @click="hide">Dashboard</b-nav-item>
                <b-nav-item href="#link-1" @click="hide"
                  >Course Reports</b-nav-item
                >
                <b-nav-item href="#link-2" @click="hide"
                  >Registration Reports</b-nav-item
                >
              </b-nav>
            </nav>
            <b-button variant="primary" block @click="hide"
              >Close Sidebar</b-button
            >
          </div>
        </template>
      </b-sidebar>
    </div>
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
