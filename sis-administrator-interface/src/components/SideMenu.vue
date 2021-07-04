<template>
  <div>
    <b-button v-b-toggle.sidebar-no-header>Open Sidebar</b-button>
    <b-sidebar
      id="sidebar-no-header"
      aria-labelledby="sidebar-no-header-title"
      :backdrop-variant="light"
      backdrop
      no-header
      shadow
    >
      <template #default="{ hide }">
        <div class="p-3">
          <img
            class="side-menu-logo"
            src="../assets/Swift Learning Logo.png"
            alt="Swift Learning"
            height="100px"
          />
          <nav class="mb-3">
            <b-nav vertical>
              <h5>Dashboard</h5>
              <b-nav-item active @click="hide">Home</b-nav-item>
              <hr />

              <h5>Reports</h5>
              <b-nav-item href="../pages/CourseReports.vue" @click="hide">
                Course Reports
              </b-nav-item>
              <b-nav-item href="#link-2" @click="hide">
                Registration Reports
              </b-nav-item>
              <b-nav-item href="#link-2" @click="hide">
                Contact Reports
              </b-nav-item>
              <hr />

              <h5>Registration</h5>
              <b-nav-item href="#link-1" @click="hide">
                Global Admin
              </b-nav-item>
              <b-nav-item href="#link-2" @click="hide">
                Primary Admin
              </b-nav-item>
              <b-nav-item href="#link-2" @click="hide">
                Teachers
              </b-nav-item>
              <b-nav-item href="#link-2" @click="hide">
                Divisions
              </b-nav-item>
              <hr />

              <h5>Course Access</h5>
              <b-nav-item href="#link-1" @click="hide">
                Assign Course Access
              </b-nav-item>
            </b-nav>
          </nav>
          <b-button variant="primary" block @click="hide">Close Menu</b-button>
        </div>
      </template>
    </b-sidebar>
  </div>
</template>

<script>
export default {
  name: "SideMenu",
  data() {
    return {
      variant: "dark",
    };
  },
};
</script>

<style scoped>
.side-menu-logo {
}
</style>
